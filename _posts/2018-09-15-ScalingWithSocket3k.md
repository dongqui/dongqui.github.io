---
layout: post
title: Scaling server with Socket.Io (3 / 3)
tags: [Node, web]

---

처음에는 150명 가량 동시접속이 가능하고 마지막에는 8000명 까지 동시접속이 가능하다는 걸 수치로 확인할 수 있었는데,
### Test tool은 [artillery](https://artillery.io/)를 사용했다. socket.io 테스트를 지원하고, 기본 베이스가 자바스크립트라 사용하기 편했다!

{% include image.html path="postimages/yml.png" path-detail="postimages/yml.png" alt="" %}

위와 같이 시나리오를 작성하면 여러가지 결과 값을 아래와 같이 시각화 할 수 있다

{% include image.html path="postimages/ten2.png" path-detail="postimages/ten2.png" alt="" %}
<br>
<hr>
<br>
<br>

그리고 퍼포먼스를 극대화 하기 위해 OS, nginx 쪽에서 신경써야할 옵션들이 있었는데, 여기는 조금 더 공부가 필요... !
### 먼저, os는 특정 프로세스가 지닐 수 있는 file descriptor의 한계치를 정해놓는데, 이를 풀어줘야 했음.<br>
- [참고](https://medium.com/mindful-technology/too-many-open-files-limit-ulimit-on-mac-os-x-add0f1bfddde)

> In Unix and related computer operating systems, a file descriptor (FD, less frequently fildes) is an abstract indicator (handle) used to access a file or other input/output resource, such as a pipe or network socket. File descriptors form part of the POSIX application programming interface.


### nginx에서도 퍼포먼스와 관련된 여러가지 옵션 값들이 있다.<br>
- [참고](https://gist.github.com/denji/8359866)




