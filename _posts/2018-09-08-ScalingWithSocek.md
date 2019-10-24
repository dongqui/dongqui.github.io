---
layout: post
title: Scaling server with Socket.Io (1 / 3)
tags: [Node, web, project]

---
이번에 위치 기반 채팅 어플리케이션 프로젝트를 진행하면서, socket과 서버 확장을 다룰 기회가 있었다.

{% include image.html path="postimages/structure.001.jpeg" path-detail="postimages/structure.001.jpeg" alt="" %}

위에 발로 만든 그림이 보여주는 구조가 완성되기 까지 여러가지 이슈와 고민거리들이 있었다. 로드밸런싱 방식, 레디스를 사용하면서 생긴 비동기 처리들, nginx 옵션 등 또 까먹기 전에 정리해둬야지.

## Socket.io와 Clustering
socket io는 쉽게 말해 웹소켓을 쉽게 사용할 수 있도록 해주는 라이브러리다. 연결이 끊겼을 시에 재연결 시도, 브라우저 호환, 권한 문제 등 여러가지 처리들을 쉽게 구현해놨다. 그 중 가장 큰 특징 중 하나는, 모든 브라우저가 사용 가능하다는 점이다.<br>
즉, 웹소켓을 지원하지 않는 브라우저도 호환이 가능한데, 그렇게 할 수 있는 이유는, Socket.io는 처음 부터 웹소켓으로 업그레이드를 시도하지 않고 먼저 long-polling 통신을 성사 시킨 후, 웹소켓으로 업그레이드가 가능하다고 판단 할 때 업그레이드를 시도한다. 만약, 웹소켓을 지원하지 않는 브라우저라면 연결을 끊는게 아니라 long-polling 방식으로 통신을 이어간다.<br>
이러한 특징 때문에 Socket.io를 사용하면서 서버를 확장 할 때 주의해야 할 점이 있다. socket.io가 long-polling 통신을 성립 시키기 위해 핸드쉐이크 과정을 거치기 때문에 한 번 핸드쉐이크가 성립 된 워커 클러스터로 다시 요청을 보낼 수 있도록 보장 해주어야 한다. 다시 발로 그린 그림으로 살펴보면 아래와 같다.

{% include image.html path="postimages/structure.002.jpeg" path-detail="postimages/structure.002.jpeg" alt="" %}

그 방법으로, sticky-session과 같은 라이브러리를 쓸 수 도 있고, nginx 같은 경우 로드밸런싱 알고리즘을 ip_hash로 설정 할 수 있다. 대략적인 레퍼런스는 socket io 문서에도 나와있다.

그런데 이렇게 클라이언트와 클러스터를 묶으면, 로드밸런싱이 불균등하게 이뤄질 수 있다. 게다가 이번에 진행하는 프로젝트는 모바일 앱이었기 때문에 굳이 long-polling 통신이 필요 하지 않다고 판단해서 long-polling을 스킵하는 옵션을 넣고, 라운드로빈 방식의 로드밸런싱을 선택했다.

{% highlight javascript %}
const connection = io.connect(URL, {transports:['websocket']}
{% endhighlight %}


### Resource
[https://redis.io/](https://redis.io/)<br>
[https://d2.naver.com/helloworld/1336](https://d2.naver.com/helloworld/1336)<br>
[https://socket.io/](https://socket.io/)



