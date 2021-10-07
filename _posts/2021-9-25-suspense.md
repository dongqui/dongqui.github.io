---
layout: post
title: "React 비동기 처리"
tags: [react]
---

Suspense와 Error Boundaries 정리!

React-query, Suspense, React-error-boundary를 함께 사용 했을 때의 개발 경험이 너무 좋았다!<br>
react-query로 비동기 요청을 간결하게 만들고, 로딩과 에러 로직을 Suspense와 React-error-boundary를 통해 분리할 수 있었다.

## Suspense
Suspense는 컴포넌트가 읽어들이고 있는 데이터가 아직 준비되지 않았다고 React에 알려줄 수 있는 메커니즘이다.<br>
데이터를 불러오는 라이브러리가 아니고 단지, 데이터를 불러오는 라이브러리가 React와 결합할수 있도록 도와준다.<br>
Suspense는 어떻게 데이터를 불러오느냐에는 관심이 없고, 앱의 로딩단계를 시각적으로 통제하는 역할을 할뿐이다.
 
기존 방식에서는 useEffect나 컴포넌트 라이프 사이클 메서드 내에서 렌더링후 데이터를 불러오기 시작한다. <br>
Suspense는 불러오기 -> 렌더링 시작 -> 불러오기 완료 순으로 진행된다!

내가 가장 좋아하는 부분은, Loading UI를 선언적으로 구현할 수 있다는 점이다.

{% highlight jsx %}
    const promise = fetchProfileData();

     // ...ChildComponent
      if (user === null) {
        return <p>Loading profile...</p>;
      }
      return (
        <>
          <h1>{user.name}</h1>
          <ProfileTimeline posts={posts} />
        </>
      );
    
{% endhighlight %}

위와 같이 컴포넌트 내부에 로딩 로직이 들어있는 코드를 아래와 같이 Suspense로 컴포넌트를 감싸고, fallback을 이용해 Loading을 처리해주는 형식으로 바꿀수 있다.

{% highlight jsx %}

    function parentComponent() {
      return (
        <Suspense fallback={<h1>Loading profile...</h1>}>
          <ChildComponent />      
        </Suspense>
      );
    }
    
{% endhighlight %}


## Error Boundaries
에러 경계는 컴포넌트계의 catch {} 구문이라고 할 수 있겠다!
리엑트에서 제공해주는 getDerivedStateFromError, componentDidCatch를 통해 에러를 핸들링 할 수 있는 컴포넌트를 만들수 있다. 
이 컴포넌트는 하위 컴포넌트에서 발생하는 에러를 케치해 fallback UI를 렌더링하거나, 로그를 남길 수 있도록 해준다.
선언적으로 정의하여 깊숙한 곳에서 에러가 발생하더라도 무엇을 렌더링 할 지 구체화 할 수 있는 장점이 있다!
(여기저기 try catch로 명령을 남발하는 것을 상상해보자!)

또한 React의 변화도 주목할만하다.
React 16부터는 에러 경계에 포착되지 않는 에러가 발생하면, 전체 컴포넌트의 마운트가 해제 된다.
이것은 손상된 UI를 보여주느니, 아무것도 안 보여주겠다는 리엑트의 강한 의지!
미리미리 에러경계를 만들어주자!

그리고 에러 경계 컴포넌트를 구현해주는 라이브러리가 바로!
[react-error-boundary](https://www.npmjs.com/package/react-error-boundary) 👍


## Suspense + Error Boundaries
이 둘을 조합하면! 
각각의 컴포넌트에서는 필요한 상태만 존재하고, error와 loading 부분은 따로 분리가 가능해진다!
{% highlight jsx %}

    function ProfilePage() {
      return (
        <Suspense fallback={<h1>Loading profile...</h1>}>
          <ProfileDetails />
          <ErrorBoundary fallback={<h2>Could not fetch posts.</h2>}>
            <Suspense fallback={<h1>Loading posts...</h1>}>
              <ProfileTimeline />
            </Suspense>
          </ErrorBoundary>
        </Suspense>
      );
    }

{% endhighlight %}




## Resource
[https://ko.reactjs.org/docs/concurrent-mode-suspense.html](https://ko.reactjs.org/docs/concurrent-mode-suspense.html)
[https://ko.reactjs.org/docs/error-boundaries.html](https://ko.reactjs.org/docs/error-boundaries.html)<br>
[https://toss.im/slash-21/sessions/3-1](https://toss.im/slash-21/sessions/3-1)

