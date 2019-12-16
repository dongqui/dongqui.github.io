---
layout: post
title: "Memoization"
tags: [알고리즘]

---
<strong>순환 기법을 사용 할 때 발생할 수 있는 비효율을 해소하기 위한 목적으로 Memoization이 이용된다.</strong><br><br>

<strong>Recursion을 설명 할 때 대표적으로 사용되는 예시인 피보나치</strong>
{% include image.html path="postimages/memoization_fibo.png" path-detail="postimages/memoization_fibo.png" alt="Uyuni" %}

여기서 문제는, 똑같은 결과 값을 얻는 데에도 다시 함수를 호출하는 비효율이 발생한다. <br>
( ex - 위 그림을 보면 fib(5)의 값을 구했더라도 fib(6) 값을 구하기 위해 fib(5)를 다시 호출 하고있다.)<br><br>


<strong>자바스크립트로 간단하게 구현해 본 memoize </strong>
{% highlight javascript %}
_.memoize = function(func) {
    var memoizedList = {};
    return function() {
        var strArguments = JSON.stringify(arguments)
        if (memoizedList.hasOwnProperty(strArguments)) {
        return memoizedList[strArguments];
        } else {
            memoizedList[strArguments] = func.apply(this, arguments);
            return memoizedList[strArguments];
        }
    };
};
{% endhighlight %}
원리는 간단하다.<br>
1.  먼저 함수 호출 후 결과 값을 담는 메모리스트를 오브젝트로 생성한다. 오브젝트의 키 값은 받으 인자값, 그리고 벨류 값은 결과 값을 담는다.
2. 함수가 Recursive하게 호출 될 때, 같은 인자 값을 받으면, 결과 값이 같게 된다. 따라서 함수를 호출 하기 전에 받은 인자 값으로 이전에 함수가 실행 된 적이 있는지 메모리스트를 통해 확인한다.
3. 같은 인자 값을 받은 적이 있으면 메모리스트에서 바로 리턴해주고, 처음 받는 인자 값이라면 함수를 호출 한 후 값을 반환하기 전에 메모리스트에 인자 값과 반환 값을 추가해준다.
4. 반복


### Resource
[https://www.inflearn.com/](https://www.inflearn.com/)<br>
[https://codestates.com/](https://codestates.com/)
