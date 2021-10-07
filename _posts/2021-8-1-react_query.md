---
layout: post
title: "React-query"
tags: [react]
---

사랑해요... React-query!

## 등장 배경
react-query는 서버 상태와 클라이언트의 상태가 분명하게 다르기 때문에, 관리하는 방법 또한 달라야 한다는 생각에서 시작한다.<br><br>
react-query가 말하는 서버 상태란,
- 서버 상태는 클라이언트에서 컨트롤 불가한 곳에 있다. (remote)
- fetch나 update를 위한 비동기 api가 필요하다. 
- 다른 사용자에 의해 데이터가 변경 될 수 있다.
- 어느 순간, 데이터가 최신화 되어있지 않은 상태가 될 수 있다.

위 와 같은 성질 때문에 생기는 아래의 문제들을 react-query가 처리해준다!

- 같은 데이터에 대한 중복 요청
- 백그라운드에서 데이터 최신화
- 서버에서 업데이트 된 데이터를 가능한 빨리 적용
- 페이지네이션, 레이지 로딩 최적화
- 서버 상태에 관한 메모리 관리
- 구조적으로 공유되는 쿼리는 메모라이즈!


## 예제 코드
{% highlight jsx %}
    import { QueryClient, QueryClientProvider, useQuery } from 'react-query'
     
     const queryClient = new QueryClient()
     
     export default function App() {
       return (
         <QueryClientProvider client={queryClient}>
           <Example />
         </QueryClientProvider>
       )
     }
     
     function Example() {
     // 인자로 전달되는 'repoData'는 유니크한 키로 사용 되어, 쿼리 인스턴스를 공유할 수 있게 된다.
     // 따라서, 같은 키를 사용하면 여러군데서 쿼리를 요청 하더라도 결과를 공유하고, 중복 요청을 막아준다.
       const { isLoading, error, data } = useQuery('repoData', () =>
         fetch('https://api.github.com/repos/tannerlinsley/react-query').then(res =>
           res.json()
         )
       )
     
       if (isLoading) return 'Loading...'
     
       if (error) return 'An error has occurred: ' + error.message
     
       return (
         <div>
           <h1>{data.name}</h1>
           <p>{data.description}</p>
           <strong>👀 {data.subscribers_count}</strong>{' '}
           <strong>✨ {data.stargazers_count}</strong>{' '}
           <strong>🍴 {data.forks_count}</strong>
         </div>
       )
     }

{% endhighlight %}



## Query Life cycle
1. ### fresh
- 새롭게 추가된 쿼리 인스턴스 → active 상태의 시작

2. ### fetching
- 요청을 수행하는 중인 쿼리

3. ### stale
- 패칭완료.
- 인스턴스를 데이터가 최신화 되지 않았다고 간주하며 refetching 대상.
- 특정 쿼리가 stale된 상태에서 같은 쿼리를 useQuery로 또 호출해 마운트를 시도한다면 캐싱된 데이터를 우선 반환.
- Stale 상태에서 Refetch되는 시점
        - 새 쿼리가 마운트 될 때 
        - 윈도우가 포커스 될 때 (다른 브라우저 탭 갔다가 오면 refetch 된다..!)
        - 네트워크가 다시 연결될 때
        - refetch interval을 직접 설정했을 때.

4. ### inactive
- 쿼리 인스턴스를 observe하는 컴포넌트가 없는 상태 (단, fetching 중일 경우 observe 되지 않아도 fetching상태를 유지한다).
- 5분 간격으로 아예 gc된다.

## 예시 시나리오 (cacheTime of 5 minutes, staleTime of 0)
1. useQuery('todos', fetchTodos)가 마운트 됨
2. 'todos' key를 가진 쿼리가 실행된적이 없기 때문에 loading state를 가지게 되고, 네트워크 요청을 만든다. (fetching 상태)
3. 마운트된 쿼리는 stale 상태로 들어감 (staleTime을 지정하지 않으면 useQuery의 기본 staleTime은 0이다. 즉 바로 stale 됨)
4. 두 번째 useQuery('todos', fetchTodos)가 어디선가 또 마운트 됨
5. useQuery('todos', fetchTodos)가 이미 stale 상태에 있기 때문에 일단 케쉬된걸 두 번째 애한테 넘겨줌
6. 백그라운드에서 refetch가 발생하고(리퀘스트는 한 번만) 양쪽의 인스턴의 데이터를 업데이트 해줌
7. 더이상 useQuery('todos', fetchTodos)을 사용하는 컴포넌트가 없어서 unmount 됨 (inactive)
8. 5분 동안 inactive면 가비지컬렉터가 집어감.


## 사용 후기
결론부터 말하면, 계속 쓰고싶다!<br>
가장 마음에 드는 점은,  API 요청/응답과 관련한 data, error, loading 상태를 직접 처리해야했던 모든 과정이 정말 간편하게 구현된다!<br>
또, 케쉬나 데이터 최신화를 백그라운드에서 알아서 처리해주기 때문에 기능면에서도 굉장히 좋다.<br>

다만, 강력한 기능을 가지고 있는만큼 조심히 사용해야 되겠다. 실제로, 로그아웃을 했음에도 데이터가 케쉬로 남아 사용자 정보를 노출 시킨적이 있다.....ㄷㄷ  <br>
커스텀 가능한 부분도 많고, optimistic-updates, pagination,  load-more-infinite-scroll 등 제공하는 기능도 굉장히 많아서 러닝 커브가 꽤 있어 보이지만, 학습할 가치가 충분히 있어보인다! 

## Resource
[https://react-query.tanstack.com/](https://react-query.tanstack.com/)
