---
layout: post
title: "트릭 - CSS애니메이션 재실행"
tags: [web]
---
CSS 애니메이션을 재 실행하기 위해서 class를 지웠다가 다시 붙여주는 형식으로 코딩을 했는데, 생각대로 구현이 안됐다.
구글링을 하다가 클래스를 지우고 뒤에 ```void element.offsetWidth;``` 를 붙여주는 형식의 신기한 코드를 발견해서 도대체 무슨 일인지 알아보았다.

{% highlight javascript %}
if (element.classList.contains(className)) {
  element.classList.remove(className);
  void element.offsetWidth;
}
element.classList.add(className);
{% endhighlight %}

클래스를 붙이고, 지우는 일은 사실 페이지의 많은 부분에 영향을 줄 가능성이 크다.<br>
이런 변화를 바로 렌더링 하기에는 비용이 너무 크기 때문에, 똑똑한 브라우저는 이런 비용이 클 가능성이 높은 변화는 감지를 한 이후에 바로 적용하지 않고 일단 기를(?) 모은다.<br> 
<br>
만약 위의 코드에서 ```void element.offsetWidth;```가 없었다면 다음의 흐름으로 동작하게 된다.
1. 클래스가 존재하면 지운다
2. 브라우저는 변화를 감지하지만 바로 적용하지 않고 일단 넘어간다.
3. 클래스 다시 추가한다.
4. 모든 로직이 종료되고 보니 아무런 변화가 없다. 브라우저는 변화가 없으므로 렌더링을 하지 않는다.

여기서 클래스를 지운 후에 ```void element.offsetWidth;```를 넣어주면서 브라우저에 dom에 대한 정보(width)를 요청하면서 강제로 브라우저에게 일은 시킨다.<br>
모르는 척 하고 싶었던 브라우저는 시키니까 일단 계산을 진행하게 되고, 클래스를 지웠을 때의 로직 또한 바로 적용되게 된다.

사실, 이런 로직은 브라우저에게 잔업을 주는거나 마찬가지다.<br>
나한테 비상식적인 잔업을 주면 퍼포먼스가 격하게 떨어지는 것을 생각하며, 브라우저에게 잔업을 줄 때도 퍼포먼스에 유의하자.

### Resource
[https://www.reddit.com/r/learnjavascript/comments/782qdx/what_does_void_elementoffsetwidth_do/](https://www.reddit.com/r/learnjavascript/comments/782qdx/what_does_void_elementoffsetwidth_do/)