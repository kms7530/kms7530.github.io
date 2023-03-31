---
title: Convolution Neural Network
date: 2023-03-20T15:15:00Z
draft: false
katex: true
---

# Convolution Neural Network의 구성

- CNN은 3가지의 구성으로 볼 수 있다.
    - Convolution, Polling Layers: 이곳은 Feature를 뽑는 구간이다.
    - Fully Connected Layer: 이곳은 위 작업의 결과를 받아 실제 결정(분류, 회귀 등)을 하는 구간이다.
        - 이는 레이어는 많아질수록 학습이 어려워지면서, generalization이 어려워지기 때문에 이를 줄이려한다.

# Stride and Padding

![https://upload.wikimedia.org/wikipedia/commons/0/04/Convolution_arithmetic_-_Padding_strides.gif](https://upload.wikimedia.org/wikipedia/commons/0/04/Convolution_arithmetic_-_Padding_strides.gif)

- Stride
    - 커널을 계산 할 때 input을 얼마나 건너 뛰고 계산하게 될 것 인지 결정하는 것 이다.
    - 예: stride=1 인 경우는 하나씩 픽셀을 넘어가게 되지만, stride=2 인 경우 픽셀을 2번 건너뛰어 kernel을 적용한다.
- Padding
    - 끝 부분에 말 그대로 padding을 넣어 계산하는 것이다. 값으로는 0을 넣게 된다.
- 위 이미지는 Padding은 1, Stride는 2인 예시다.

# Parameter 갯수

![스크린샷%202023-01-11%20오후%2011.29.42.png](/Convolution%20Neural%20Network%208e83dc8faaca419d9e4ce7b875927393/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.29.42.png)

$$
W(3)\times H(3)\times I_C(128)\times O_C(64)=73,728
$$

# $1\times 1$ Convolution

## 이런짓은 왜 할까?

- 정답인 이 계산을 통하게 되면 채널의 갯수 즉 차원을 줄일 수 있다.
- 줄여진 차원을 통해 깊이를 늘려 계산을 향상시킨다.
