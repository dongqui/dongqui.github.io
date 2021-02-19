---
layout: post
title: "Nuxt bundle size 줄이기"
tags: [web]
---

Analyzer를 돌려보니 5mb가 넘는 충격적인 vendor 사이즈.

{% include image.html path="postimages/big_bundle" path-detail="postimages/big_bundle" alt="" %}

## Moment
Moment 번들 이슈는 이미 많이 알려져 있었다. <br>
Moment안에 locale이 용량을 꽤 많이 차지하는데, 대부분 사용하지 않는 언어와 관련된 부분이라 없앨 필요가 있다. <br>
보통은 웹팩을 통해 번들에서 제외하는데, nuxt에서 자동으로 최적화해 제공해주는@nuxt/moment가 있어서 사용했다.

## Konva
Konva는 html canvas를 쉽게 사용할 수 있도록 도와주는 라이브러리인데,<br> 
plugin 폴더 내에서 import 돼서 처음 vue 인스턴스가 생성될 때 konva가 주입되고 있었다.<br>
사실상 처음부터 로드될 필요는 없는 모듈이라, plugin에서 제거하고 동적으로 불러오도록 변경했다.

## Firebase sdk
가장 큰 사이즈를 차지한 모듈이다. <br>
이번 프로젝트에서 사용하는 서비스는 Firestore, Fireauth, Firestorage인데 RealtimeDatabase, Messaging, Performance 등 온갖 서비스 모듈이 번들로 다 들어왔다.<br> 
import Firebase로 sdk를 통체로 불러오는 곳을 찾아내 제거해서 쓸모 없는 sdk가 들어오지 않도록 하고, 사용되는 sdk들도 분리해서 동적으로 불러오도록 했다.

## Vuetify
Material 디자인을 vue에서 쉽게 사용하게 해주는 UI 라이브러리다. <br>
nuxt에서 자체적으로 트리쉐이킹은 적용해주고 있어 크게 건드릴 건 없었고, css만 추출해서 따로 분리했다.<br>
그리고 번들과는 별개로, defaultAssets 설정이 없을 경우  쓸모 없는 폰트와 아이콘을 디폴트로 다 가져오는 이슈가 발견돼서 수정했다.

## Javascript-time-ago
 Moment.fromNow() 로 충분히 대체가능해서 삭제... 안일했다...!
 
## ETC
readable-stream, hash-base, elliptic ... 등 이름부터 괴랄한 모듈들이 어디서 들어온건지 webstorm의 힘으로 찾아보려고 했으나 실패.<br>
그래서 그냥 node_modules에서 지우고 다시 빌드해서 에러메시지로 추적했다;;; <br>
nuxt-vuex-localstorage 발견! 스토어랑 브리우저 로컬 스토리지랑 연결 시켜주는 모듈인데, 사용중인 범위가 너무 좁고 제한적이라서 삭제하고 직접 구현했다.<br>
audio-recorder-polyfill도 발견! 동적으로 임포트하도록 수정했다.

{% include image.html path="postimages/small_bundle" path-detail="postimages/small_bundle" alt="" %}
