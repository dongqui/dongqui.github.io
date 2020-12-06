---
layout: post
title: "Browser storage"
tags: [web]
---
한 번은 제대로 정리하고자 했던, 브라우저 저장소

{% include image.html path="postimages/browser_storage" path-detail="postimages/browser_storage" alt="" %}

## 용어 정리
- Origin
- Page Session

## Local Storage
- <strong>Document</Strong> Origin의 Storage 객체에 접근
- 브라우저 세션 간에 데이터가 공유되고, 페이지가 닫혀도 데이터가 만료되지 않음 (Secret 모드일 경우 마지막 탭이 닫힐 때 지워짐)

## Session Storage
- <strong>현재 Origin 세션</strong>의 Storage 객체에 접근
- 페이지 세션이 끝날 때 데이터가 지워짐
    - 페이지 세션은 브라우저가 열려있는 한 새로고침과 페이지 복구를 거쳐도 남아있음
    - 페이지를 새로운 탭이나 창에서 열면, 세션 쿠키의 동작과는 다르게 최상위 브라우징 맥락의 값을 가진 새로운 세션을 생성
    - 같은 URL을 다수의 탭/창에서 열면 각각의 탭/창에 대해 새로운 sessionStorage를 생성
    - 탭/창을 닫으면 세션이 끝나고 sessionStorage 안의 객체를 초기화
    - 저장한 자료는 페이지 프로토콜에 따라 구분 됨 (같은 도메인이어도 http와 https가 서로 다른 sessionStorage를 가짐)

## Resource
[https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage](https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage)<br>


Origin
웹 콘텐츠의 출처(origin)는 접근할 때 사용하는 URL의 스킴(프로토콜, 호스트(도메인), 포트로 정의됩니다. 두 객체의 스킴, 호스트, 포트가 모두 일치하는 경우 같은 출처를 가졌다고 말합니다.

일부 작업은 동일 출처 콘텐츠로 제한되나, CORS를 통해 제한을 해제할 수 있습니다.
