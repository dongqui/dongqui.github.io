---
layout: post
title: "Basic Statistics(2)"
tags: [data]
---
큰 수의 법칙, 중심극한정리, 신뢰도, 베이지안 정리

<br>
<hr>
<br>

## 큰 수의 법칙 ( Law of large numbers )
{% include image.html path="postimages/law_large_numbers.png" path-detail="postimages/law_large_numbers.png" alt="" %}
sample 데이터의 수가 커질 수록, sample의 통계치는 점점 모집단의 모수와 같아진다.

<br>
<hr>
<br>

## 중심극한정리 ( Central Limit Theorem, CLT )
{% include image.html path="postimages/clt.jpg" path-detail="postimages/clt.jpg" alt="" %}
Sample 데이터의 수가 많아질 수록, sample의 평균은 정규분포에 근사한 형태로 나타난다.

<br>
<hr>
<br>

## 신뢰도 (Confidence interval)
{% include image.html path="postimages/confidence-interval.jpeg" path-detail="postimages/confidence-interval.jpeg" alt="" %}
신뢰도가 95% 라는 의미는 표본을 100번 뽑았을때 95번은 신뢰구간 내에 모집단의 평균이 포함된다.<br>

{% include image.html path="postimages/confi_math.svg" path-detail="postimages/confi_math.svg" alt="" %}
위의 공식에서 볼 수 있듯이, t-value, 분산이 커지면 허용 에러가 커지고 (신뢰구간이 넓어지고), 표본의 수가 커질 수록 허용 에러가 작아진다 (신뢰 구간이 좁아진다)
<br>
### 신뢰구간과 T-test의 관계
신뢰구간 = 시행한 가설검정이 통계적 의미를 갖는 범위<br>
모집단의 평균이 표본평균으로 부터 계산된 신뢰구간 안에 들어가는 경우, 귀무가설을 기각 X.<br>
반대로, 모집단의 평균이 표본평균으로 부터 계산된 신뢰구간 밖으로 나가는 경우 귀무가설을 기각.<br>
귀무가설 : 모집단의 평균값은 표본 평균값일 것이다. (2 tail t-test 기준)<br>

<br>
<hr>
<br>

## 베이지안 이론 (Bayesian Theroy)
흔히 알려진 통계는 빈도주의적 통계라고 불린다. 가령 주사위를 굴려 1이 나올 확률은 1/6이다! 라고 명확하게 정의하고 그 뒤에 계산을 통해 파생되는 결과물들을 검증하여 수용하는 과정을 거친다.

반면에 베이지안은 불확실성을 내포하는 경험적이이고 주관적인 사전확률을 기반으로하며, 추가되는 정보를 통해 계속해서 사전확률을 갱신해간다. 즉, 새로운 정보를 통해 어떤 사건이 발생했다는 주장의 신뢰도를 갱신해간다.

{% include image.html path="postimages/bayesian.png" path-detail="postimages/bayesian.png" alt="" %}

<br>
<hr>
<br>

## Resource
[https://ko.khanacademy.org/math/statistics-probability/random-variables-stats-library/expected-value-lib/v/law-of-large-numbers](https://ko.khanacademy.org/math/statistics-probability/random-variables-stats-library/expected-value-lib/v/law-of-large-numbers)
[https://www.youtube.com/watch?v=Y4ecU7NkiEI](https://www.youtube.com/watch?v=Y4ecU7NkiEI)