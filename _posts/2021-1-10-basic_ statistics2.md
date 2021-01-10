---
layout: post
title: "Basic Statistics(2)"
tags: [data]
---
큰 수의 법칙, 중심극한정리, 신뢰도

### 큰 수의 법칙 ( Law of large numbers )
{% include image.html path="postimages/law_large_numbers.png" path-detail="postimages/law_large_numbers.png" alt="" %}
sample 데이터의 수가 커질 수록, sample의 통계치는 점점 모집단의 모수와 같아진다.
<br>

### 중심극한정리 ( Central Limit Theorem, CLT )
{% include image.html path="postimages/clt.jpg" path-detail="postimages/clt.jpg" alt="" %}
Sample 데이터의 수가 많아질 수록, sample의 평균은 정규분포에 근사한 형태로 나타난다.
<br>

### 신뢰도 (Confidence interval)
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

### Resource
