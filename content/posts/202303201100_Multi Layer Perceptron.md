---
title: Multi Layer Perceptron
date: 2023-03-20T11:00:00Z
draft: false
katex: true
---

# Linear Neural Networks

- 먼저, Neural network는 다은과 같은 특징을 가지고 있다.
    - 행렬 곱으로 구성된다.
    - 비선형의 데이터가 처리 가능하다.
    - 위 두개를 이용한 근사함수 형태이다. 식은 아래와 같다.
    
$$
\hat y=wx+b
$$
    
- MSE를 이용해 loss 값을 알아내는 경우, 이를 backpropagation 한 경우는 다음과 같다.
    - weight에 대해 계산
    
$$
\frac {\partial loss}{\partial w}=\frac \partial {\partial w} \frac 1 N \sum^N_{i=1}(y_i-\hat{y_i})^2
$$
    
$$
=\frac \partial {\partial w} \frac 1 N \sum^N_{i=1}(y_i-wx_i-b)^2
$$
    
$$
= \frac 1 N \sum^N_{i=1}-2(y_i-wx_i-b)x_i
$$
    
- bias에 대한 계산
    
$$
\frac {\partial loss}{\partial b}=\frac \partial {\partial b} \frac 1 N \sum^N_{i=1}(y_i-\hat{y_i})^2
$$
    

$$
=\frac \partial {\partial b} \frac 1 N \sum^N_{i=1}(y_i-wx_i-b)^2
$$

$$
= \frac 1 N \sum^N_{i=1}-2(y_i-wx_i-b)x_i
$$

- 위의 편미분 값을 이용하여 각각의 값을 update 할 수 있다.

$$
w\leftarrow w-\eta \frac{\partial loss}{\partial w}
$$

$$
b\leftarrow b-\eta \frac{\partial loss}{\partial b}
$$

- 이때 $\eta$ 는 step-size 다. 이를 적절하게 지정하는 것이 필요하다.
    - step-size를 고정하여 학습하는 방법도 있으나, 최근에는 이를 자동으로 변환하여 학습하는 방법도 생겼다.

## Linear Neural Networks의 행렬로 보는 관점

- Linear Neural Network에 대해 행렬로 나타내 어떻게 계산되는지 볼수있다.
- 먼저 기본적인 단층 perceptron을 행렬로 나타내면 다음과 같다.

$$
y=W^TX+b
$$

- 위의 수식에서 각각 $W$는 weight, $b$는 bias를 말한다.
- 위의 단층 perceptron을 이용하여 multi layer perception도 표현이 가능하다.
    
$$
y=W^T_2h=W^T_2W^T_1X
$$
    
- 하지만, 위의 식의 경우 활성화 함수가 없어 non-linear에 대응할수 없으로 중간에 활성화 함수를 넣어 다음과 같이 표현한다.

$$
y=W^T_2h=W^T_2\rho(W^T_1X)
$$

- 단, 활성화 함수의 경우 문제에 따라 적합한 함수가 다르다.

# Loss Functions

- Loss function은 목적에 맞게 다음의 종류 중에서 선택하여 사용하게 된다.

## 종류

- MSE
    
$$
MSE=\frac1N \sum^N_{i=1} \sum^D_{j=1}(y^d_i-\hat t^d_i)^2
$$
    
    - 기본적인 회귀 문제를 해결 할 때 사용하게 된다.
- CE
    
$$
CE=-\frac1N \Sigma^N_{i=1} \Sigma^D_{j=1}(y^d_i)\ log(\hat y^d_i)
$$
    
    - 분류 문제를 해결할 때 사용한다.
    - 이러한 식을 사용하게 되는 이유는 one-hot encoding으로 목적하는 하나의 label 만 부각되면 되기 때문에 이를 사용하게 된다.
- MLE
    
$$
MLE=\frac1N \sum^N_{i=1} \sum^D_{j=1}logN(y^d_i;\hat y_i^d, 1)
$$
    
    - 확률 문제를 해결할 때 사용한다.
    - 확률적인 불확실한 정보도 같이 필요하게 되는 경우 사용하게 된다.
