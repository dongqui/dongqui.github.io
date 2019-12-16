---
layout: post
title: "Http connection management"
tags: [네트워크]
---

<strong>대표적인 HTTP connection 모델들: <br>
short-lived connections, persistent connections, HTTP pipelining</strong>

### Short lived connection
초기 HTTP에서는 단기 커넥션 모델만을 제공했다. 요청이 보내질 때마다 커넥션들이 매번 새롭게 생성되었고 응답이 오면 연결을 닫는 단기 형태로 유지되었다.
TCP 연결은 핸드쉐이크 과정, DNS해석 등 많은 비용이 드는데다가 기본적으로 TCP는 느린 시작 지연을 가진다. 때문에, 이 초기 모델은 비효율을 낳기 상당히 쉽다.
>TCP 느린 시작 지연 - TCP 커넥션은 처음에 최대 속도를 제한하고 데이터가 성공적으로 전송됨에 따라 점점 제한을 줄여나가며 자체적으로 튜닝된다. 이는, 급작스러운 부하와 혼잡을 방지하기 위함이다.

예를 들어, 어느 웹 사이트의 메인 화면에 네비게이션, 사이드 바, 컨텐츠에 각각 이미지 파일을 받는다고 가정하자.
처음에 TCP 연결 후 HTML 파일을 받고 TCP 연결을 닫는다. 이 후에 다시 TCP 연결 후 네비게이션 이미지를 받고 TCP 연결을 닫는다. 모든 컨텐츠를 다 받을 때 까지 반복한다.
이렇게 순차적으로 연결과 요청이 발생하게 되면, 비용이 많이 드는 TCP 연결을 반복하게 되고 예열 후의 최적화 된 TCP 연결을 이용하기 힘들게 된다.

### Persistent connection
기존 모델의 단점을 해결 하기 위해 한 번 열린 커넥션을 재사용하는 영속적인 커넥션 모델이 나왔다. keep-alive 모델이라고도 불리며, TCP 연결 비용을 줄이고 느린 시작 지연 후 튜닝된 TCP를 사용 할 수 있게 된다.
단점으로는 유휴 상태일 때도 서버 리소스를 소비하게 된다. 가령, 접속 빈도가 높은 메인 페이지 같은 경우 서버의 가용성을 고려해 Persistent 모델이 꼭 필요한지 생각해 볼 필요가 있다.
HTTP1.1 부터는 기본 연결이 Persistent로 되어있어 따로 헤더 값이 필요 없다. Persistent를 사용하지 않을 경우에는 <string>Connection: close</string> 값을 헤더에 넣어주면 된다.


### Pipelining
Pirplelining 커넥션은 Persistent 연결을 이용해, 하나의 소켓에 여러 개의 요청을 보내게 된다. 
이 전에는 하나의 요청 후에 응답을 기다린 후 다시 요청을 보냈기 때문에 심각한 지연이 쉽게 생기는 반면에, 파이프라인을 이용하면 지연 시간을 훨씬 단축 할 수 있다.<br>
다만, 파이프라인을 지원하지 않는 브라우저의 존재와 복잡한 특성으로 정확하게 다루기 어렵고 예측할 수 없는 에러 사항이 많아 HTTP/2. 부터는 multiplexing 알고리즘으로 대체되었다.


{% include image.html path="postimages/http_connection.png" path-detail="postimages/http_connection.png" alt="" %}

### Resource

[https://developer.mozilla.org/en-US/docs/Web/HTTP/Connection_management_in_HTTP_1.x](https://developer.mozilla.org/en-US/docs/Web/HTTP/Connection_management_in_HTTP_1.x)
[https://americanopeople.tistory.com/115]()
