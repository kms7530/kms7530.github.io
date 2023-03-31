---
title: Batch와 Grdient Discent
date: 2023-03-20T13:57:00Z
draft: false
katex: true
---

## Gradient Descent Methods의 batch-size 구성 방식

- Stochastix Gradient Decent: Single Sample
- Mini-batch Gradient Descent: Subset of data
- Batch Gradient Descent: Whole data

## Batch-size

- 위의 방식에서 batch 크기를 정해주어야 하는데 이는 중요한 요소 중 하나다.
- Batch-size를 작게 할 수록 flat minimized 되고, 반대로 크게 할 수록 sharp minimized 된다고 한다.
    - 이 뜻은 generalization이 잘 된 즉, generalization gap 작게 된다는 뜻 이다.
    - 이를 아래의 그래프 참고해보자.
        
![스크린샷%202023-03-20%20오후%201.39.44.png](/Batch와%20Grdient%20Discent%205303bd0c23e847aeb0f89a9eb6fbf5d3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-03-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.39.44.png)
        
- 위의 그래프에서 점선은 Testing Function, 실선은 Training Function 즉 학습된 결과를 말한다.
- Flat과 Sharp 지점 각각을 보게 되면 Traing과 Test Function들 간의 차가 각각 작고 큰것을 볼수 있다.
- 차이가 크다는 점은 학습 결과와 테스트간의 괴리가 크다는 점을 의미한다.

## Gradient Descent Methods

![https://i.imgur.com/2dKCQHh.gif](https://i.imgur.com/2dKCQHh.gif)

### Gradient Descent

$$
W_{t+1}\leftarrow W_t-\eta g_t
$$

- 이를 사용하기 위해서는 Learning rate$(\eta)$가 필요한데, 이를 *“적절히”* 설정해야 한다.
- 하지만 적절히에 대해서 모를 뿐더러 시간도 오래 걸린다.’

### Momentum

$$
a_{t+1}=\leftarrow \beta a_t+g_t
$$

$$
W_{t+1}\leftarrow W_t-\eta a_{t+1}
$$

- Gradient Descent에서 오래 걸리는 문제점을 W의 변환되는 *“관성”* 이용하여 해결한 것 이다.

### Nesterov Accelerated Gradient

$$
a_{t+1}=\leftarrow \beta a_t+\nabla L(W_t-\eta \beta a_t)
$$

$$
W_{t+1}\leftarrow W_t-\eta a_{t+1}
$$

- Momentum의 경우 momentum 자체로 인해 spot을 지나치는 경우가 있는데, 이를 해결한 것이다.
- 이를 이용하여 더 빠른 계산이 가능하다.

## Adagrade

$$
W_{t+1}=W_t-\frac{\eta}{\sqrt{G_t+\epsilon}}g_t
$$

- 기존의 Gradient Decent에서 learning rate를 알아서 갱신하도록 만든 방식이다.
- $G_t$ 의 경우 grdient의 제곱들의 합인데, 이는 학습이 지속될수록 커지게 된다.
- 위의 사항으로 인해 학습이 지속되면 학습이 멈추게 된다.

## Adadelta

$$
G_t = \gamma G_{t-1}+(1-\gamma)g^2_t
$$

$$
W_{t+1}=W_t-\frac{\sqrt{H_{t-1}+\epsilon}}{\sqrt{G_t + \epsilon}}g_t
$$

$$
H_t=\gamma H_{t-1}+(1-\gamma)(\Delta W_t)^2
$$

- 기존의 Adagrade에서 $G_t$ 가 무한히 커지지 않도록 방지해놓은 방식이다.
- 하지만, learning rate가 없기 때문에 많이 사용되지 않는다.

## RMSProp

$$
G_t = \gamma G_{t-1}+(1-\gamma)g^2_t
$$

$$
W_{t+1}=W_t-\frac{\eta}{\sqrt{G_t + \epsilon}}g_t
$$

- 기존의 Adagrade에서 파생되어, Geoff Hinton이라는 교수가 자기 수업에서 경험적으로 해보니 좋아요 라고 한 방식이다.
- $\eta$ 는 stepsize를 말한다.

## Adam

$$
m_t=\beta_1m_{t=1}+(1-\beta_1)g_t
$$

$$
v_t=\beta_2v_{t-1}+(1-\beta_2)g^2_t
$$

$$
W_{t+1}=W_t-\frac \eta {\sqrt{v_t+ \epsilon}}\frac{\sqrt{1-\beta^t_2}}{1-\beta^t_1}m_t
$$

- $m_t$: momentum, $v_t$: gradient squres, $\eta$: stepsize
- 기존의 Momentum에서 파생되어, adaptive learning rate를 사용하고 있는 방식이다.
- 가장 많이 사용되고 있다.
