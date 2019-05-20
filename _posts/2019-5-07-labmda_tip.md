---
layout: post
title: "AWS lambda tip: cold start"
tags: [aws]
---

labmda가 요청을 받았을 때, 코드가 실행 될 컨테이너가 존재하지 않으면 컨테이너를 새로 생성 후 코드를 실행하게 되는데, 여기서 지연이 발생한다. <br/>
Cold start 발생 빈도수, 지연 시간은 각각의 상황에 따라 상이하다.<br/>
(VPC 설정 유무, memory, code size...) <br/>


사실상 굳이 신경쓰지 않아도 될 만큼의 지연이 발생하는 경우, 그냥 사용하면 된다.<br/>
<br/>
아래는 그런 경우가 아닐 때의 해결방법으로, AWS 시니어 개발자 [Chris Munns](https://twitter.com/chrismunns)의 발표 내용 요약본 중 생성된 람다 컨테이너를 계속 유지하는 Best practice에 관한 이야기다.<br/>
<br/>
기본적인 내용은, 한 번 생성된 컨테이너가 닫히지 않도록 CloudWatch를 이용해서 계속 핑을 하나씩 보내는 것이다. (다만, 이미 존재하는 Lambda 컨테이가 있더라도 요청이 그 컨테이너로 가는 것을 100% 보장하지는 않고, 그렇게 되도록 '노력'한다라고 aws측에서 이야기 함) <br/>
그리고 다음의 조건을 충족시킬 때 가장 좋다!
- 핑을 보낼 때 5분 보다 더 긴 간격으로 보낼 것
- API Gateaway 같은 거 거치지 말고 직행으로 Lambda를 실행시킬 것
- warming 요청인 것을 payload에서 확인 할 수 있을 것
- warming ping일 경우 본래의 람다 코드를 다 실행 시키지말것

### 출처: [https://www.jeremydaly.com/15-key-takeaways-from-the-serverless-talk-at-aws-startup-day/](https://www.jeremydaly.com/15-key-takeaways-from-the-serverless-talk-at-aws-startup-day/)
<br/>
<br/>

위에서 제시된 Best practice에 따라 만들어진 <strong>[Lambda-warmer](https://github.com/jeremydaly/lambda-warmer)</strong> 라이브러리가 있다.
{% highlight javascript %}
const warmer = require('lambda-warmer')

exports.handler = async (event) => {
  // if a warming event
  if (await warmer(event)) return 'warmed'
  // else proceed with handler logic
  return 'Hello from Lambda'
}
{% endhighlight %}
기본적인 사용법은 매우 간단하다.
핸들러 밖에서 lambda-warmer를 실행하고, 핸들러 가장 윗줄에 event의 payload를 검사해 warming ping인지 유저의 request인지 확인 후 예외 처리를 한다.

여기에 더해 lambda-warmer는 concurrency 기능도 지원한다.
{% include image.html path="postimages/lambda.001.jpeg" path-detail="postimages/lambda.001.jpeg" alt="" %}
{% include image.html path="postimages/lambda.002.jpeg" path-detail="postimages/lambda.002.jpeg" alt="" %}
{% include image.html path="postimages/lambda.003.jpeg" path-detail="postimages/lambda.003.jpeg" alt="" %}

lambda-warmer의 코드를 뜯어서 살펴보면, 
{% highlight javascript %}
// Fan out if concurrency is set higher than 1
    if (concurrency > 1 && !event[config.test]) {

      // init Lambda service
      let lambda = require('./lib/lambda-service')

      // init promise array
      let invocations = []

      // loop through concurrency count
      for (let i=2; i <= concurrency; i++) {

        // Set the params and wait for the final function to finish
        let params = {
          FunctionName: funcName,
          InvocationType: i === concurrency ? 'RequestResponse' : 'Event',
          LogType: 'None',
          Payload: new Buffer(JSON.stringify({
            [config.flag]: true, // send warmer flag
            '__WARMER_INVOCATION__': i, // send invocation number
            '__WARMER_CONCURRENCY__': concurrency, // send total concurrency
            '__WARMER_CORRELATIONID__': correlationId // send correlation id
          }))
        }

        // Add promise to invocations array
        invocations.push(lambda.invoke(params).promise())

      } // end for

      // Invoke concurrent functions
      return Promise.all(invocations)
        .then((res) => true)

    } else if (invokeCount > 1) {
      return delay(config.delay).then(() => true)
    }
{% endhighlight %}
n개의 concurrency를 보장하기 위해, delay()함수를 이용하는 것을 볼 수 있다. 여러 개의 lambda를 인보크 할 때, 이미 생성된 람다가 재실행 될 수 있기 때문이다.<br/>
<br/>
예를 들면 5개의 람다를 인보크 시켜야 하는데, 1번 람다가 생성 된 후, 2,3,4,5번의 람다가 생성되지 않고 계속 1번으로 요청이 들어가는 경우가 있을 수 있다.<br/>
그걸 방지하기 위해 나머지 2, 3, 4, 5번의 람다가 생성 될 때 까지 delay()를 사용해 바쁘게 굴려 다른 요청을 받지 못하게 한다. 

자세한 내용은 아래에!<br/>
[https://www.jeremydaly.com/lambda-warmer-optimize-aws-lambda-function-cold-starts/](https://www.jeremydaly.com/lambda-warmer-optimize-aws-lambda-function-cold-starts/)
