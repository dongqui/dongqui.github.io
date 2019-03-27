---
layout: post
title: "React hooks 여행기"
tags: [web, react]
---
사이트 프로젝트에 Hooks를 적용 해보면서 살짝 당황했던 batching up 문제.

간단하게 예를 들면, api call을 할 때 데이터와 로딩 state 관리하게 되는데 아래와 같이 loading state, data state를 묵어서 set 할 수 있다.
{% highlight javascript %}
const foo = async () => {
    setState({...state, loading: false});
    const data = await apiCall();
    setState({...state, loading: true, data});
}
{% endhighlight %}

하지만, useState를 사용하면 아래와 같이 loading state, data state를 따로 set하게 된다.
{% highlight javascript %}
const foo = async () => {
    setLoading(true);
    const data = await apiCall();
    setLoading(false);
    setData(data);
}
{% endhighlight %}

이렇게 되면, 쓸 때 없이 렌더링이 더 일어나게 된다.
리엑트는 기본적으로 React-based event (버튼 클릭, 인풋 값 변경... 등)는 state를 묶어서 업데이트 해주지만, 리엑트 이벤트 핸들러 밖에서 실행되는 것들(setTimeout, api 요청.. 등)은 묶이지 않는다.

그래서 나는 처음에 loading state, data state를 합쳐서 관리하는 식으로 해봤다.
{% highlight javascript %}
const [{loading, data}, setData] = useState({loading: false, data:[]});
const foo = async () => {
    setData({loading: true, data:[]});
    const data = await apiCall();
    setData({loading: false, data});
}
{% endhighlight %}

문제는, data 이외의 다른 state 관리해야 할 때!
실제 사이트 프로젝트를 예로 들면, Restaurants 먼저 fetch한다. 여기 까지는 위의 코드로 잘 돌아간다.
그런데 Restaurant(단수) state를 


### Resource
[https://www.reddit.com/r/learnjavascript/comments/782qdx/what_does_void_elementoffsetwidth_do/](https://www.reddit.com/r/learnjavascript/comments/782qdx/what_does_void_elementoffsetwidth_do/)
