---
layout: post
title: "Hooks useState - batch up문제"
tags: [react]
---
하나의 object로 묶어 setState하는 class 컴포넌트와는 달리, 간단하게 state를 한 개씩 다루는 useState에서 발생하는 batch up 문제 발견..!<br>
영 찝찝한 모양세라 콘솔에 찍어보니 역시나 불필요한 render가 발생하고 있는 것을 확인했다.

{% highlight javascript %}

    const Comonent = (props) => {

        const [a, setA] = useState('');
        const [b, setB] = useState('');
        
        useEffect(() => {
            const getData = async () => {
                const data = await callApi();
                setA(data.a);
                setB(data.b);
            }
            getData();
        })
    }

{% endhighlight %}


위와 같은 코드를 예로 들면, async 함수 내에서 데이터를 요청하고 state a, b를 setState 하고있다.
이 경우 각각의 setA, setB마다 render가 발생하게 된다.

리액트 내부의 라이프싸이클이나, 이벤트 핸들러 같은 경우 기본적으로 안에서 발생되는 setState는 모두 모여서 일괄 처리된다. <br>
하지만, 외부 모듈이나 비동기, 프로미스 내에서는 일괄 처리를 지원하지 않는다.

React 팀 에서는 향후에 어디서든 setState가 일괄 처리 될 수 있도록 업데이트할 예정이라 밝혔고 (아마도 17v), 임시로 사용할 API로 ReactDOM.unstable_batchedUpdates 제공한다.<br>
다만.. 함수명을 너무 무섭게 만들어놔서......ㅎㅎㅎㅎㅎㅎ

그 외에 해결 방법은 useReducer, class로 대체하거나 useState로 오브젝트를 관리 하는 방법도 있다.   ```ex) const [ab, setAB] = useState({a: '', b: ''});```


## Resource
[https://github.com/facebook/react/issues/14259](https://github.com/facebook/react/issues/14259)
[https://blog.logrocket.com/simplifying-state-management-in-react-apps-with-batched-updates/](https://blog.logrocket.com/simplifying-state-management-in-react-apps-with-batched-updates/)

