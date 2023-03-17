---
title: Hyperparameter Tuning
date: 2023-03-16T13:09:00Z
draft: false
katex: true
---

# Hyperparameter Tuning

- 모델 스스로 학습하면서 결정되는 요소 외의 값에 대해 변화를 주며 성능 향상을 시키는 방법이다.
- 주로 Learning rate, model size, optimizer 등을 변경해본다.
- 과거의 경우 hyperparameter에 의해 성능이 크게 좌우되는 경우가 있었는데, 이는 현재에 와서 다량의 데이터로 인해 편차가 작아졌다.
- 가장 기본적인 방법은 grid search, random search가 있으며 최근에는 베이지안 기반 방법이 주도하고 있다.

# Ray

- Spark를 만든 곳에서 나온 Hyperparameter Tuning API이다.
- Multi-node Multi, processing을 지원하며, 병렬처리가 가능하게 해준다.
