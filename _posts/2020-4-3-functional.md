---
layout: post
title: "함수형 프로그래밍 정리"
tags: [programming]
---
## 후기
함수형 프로그래밍을 제대로 이해하고 구현하는 건 꽤 오랜 시간이 필요하지 않을까 싶다.
그럼에도, 다형성과 추상화를 통해 함수를 합성하고, 간결하게 선언형으로 표현된 코드들이 너무 이쁘게 보였다.
시간을 들여 체득하고 싶은 마음이 불쑥불쑥 샘 솟는다.

<hr/>
<br/>

### 일급
 - 값으로 다룰 수 있다.
 - 변수에 담을 수 있다.
 - 함수의 인자로 사용될 수 있다.
 - 함수의 결과로 사용될 수 있다.
 
### 일급 함수
 - 함수를 값으로 다룰 수 있다.
 - 조합성과 추상화의 도구.
 
 
### 고차 함수
 - 함수를 값으로 다루는 함수
     - 함수를 인자로 받아서 실행하는 함수
     - 함수를 만들어 리턴하는 함수 (클로저를 만들어 리턴하는 함수)

 <br/>
 <hr/>
 <br/>
 
## ES6에서의 순회
 
 정통적으로 어떤 객체를 순회 할 때 index와 for 구문을 이용해 왔지만, ES6에서의 순회는 몇 가지 개념이 추가되어 개발자에게 새로운 규약을 제공하게 됨.  ex) for of, 전개 연산자, 구조 분해...
 
### 이터러블/이터레이터 프로토콜
 - 이터러블: 이터레이터를 리턴하는 Symbol.iterator 를 가진 값
 - 이터레이터: { value, done } 객체를 리턴하는 next() 를 가진 값
 - 이터러블/이터레이터 프로토콜: 이터러블을 for...of, 전개 연산자 등과 함께 동작하도록한 규약
 - todo: 사용자 정의 이터러블 만들어보기
 
### 제너레이터/이터레이터
 - 제너레이터: 이터레이터이자 이터러블을 생성하는 함수
 - 어떤 문장이든 순회가능한 이터러블로 만들 수 있다는 것에  큰 의의가 있음.
 
<br/>
<hr/>
<br/>

## 다형성 feat. map, filter, reducer

Array 내장 함수를 생각하면, 함수를 인자로 받아 그 함수에  순차적으로 배열의 값을 넣어 실행한다.<br/>
document.querySelector('*')이 반환하는 객체는 배열을 상속 한 것이 아니기 때문에 Array 내장 함수들을 사용할 수 없다.<br/>
하지만 이터러블 프로토콜을 따르기 때문에, 이터레이터를 이용해 함수를 새로 정의 한다면, 기존에 배열에 종속 되어 있는 함수와는 달리 모든 이터러블 객체에 사용할 수 있게 된다.

<br/>
<hr/>
<br/>

## go & pipe
{% highlight javascript %}
    const go = (...args) => reduce((a, f) => f(a), args);
    const pipe = (f, ...fs) => (...as) => go(f(...as), ...fs);
{% endhighlight %}
연속적으로 실행되는 함수들을 값으로 다루어 표현력을 높임!

## curry
{% highlight javascript %}
    const curry = f => (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);
{% endhighlight %}
함수를 값으로 다루면서 받아둔 함수를 원하는 시점에 평가함!
함수를 받아서 함수를 리턴하고, 원하는 숫자의 인자가 들어왔을 때 받아둔 함수를 평가함.

<br/>
<hr/>
<br/>

## 지연 평가
즉시 평가되는 map함수를 생각해보면, map이 실행되는 순간 결과를 담은 모든 값이 새로운 array 담겨 반환된다.
이에 비해 제너레이터 / 이터레이터를 이용해 map을 만든다면, 결과 값을 한 번에 반환하지 않고 이터레이터를 통해 필요한 순간에 평가를 할 수 있게 된다. 이런 지연을 통해 메모리 공간, 속도를 개선할 수 있다.


## map, filter 계열 함수들이 가지는 결합 법칙

- 사용하는 데이터가 무엇이든지
- 사용하는 보조 함수가 순수 함수라면 무엇이든지
- 아래와 같이 결합한다면 둘 다 결과가 같다.
    [[mapping, mapping], [filtering, filtering], [mapping, mapping]] = [[mapping, filtering, mapping], [mapping, filtering, mapping]]

## reduce, take
지연성 보다는 특정 지점에서 결과를 내고 함수를 끝내거나 다시 시작.

<br/>
<hr/>
<br/>

## Promise와 비동기
Promise는 비동기 상황이 값으로서 (일급으로) 다뤄진다는 점이 단순 callBack 형식과 비교할 때 가장 큰 차이.
일급으로 다뤄지기 때문에 지속적으로 어떤 일들을 연결해 나갈 수 있다.

## 모나드
대략적으로 정리해보자면, 모나드는 함수 합성을 안전하게 할 수 있는 도구이다.
{% highlight javascript %}

    const g = a => a + 1;
    const f = a => a * a;
    // log(f(g()));  -- NaN 발생!!!
    
    [].map(g).map(f).forEach(log); // 값이 잘못 되었어도 효과 처리
    
{% endhighlight %}
위의 예를 보면, 함수가 합성되기 전에 '배열'을 하나의 박스로 이용해, 함수 합성이 안전하게 진행되도록 한다.<br/>
프로미스를 모나드의 관점에서 보면, 비동기에 일어나는 상황(대기)을 프로미스라는 박스를 이용해, 함수 합성이 안전하게 진행되도록 만들 수 있다!

{% highlight javascript %}

    new Promise(resolve =>setTimeout(() => resolve(2), 100)).then(g).then(f)

{% endhighlight %}

## 클레이슬리 (Kleisli)
클레이슬리는 오류가 있을 수 있는 상황에서의 함수 합성을 안전하게 할 수 있는 하나의 규칙이다. <br/>
수학적인 프로그래밍은 같은 변수와 같은 함수가 만났을 때 정확한 결과를 기대하지만, 현대 프로그래밍에서는 변수의 상태나 의존하고 있는 외부 상황등에 따라 에러가 발생할 수 있다.<br/>
예를 들면 임의의 인덱스 값을 인수로 받아 배열에 접근하는 함수가 있을 때, 함수의 결과 값은 배열의 상태 (ex: Array.pop())에 따라 달라질 수 있다.

크레이슬리는 함수 합성시에 이런 위험성을 제거하기 위해 다음의 형태로 규칙을 적용한다.

```
f(g(x)) = f(g(x)) 와 같은 상황을 기대하지만,
g(x)에서 에러가 나타났을 때 f(g(x)) = g(x) 경우로 만든다.
```
<br/>
실제 예시:
{% highlight javascript %}

    var users = [
    {id: 1, name: 'aa'},
    {id: 2, name: 'bb'},
    {id: 3, name: 'cc'}
    ];

    const getUserById = id =>
    find(u => u.id == id, users) || Promise.reject('없어요!');

    const f = ({name}) => name;
    const g = getUserById;

    // const fg = id => f(g(id));

    const fg = id => Promise.resolve(id).then(g).then(f).catch(a => a);

    fg(2).then(log);

    setTimeout(function () {
    users.pop();
    users.pop();
    fg(2).then(log);
    }, 10);
    
{% endhighlight %}
<strong> reject와 catch를 통해 함수 합성 전과 후의 결과를 같게 만든 것을 볼 수 있다! </strong>


## go, pipe. reduce 비동기 제어
파이프에 동기 코드와 프로미스가 섞여 있다면, 프로미스 체인이 동기 코드까지 이어지지 않도록 해야한다.
쓸데 없는 로드를 막고 동기 코드는 하나의 콜스택에서 해결 되로록 하는 것이 효율적!

<br/>
<hr/>
<br/>

## async / await이 있는데 왜 pipe line 이 필요?
목적 자체가 다르다.<br/>
async / await은 then으로 이어지는 함수 체인을 문장형으로 만들기 위함이고 pipline은 비동기 처리를 수행하며 안전하게 함수 합성을 하기 위함이다.<br/>
+)Promise의 reject 평가가 적절한 곳에서 진행될 수 있어 에러 핸들링 하기도 훨씬 수월하다!


## Resource
[함수형 프로그래밍과 JavaScript ES6+ by 유인동님](https://www.inflearn.com/course/functional-es6/lecture/16643)


