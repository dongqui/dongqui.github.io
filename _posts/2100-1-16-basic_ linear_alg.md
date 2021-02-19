---
layout: post
title: "Linear algebra basic"
tags: [data]
---
기저 벡터, 선형 변환, 행렬식, 랭크

"행렬이 보이면 선형 변환이라고 생각해라"
"

## 선형 변환(LInear transformation)
선형 변환은 일종의 함수로, 벡터를 다른 공간으로 이동 시키는 것과 같다. 
- 조건
    - 모든 선들은 변한 뒤에도 평행이 유지되고 동일한 비율의 간격이어야 함 (변한 후에도 직선)
    - 원점은 그대로 있어야함
    
기저 벡터가 어디로 이동 했는지만 알면 나머지 벡터들의 위치도 알 수 있다.

행렬이 보이면

벡터에 행렬을 곱하는 것은 그 벡트를 선형 변환 하는 것과 같다.

벡터의 내적은 일반거으로 두 벡터의 곱으로 표현 되는 경우가 많다
두 벡터 중 하나는 transformation 으로 보고 1xN 행렬로 봐야 한다.
선형 변환이 발생하고 각각 스케일된 hat들의 수치를 더하면 최종 값이 나온다.


Correlation coefficient
분산에서 스케일을 조정하기 위해 표준편차를 사용했던 것처럼,

이번에도 공분산의 스케일을 조정할 수 있습니다.

공분산을 두 변수의 표준편차로 각각 나눠주면 스케일을 조정할 수 있으며 상관계수라고 부릅니다.

상관계수는 -1에서 1까지로 정해진 범위 안의 값만을 갖으며 선형연관성이 없는 경우 0에 근접하게 됩니다.

대부분의 경우, 상관계수는 공분산에 비해서 더 좋은 지표로써 사용되며 이유는 다음과 같습니다

공분산은 이론상 모든 값을 가질 수 있지만, 상관계수는 -1 ~ 1 사이로 정해져 비교하기가 쉽습니다.
공분산은 항상 스케일, 단위를 포함하고 있지만, 상관계수는 이에 영향을 받지 않습니다.
상관계수는 데이터의 평균 혹은 분산의 크기에 영향을 받지 않습니다.


 numeric이 아니라, categorical 이라면..?

이를 위해서 우리는 저번 주에 non-parametric에 대한 개념을 배웠습니다.

spearman correlation coefficient 는 값들에 대해서 순서 혹은 rank를 매기고, 그를 바탕으로 correlation을 측정하는 Non-parametric한 방식입니다.

🔥 python 에서 spearman correlation을 계산하는 법을 알아보세요.


{% include image.html path="postimages/law_large_numbers.png" path-detail="postimages/law_large_numbers.png"
<br>
<hr>
<br>

## Resource
