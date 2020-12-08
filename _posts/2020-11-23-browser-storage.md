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
    
## IndexedDB
- 영속적이고 대용량 데이터를 저장할 수 있다.
- Index를 사용하여 빠르게 데이터에 접근 가능하다.
- 모든 처리는 Transaction내에서 처리된다. 때문에 여러개의 탭을 열어 사용해도 데이터 무결성을 보장해준다.
- same-origin-policy를 따른다.
- key와 value로 구성되며 value는 복잡한 형태의 객체가 될 수 있다. 

## webSQL
- 대용량을 위한 SQLite 기반의 관계형 DB
- W3C에서 deprecated 시켰다. 이유는 WebSQL 자체가 SQLite와 굉장히 타이트하게 묵여있는 구조라, W3C입장에서 굉장히 중요하게 여기는 'Standard'를 세우기가 어려웠다고 한다.

## Cookies

## Resource
[https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage](https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage)<br>
[https://developer.mozilla.org/ko/docs/Web/API/Window/sessionStorage](https://developer.mozilla.org/ko/docs/Web/API/Window/sessionStorage)<br>
[https://developer.mozilla.org/ko/docs/Web/API/IndexedDB_API/Basic_Concepts_Behind_IndexedDB](https://developer.mozill.org/ko/docs/Web/API/IndexedDB_API/Basic_Concepts_Behind_IndexedDB))<br>
