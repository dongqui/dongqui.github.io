---
layout: post
title: "자바스크립트 호이스트"
tags: javascript
---

<strong>모든 변수는 호이스팅 된다. 즉, 모든 변수는 어디에서 할당하고, 초기화 하든 스코프의 가장 위에서 선언된다.</strong>
<br><br>
예시로 살펴보자.

{% highlight javascript %}
function showName () {
console.log ("First Name: " + name);
​var name = "Ford";
​console.log ("Last Name: " + name);
​}
​
​showName ();
​​// First Name: undefined
​​​// Last Name: Ford
​​​​​​​​​​
​​​​​​​// undefined가 출력 될 수 있었던 이유는, name 변수가 아래에서 선언/초기화 됐지만 호이스팅 개념 덕에 맨 위에서 변수가 선언 됐기 때문
​​​​​​​// undefined는 변수는 있으나 값이 정해지지 않은 것을 뜻함에 주의!

​​​​​​​​​​​​​​​// 사실상 아래와 같이 동작하고 있다. 위에서 name변수만 먼저 선언됨!
​​​​​​​​​function showName () {
​​​​​​​​​var name;
​​​​​​​​​console.log ("First Name: " + name); // First Name: undefined
​​​​​​​​​​​​​​​​​​​
​​​​​​​​​​name = "Ford"; // name is assigned a value
​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​
​​​​​​​​​​​​console.log ("Last Name: " + name); // Last Name: Ford
​​​​​​​​​​​​}

{% endhighlight %}


<strong>Function Declaration Overrides Variable Declaration When Hoisted</strong><br>
함수 선언 역시 호이스팅된다. 그리고 함수의 선언이 변수 선언보다 먼저 오게 된다. <br>
변수 할당 호이스팅되지 않듯이, 함수 할당 역시 호이스팅 되지는 않음.

{% highlight javascript %}
// Both the variable and the function are named myName
​var myName; 
​function myName () {
​console.log ("Rich");
​}
​
​​// The function declaration overrides the variable name
​​console.log(typeof myName); // function
​​// But in this example, the variable assignment overrides the function declaration
​​​var myName = "Richard"; // This is the variable assignment (initialization) that overrides the function declaration.
​​​​​​​
​​​​​function myName () {
​​​​​console.log ("Rich");
​​​​​}
​​​​​​​​​​​
​​​​​​console.log(typeof myName); // string
{% endhighlight %}


<strong>아래와 같은 표현은 호이스팅되지 않는다!</strong>
{% highlight javascript %}
var myName = function () {
    console.log ("Rich");
}
{% endhighlight %}


'strict mode '에서는, 변수 선언 전에 할당하려고 하면 에러가 발생하므로, 변수 선언부터 하도록 하자!

## Resource
[http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/)
