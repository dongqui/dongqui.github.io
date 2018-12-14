---
layout: post
title: Scaling server with Socket.Io (2 / 3)
tags: [Node, web, project]

---

하나의 프로세스만 있을 때는 socket.io 프로세스내에 메모리가 존재해서 socket, room, channel과 같은 정보를 저장 할 수 있다.
그런데 클러스터를 만들면 각각의 프로세스가 생성되게 되고 프로세스가 저마다의 저장소를 가지게 되면서, 서로 공유하는 메모리가 사라진다.

{% include image.html path="postimages/structure.003.jpeg" path-detail="postimages/structure.003.jpeg" alt="" %}

즉, 1번 워커에 접속한 유저와 2번 워커에 접속한 유저가 대화를 할 수 없는 일이 발생한다.

socket.io 개발 팀은 이를 해결하기 위해 [Redis](https://redis.io/)를 사용했다.<br>
Redis는 간단히 말해, in-memory DB다.<br>
Redis는 Pub/Sub message pattern을 이용하고 있는데, 간단히 살펴보면, 퍼블리셔는 특정 리시버에게 메세지를 전달 하는 것이 아니라, 누가 받게 되는지 모른체 특정 채널에 메세지를 전달하게 된다. 리시버는 자신이 원하는 채널을 구독할 수 있고, 누가 보내는지 모르는체 메세지를 받게 된다.<br>
결과적으로, 내부에 각각 지니고 있던 메모리 저장소 외에, 레디스를 이용해서 프로세스간에 저장소를 공유하도록 할 수 있다.
{% include image.html path="postimages/structure.004.jpeg" path-detail="postimages/structure.004.jpeg" alt="" %}

여기서 레디스를 쓰면서 내가 만났던 문제는, 비동기 처리가 늘어났다는 것이다. 이 앱에서는 시간 제한, 인원수 같은 room에 들어가는 제한 조건이 있기 때문에 조건을 검사하기 위해 비동기 처리를 loop로 돌아야 했다.<br>
그런데 우아한 형제들 기술 블로그나, 여타 다른 곳에서 이벤트 루프에서 블락을 일으키는 다 수의 작업이 싱글쓰레드 기반의 Node js에는 치명적으로 작용한다는 것을 이미 보았기 때문에 단순히 비동기 처리를 동기적으로 loop를 돌리는 건 피하고 싶었다.

내가 선택한 방법은, 하나의 데이터 구조를 더 만들고 유저가 접속하고 나갈 때 마다 조건을 확인해서 조건에 부합하지 않으면 아얘 지워버리거나 다시 생성하는 방법으로 구현했다.  즉, 조금은 로직이 번잡해지더라도 방 정보가 존재한다는 것 자체가 조건에 부합 한다는 것을 의미하고, 순차적으로 loop를 돌며 조건을 확인할 필요를 없앴다.

### Resource
[https://redis.io/](https://redis.io/)<br>
[http://woowabros.github.io/woowabros/2017/09/12/realtime-service.html](http://woowabros.github.io/woowabros/2017/09/12/realtime-service.html)<br>
[http://www.nextree.co.kr/p7292/](http://www.nextree.co.kr/p7292/)




