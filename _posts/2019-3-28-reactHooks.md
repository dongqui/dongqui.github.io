---
layout: post
title: "React hooks 여행기"
tags: [web, react]
---
사이트 프로젝트에 Hooks를 적용 해보면서 살짝 당황했던 batching up 문제.


간단하게 예를 들면, <br/>
api call을 할 때 데이터 스테이트와 로딩 스테이트 관리하게 되는데 Component 클래스에서 제공하는 setState를 아래와 같이 두 개의 스테이트를 묵어서 set 할 수 있다.
{% highlight javascript %}
const foo = async () => {
    setState({...state, loading: false});
    const data = await apiCall();
    setState({...state, loading: true, data});
}
{% endhighlight %}

하지만, useState를 사용하면 아래와 같이 loading state, data state를 따로 set하게 된다.
{% highlight javascript %}
const [data, setData] = useState([]);
const [loading, setLoading] = useState(false);
const foo = async () => {
    setLoading(true);
    const data = await apiCall();
    setLoading(false);
    setData(data);
}
{% endhighlight %}

이렇게 되면, 쓸 때 없이 렌더링이 더 일어나게 된다.<br/>
리엑트는 기본적으로 React-based event(버튼 클릭, 인풋 값 변경... 등)는 state를 묶어서 업데이트 해주지만, 리엑트 이벤트 핸들러 밖에서 실행되는 것들(setTimeout, api 요청.. 등)은 묶이지 않는다.

그래서 loading state, data state를 합쳐서 관리하는 식으로 해봤다.
{% highlight javascript %}
const [{loading, data}, setData] = useState({loading: false, data:[]});
const foo = async () => {
    setData({loading: true, data:[]});
    const data = await apiCall();
    setData({loading: false, data});
}
{% endhighlight %}

문제는, 부수적인 스테이트가 더 따라올 때...!<br/>
실제 사이트 프로젝트를 예로 들면, 
{% include image.html path="postimages/state_management.png" path-detail="postimages/state_management.png" alt="" %}
Restaurants 먼저 fetch한다. 여기 까지는 위의 코드로 잘 돌아간다.

Restaurant(단수) 스테이트를 set하는 경우에는?
{% include image.html path="postimages/state_management2.png" path-detail="postimages/state_management2.png" alt="" %}
여기 까지도 괜찮다

그런데 여기서 필터(한식, 중식, 일식) 버튼을 누르면, 필터링 된 restaurants를 다시 스테이트 값에 업데이트 시키고 restaurant는 초기화 시켜야 한다.
이렇게 되면, ```setRestaurants({loading: false, restaurants});``` 과정 이 후에 다시 ```setRestaurant(null);```...
다시 쓸 때 없는 렌더링 발생!

react에서는 이런 경우를 위해 useReducer를 제공한다.
>useReducer is usually preferable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one. useReducer also lets you optimize performance for components that trigger deep updates because you can pass dispatch down instead of callbacks.

	


### Resource
[https://www.reddit.com/r/learnjavascript/comments/782qdx/what_does_void_elementoffsetwidth_do/](https://www.reddit.com/r/learnjavascript/comments/782qdx/what_does_void_elementoffsetwidth_do/)
