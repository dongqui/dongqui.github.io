---
layout: post
title: "Basic Statistics(1)"
tags: [data]
---
T-test, ANOVA,  Chi square test


### P-value
- P 값은 특정 값이 정규 분포(확률 분포)의 어딘가에 해당하는지 나타내는 확률이다.
- 임계치(보통 0.05%)를 설정하고 확률적으로 우연인 것인지, 유의한 것인지 판단한다. 

### T-test
- 모집단의 표준편차가 알려지지 않았을 때, 정규분포의 모집단에서 모은 샘플(표본)의 평균값에 대한 가설검정 방법
- 평균값의 차이와 표준편차의 비율이 얼마나 큰지 혹은 작은지를 보고서 결정하는 통계적 과정
- 단순하게 이야기하면, 두 개의 집단이 같은지 다른지 비교하기 위해 사용한다고도 할 수 있다.

### T-test를 위한 통계적 질문
- 두 집단을 비교하기 위해 각각 표본의 평균 값을 구했을 때, 두 평균 값이 우연히 같을 확률은 얼마나 될까?
- 두 표본의 평균 값의 차이가 우연히 발생했을 확률은 얼마나 될까?<br>/Users/dongjinkim/github/dongqui.github.io/_posts/2021-1-05-basic_ statistics.md
***두 집단의 차이가 우연히 발생했다면 두 집단은 같게 볼 수 있다***

A 대학의 남학생 중 1000명의 평균 키가 180cm, B 대학의 남학생 중 1000명의 평균 키가 179cm라고 하자.<br> 
결정론적으로 말하면 A 대학의 남학생 키가 더 크다! 라고 이야기 할 수 있지만 통계학적으로는 1cm 차이가 우연히 발생한 것은 아닌지? 우연히 발생할 확률은 얼마인가에 대한 질문이 필요하다.
이에 1cm의 차이가 얼마나 큰지 혹은 작은지 결정할 나름의 비교 대상이 필요하다. 이때 표준 편차를 가져와 비교한다. 표준 편차는 데이터에 큰 문제가 없는 한은 ** 의미 없는, 우연히 퍼져있는 ** 정도를 수치로 나타내 값이다.<br>
그리고 두 표본 평균의 차이 1cm를 다시 생각해보면 1cm라는 값은 두 집단의 데이터들 사이의 평균적인 거리가 1cm라는 의미가 된다.<br>
이렇게 생각한다면, 두 집단의 평균 거리 1cm가 표준편차보다 현적히 작다면, 우리는 이 1cm의 차이에 큰 의미를 둘 수 없을 것이다. 반대로 1cm가 표준편차보다 현저히 크다면, 우리는 1cm의 차이에 큰 의미를 둘 수 있을 것이다.

### t-test의 종류
- One-sample t-test
    - 표본이 하나
-Paired t-test
    - 표본이 하나, 시간의 흐름에 따라 before/after 비교
- Two-sample t-test
 - 표본이 두 개
 
### t-test를 위한 t-값 & t-분포
{% include image.html path="postimages/t_value.png" path-detail="postimages/t_value.png" alt="" %}

- 두 집단의 평균값의 (편)차가 의미 없는 편차인 표준편차만도 못하다면 당연히 이 차이는 우연히 발생했다고 보아야한다.
- 자유도(df)는 n-1로 계산되고, 여기서 자유도가 커질수록 t분포가 점점 표준정규분포를 닮아간다.
    (사전 정의: 주어진 조건하에서 통계적 제한을 받지 않고 자유롭게 변화될 수 있는 요소의 수)
- t-value가 나왔다면, t-table(cf: 표준정규분포표)을 보고 P-value를 구하면 된다.

### Chi square test
{% include image.html path="postimages/chi_1.png" path-detail="postimages/chi_1.png" alt="" %}
- 종속변수, 독립 변수 모두 명목 척도일때 (categorical variable)
- 데이터의 값은 개수여야함
- 변수가 한 개인 경우: 변수가 특정 분포를 따르는지
- 변수가 두 개인 경우: 변수 사이의 연관성이 있는지 없는지
- 역시나 카이제곱 테이블, 분포가 존재함

### ANOVA (ANalysis Of VAriance)
- 세 개 이상 다수의 집단을 비교하고자 할 때 사용하는 검정

### One-way ANOVA에 사용되는 변수
- 종속변수: 연속형(Continuous) 변수만 가능
- 독릭변수: 이산형, 변주형(Discrete, Categorical) 변수만 가능, 독립변수는 한 개이고 그 안에 여러개의 레벨이 있는 형태
    - ex) 신약 개발: 새로 개발된 약, 기존 약, 플라시보 약 

### F-value를 통해 F-분포 확인
- F-value는 두개의 분산의 비율 -> 두 개의 평균이 필요하다.
- 첫번째 분산은 전체 평균으로부터 각 그룹의 평균 분산: Between Variance
    - 만약에 Between variance가 충분히 크다면, 적어도 어떤 그룹 한 개는 다른 그룹과 떨어져 있을 수 있다.
    - 여기서 Between variance가 얼마나 커야 유의한 것일까 -> 비교할만한 다른 분산이 필요!
- 그래서 필요한 두번째 분산: 그룹내의 분산: Within Variance
    - t-test의 t-value 계산시의 분모의 표준편차와 같은 의미 -> <br>
    random한 (즉, 무의미한) 변화의 정도 -> <br>
    Between variance가 Within Variance 보다 충분히 커야, 적어도 한 그룹이 전체 평균과는 다르다고 할 수 있다.
{% include image.html path="postimages/f-value.jpeg" path-detail="postimages/f-value.jpeg" alt="" %}
        
### t-test, ANOVA 조건
- 독립성: 두 그룹이 연결되어있는 (paired) 쌍인지
- 등분산성 : 두 그룹이 어느정도 유사한 수준의 분산 값을 가지는지
- 정규성: 데이터가 정규성을 나타는지

### Resource
[https://www.youtube.com/channel/UCnN2E8RCEuKi-WLBrd0Nu1A](https://www.youtube.com/channel/UCnN2E8RCEuKi-WLBrd0Nu1A)
[https://doooob.tistory.com/101](https://doooob.tistory.com/101)<br>
[https://www.youtube.com/watch?v=HKDqlYSLt68](https://www.youtube.com/watch?v=HKDqlYSLt68)
[https://bioinformaticsandme.tistory.com/198](https://bioinformaticsandme.tistory.com/198)<br>
[https://www.youtube.com/watch?v=MnZ3f0YjC3M](https://www.youtube.com/watch?v=MnZ3f0YjC3M)
