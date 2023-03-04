---
title: Pytorch - 01
date: 2023-03-04T04:13:00+09:00
draft: false
katex: true
---

# Pytorch

- PT(Pytorch)와 TF(Tensorflow)간의 가장 큰 차이는 각각 Dynamic Computation Graphs와 Static Graphs를 사용한다는 점이다.
    - 이 그래프는 연산과정을 나타내는 그래프이다.
    - 각각의 차이는 다음과 같다.
        - Dynamic Computation Graphs: 실행을 하면서 그래프를 생성해간다.
        - Static Graphs: 실행 전 그래프를 모두 구성한 후 실행한다.
- 이 차이점으로 인하여 PT의 경우 학습 중간에 값을 확인하기 용이하다. 또한 이러한 이유로 인해 최근 사용량, 검색량이 늘어가는 추세다.
- PT = np + AutoGrad + Function
    - PT의 경우 Numpy 구조와 매우 흡사한 Tensor라는 객체로 array를 표현한다.
    - 자동미분을 지원하여 DL이 학습이 되도록 한다.
    - 또한 DL을 하기위한 다양한 fn들도 지원한다.