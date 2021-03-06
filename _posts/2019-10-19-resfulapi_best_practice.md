---
layout: post
title: "RESTful API Designing guidelines — The best practices"
tags: [guide]
---
이 글은 [RESTful API Designing guidelines — The best practices](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9)를 번역한 글 입니다.


Facebook, Google, Github, Netflix 등 거대 아이티 기업에서는 API를 통해 개발자들에게 데이터를 제공하고 하나의 플랫폼으로서 역할을 해왔다.
다른 개발자들을 위해 API를 제공하지 않더라도, API를 아름답게 잘 설계하는 건 자신의 어플리케이션을 위해서도 언제나 옳다!

긴 시간동안 API를 디자인하기 위한 가장 좋은 방법들이 논의 되어왔지만, 아직 공식적인 가이드라인은 없다.

API는 개발자들이 데이터와 상호작용하는 하나의 인터페이스다. 잘 디자인 된 API는 항상 사용하기 쉽고 개발자의 삶을 순탄하게 만들어준다. API는 개발자들에게 GUI이며, 만약 API가 명확하지 않거나 설명이 부족하다면, 개발자들은 사용을 멈추고 대체할 것들을 찾아 나설 것이다. 개발자 경험은 API의 품질을 측정하는 가장 중요한 지표다.

API가 무대위에서 공연하는 예술가라면 API유저들은 관객이라고 할 수 있다!

1) **용어**


다음은 REST API와 관련된 가장 중요한 용어들이다.
- **Resource**는 하나의 객체 혹은 데이터와 연관되어 표현된 무언가다. Resource는 수행 가능한 여러개의 메소드들의 집합을 가질 수 있다.<br>
E.g. Animals, schools and employees는 Resource이고, delete, add, update는 Resource에 적용될 operation이다.
- **Collections**은 Resource의 집합이다. <br> e.g) Companies는  Company resource의 집합
- **URL**(Uniform Resource Locator)은 Resource가 존재하는 위치와 어떤 action들이 수행될 수 있는지 보여주는 경로이다.

2) **API endpoint**

이해를 위해 Employees를 가지는 Companies API를 만들어보자.<br>
/getAllEmployees는 Employees list를 받기 위한 API다. <br><br>
Company API를 조금 더 살펴보면 아래와 같을 것이다.
- /addNewEmployee
- /updateEmployee
- /deleteEmployee
- /deleteAllEmployees
- /promoteEmployee
- /promoteAllEmployees

그리고 수 많은 API endpoint들이 위와 같은 형식으로 존재 할 거다. 위의 API들 형식을 보면 모두 불필요한 action을 포함하고 있다. 이런식이면, API수가 늘어날 때 유지보수가 굉장히 힘겨워진다.

### 뭐가 문제일까?
URL은 resources(명사)를 포함해야하며 action이나 동사를 포함해서는 안된다. /addNewEmployee를 예로 들면, resource Employee와 함께 addNew라는 action이 들어있다.

### 어떻게 해야 할까?
action을 포함하고있지 않는 /companies가 좋은 예이다.<br>
여기서 의문이 생길 수 있는데, 우리는 서버에 우리가 필요한 액션들(add, delete, update...)를 어떻게 알릴 수 있을까?<br>
**HTTP methods (GET, POST, DELETE, PUT)**가 그 역할을 해준다.

또한, Resource는 API endpoint에서 언제나 복수형 이어야한다. 만약, 단 하나의 인스턴스에 접근하고자 한다면, URL에 id를 실어 보낼 수 있다.
- method GET path /companies  -> 모든 company list를 받게 된다.
- method GET path /companies/34 -> company 34의 세부사항을 받게 된다.
- method DELETE path /companies/34 -> company 34를 지운다

또 다른 케이스들을 보면, 만약 우리가 한 resource에 종속된 resource를 가지고 있을 때 (예를 들어 특정 회사에 소속된 직원), 다음과 같은 API endpoints를 만들 수 있을 것이다.
- GET /companies/3/employees -> company 3의  employees list를 받는다.
- GET /companies/3/employees/45 -> company 3에 소속된 employee 45의 세부항을 받는다.
- DELETE /companies/3/employees/45 -> company 3에 소속된 employee 45 지운다.
- POST /companies -> 새로운 회사를 만들고, 만들어진 새 회사에 대한 세부사항을 리턴한다.

지금의 API가 훨씬 정확하고 일관성 있지 않은가? 😎

**결론**: 경로는 Resource의 복수형이어야 하고, HTTP method를 이용해 Resource에 수행할 action들을 정의해야 한다.

3) **HTTP methods (verbs)**

HTTP는 Resource에 수행할 몇 가지 ation 타입들을 정의한다.
>URL는 명사 - Resource와 동사 - HTTP method로 이루어진 문장이다.
주요한 HTTP method는 다음과 같다.

1. GET method는 Redource로 부터 데이터를 요청하고, 어떤  side effect도 발생해선 안된다.
E.g. /companies/3/employees company -> 3의 모든 employees를 리턴한다

2. POST method는 데이터베이스에 resource가 생성되도록 서버로 요청한다. 대부분, web form이 제출되는 경우이다.<br>
E.g. /companies/3/employees -> company 3의 Employee를 생성한다.<br>
POST는 멱등성을 지니고 있지 않아, 여러 번의 요청은 각각 다양한 결과를 가져 올수 있다.

3. PUT method는 Resource를 변경하거나, Resource가 존재 하지 않을 때 생성하도록 서버에 요청한다.<br>
E.g. /companies/3/employees/john -> compnay3 의 employees중 john Resrouce를 변경하거나, 존재 하지 않으면 생성하는 요청을 보낸다.<br>
PUT은 멱등성을 가지므로, 여러 번 요청을 해도 같은 결과를 가져온다.

4. DELETE method는 Resource나 인스턴스를 데이터베이스에서 지우도록 요청한다.<br>
E.g /companies/3/employees/john/ -> company 3에 소속된 john emplyees resource를 삭제하도록 서버에 요청한다.<br>

그 외에 몇 가지 method들은 다른 포스트에서 다루겠다!

4) **HTTP response status codes**


클라이언트가 API를 통해 서버에 요청할 때, 클라이언트는 그 요청이 실패했는지, 잘 통과 되었는지 또는 무언가 잘못 되었는지 피드백을 받을 수 있어야 한다. HTTP status code는 다양한 상황에 대한 설명으로서 표준화된 코드들이다. 서버는 항상 올바른 status code를 반환해야 한다.<br>
다음은 주요한 HTTP code들이다.

**2xx (Success category)**
200대 코드는 요청이 잘 전될 되었으며, 서버에서 올바르게 action 수행되었음을 나타낸다.

- **200 Ok**  : GET, PUT or POST 요청에 대한 성공을 나타낸다.
- **201 Created** : 새로운 인스턴스가 생성되었을 때 반환되는 status code다. E.g POST method를 통해 새로운 인스턴스를 생성했을 때 항상 201 status code가 반환되어야 한다.
- **204 No Content** : 요청이 성공적으로 수행되었으나 반환되는 content가 없을 때를 나타낸다. <br> DELETE 가 204의 좋은 예다. DELETE /companies/43/employees/2 : employee 2를 삭제하고 시스템에 명시적으로 삭제를 요청함으로서 API의 response body에 어떤 데이터도 넣어줄 필요가 없다. 만약, 어떤 에러라도 발생한다면, 예를들어 employee 2가 데이터베이스에 존재하지 않는다면, response code는 2xx가 아니라 4xx의 클라이언트 에러를 나타낼 것이다.

**3xx (Redirection Category)**
- **304 Not Modified** : 클라이언트가 이미 케쉬를 통해 응답 받았음을 나타낸다. 따라서, 데이터를 다시 전달해줄 필요가 없다.

**4xx (Client Error Category)**
다음의 status code들은 클라이언트 쪽에서 잘못된 요청을 보냈을 경우를 나타낸다.
- **400 Bad Request** : 서버측에서 클라이언트가 무얼 요청한 건지 알 수 없어, 요청이 진행되지 않았음을 나타낸다
- **401 Unauthorized** : 허가 되지 않은 클리아인트가 resource에 접근하였을 때, 인증과 함께 재요청을 해야함을 나타낸다.
- **403 Forbidden** : 요청도 유효하고 권한을 가진 유저지만, 어떤 이유에서 페이지나 resource에 접근하는 것이 불가할 때를 나타낸다. E.g 종종, 허가된 유저라도, 서버의 디렉토리에는 접근할 수 없다.
- **404 Not Found** : 유저가 요청한 resource가 현재 사용될 수 없음을 나타낸다.
- **410 Gone** : 요청되어진 resource가 의도적으로 옮겨져 사용될 수 없음을 나타낸다.

**5xx (Server Error Category)**
- **500 Internal Server Error** : 요청은 유효하지만, 서버측에서 예상치 못한 상황을 만나 해결하지 못했음을 나타낸다.
- **503 Service Unavailable** : 서버가 다운됐거나, 요청을 받거나 수행할 수 없을 때를 나타낸다. 대부분 서버가 점검중일 때가 많다.

5) **Field name casing convention**


어떤 네이밍 컨벤션을 사용해도 되지만, 앱 전체에 일관성 있게 적용되어야 한다. 만약 request body 또는 response type이 JSON일 경우에, camelCase를 일관성을 유지하기 위해 따라라.

6) **Searching, sorting, filtering and pagination**


이러한 모든 것들은, 하나의 데이터 집합의 쿼리를 간단하게 만들어 준다. 이런 액션들을 위한 새로운 API 집합은 필요 없다. GET method API에 쿼리 파라미터들만 추가하면 된다.
몇 가지 예로 어떻게 적용되는지 이해해보자.
- **Sorting** : 클라이언트가 정렬된 companies list를 얻고 싶은 경우, GET /companies endpoin는 여러가지 정렬 파라미터를 쿼리에 담아야한다.<br>E.g. GET /companies?sort=rank_asc -> companies list를 rank에 따라 오름차순으로 정렬할 것이다.
- **Filtering** : 데이터 집합을 필터링하는 경우, 쿼리 파라미털르 통해 여러가지 옵션을 전달할 수 있다.<br>E.g. GET /companies?category=banking&location=india -> india에 위치한 banking 카테고리의 companies list만 받게된다.
- **Searching**: companies list 에서 company 이름만 검색 할 때, API endpoint는 GET /companies?search=Digital Mckinsey 와 같은 형식으로 되어야 한다.
- **Pagination** : 데이터 집합이 너무 클 때, 우리는 response를 조금 더 쉽게 다루고, 퍼포먼스 향상을 위해 데이터를 작은 조각으로 나눈다.<br>E.g. GET /companies?page=23 -> 23 번째 페이지의 companies list를 받는다

만약 GET method에 쿼리 파라미터가 너무 많아 URI를 너무 길게 만드는 경우, 서버는 414 URI Too long HTTP status로 응답할수 있는데, 이 경우에는 POST method의 body를 이용해 파라미터들을 전달할 수 있다.

7) **Versioning**


만약 내 API가 여기저기서 쓰일 때, API에 변경사항과 함께 업그레이드를 한다는 것은, 내 API를 사용하는 프로덕트들과 서비스들에 문제를 일으킬 수 있다.
http://api.yourservice.com/v1/companies/34/employees 가 좋은 예가 될 수 있는데, 버전 넘버를 API 경로에 담은 것은 볼 수 있다.
만약 어떤 중대한 변화가 있을 경우에 API에 새로운 네이밍으로 v2나 v1.x.x와 같이 넣을 수 있다.






## Resource
[RESTful API Designing guidelines — The best practices](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9)

