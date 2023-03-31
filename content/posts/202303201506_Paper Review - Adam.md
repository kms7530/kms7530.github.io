---
title: Paper Review - Adam
date: 2023-03-20T15:06:00Z
draft: false
katex: true
---

# Adam: A Method for Stochastic Optimization

## 앞서...

- Adam은 고차원 파라미터 공간에서의 최적화를 위해 나온 최적화 알고리즘이다.
- 이는 상대적으로 적은 메모리 공간을 요구한다.
- Adam의 이름은 Adoptive Moment Estimation의 약자다.
- 이는 RMSPop(Tieleman & Hilton, 2012)와 AdaGrad(Duchi et al., 2011)의 결합으로 만들어 졌다.
- 여담으로 저자의 순서는 기여도가 아닌 동전뒤집기(Google hangout을 통한)로 결정되었다.

## Adam's Update Rule

- Adam의 업데이트 방식에서 가장 중요한 점은 stepsize를 결정하는것에 있다.
- \delta_t=\alpha \dot \hatm_t / \sqrt{\hatv_t}
- 위의 식에서 다음과 같은 상황에 각각 다르게 계산된다.
    - (1-\beta_1) > \sqrt{1-\beta_2}인 상황에서는
    - |\delta_t| ≤ \alpha \dot (1 - \beta_1) / \sqrt{1-\beta_2}
    - 아닌 경우에는
    - |\delta| ≤ \alpha
- 위의 식을 각각 식 1, 식 2라고 칭하겠다.
- 식 1의 경우 현재 timestep 외의 모든 시간대에서 gradient가 0인 경우이다.
    - 다시 말하면 gradient의 크기가 늘어난다고 본다.
    - 이 경우는 매우 적게 일어난다.
- 식 2의 경우 gradient의 크기가 작아지는 경우이다.
- 수식 생략(나에게 너무나도 알기 힘든 내용이다...)
- 이러한 점으로 인해, 우리는 현재 파라미터 값에 대해, 충분한 정보가 있지 않는 경우에도, trust region을 만들수 있다.
    - 이 점은 \alpha 값을 상대적으로 쉽게 설정이 가능하게 해준다.
    - 값을 쉽게 설정이 가능하다는 점은, 대부분의 기계학습에서 높은 확률의 파라미터 공간을 설정하는것이 이점이 된다는 점에서 큰 장점으로 될수 있다.
- \alpha 즉 step의 크기가 정해지게 되면 \theta_0에서 어느정도의 iteration 후에 optima에 도달하게 될지 알수있다.
- SNR이 작은 값인 경우, stepsize \delta_t의 값이 0에 가까울 것이다.
    - 실제 gradient와 \hat{m_t} 간의 방향의 불확실성이 커질것(?)이다.

## 기존 연구의 특징과 차이점

- 기존의 유사 Newton 방식의 경우 mini-batch 방식을 사용 하지만, 이는 dataset의 minicatch 갯수와 linearly하게 연관이 있다.
- 이를 개선하기 위해 NGD(Natural Gradient Descent)(Amari, 1998)과 같이, 데이터의 기하학 관점을 선조건자로 사용하였다.
- 하지만 NGD와는 다르게, 피셔 정보 행렬의 역수에 대한 제곱근 값을 선조건자로 사용함으로써 비교적 보수적인 성격을 띈다.
