---
title: Modern Convolution Neural Network
date: 2023-03-21T10:24:00Z
draft: false
katex: true
---

# AlexNet

- 해당 모델은 ILSVRC에서 처음으로 딥러닝을 이용해 대회에서 우승을 했다는 점에 있어서 기념비적인 모델이다.
- 또한 ReLU, Data Argument 그리고 Dropout 등 현재는 당연하지만, 당시 이런 방식을 이용해 학습을 시키면 우수한 성능을 낸다고 보여주게 된 사례다.

# VGGNet

- $3 \times 3$ 필터를 이용해 깊이를 늘리고, $1\times 1$ Fully connected Layer를 이용했다.
    - 이때, $3\times 3$의 필터를 이용한 경우와 $5\times 5$의 필터를 이용하여 계산하는 경우 다음과 같이 파라미터 갯수가 차이난다.
        - 다음의 식은 128 크기의 레이어를 128로 변환하는 식이다.
        
$$
    3\times3\times128\times128+3\times3\times128\times128 = 294,912
$$
        
$$
    5\times5\times128\times128=409,600
$$
        

# GoogLeNet

- GoogLeNet 또한 ILSVRC에서 2014년도 우승한 모델이다.
- 이 모델의 특징으로 $1\times 1$ 필터를 이용하여 파라미터의 갯수를 줄이는 방법에 대해 처음 적용한 모델이라는 점이다.
    - 이를 사용했을 때 Channel의 차원을 줄일수 있다.
    - 다음의 예시는 위와 같이 128 크기의 레이어를 128로 변환하는 과정에서의 파라미터 갯수다.
        
$$
    3 \times3\times128\times128=147,456
$$
        

$$
(1\times1\times128\times32)+(3\times3\times32\times128)=40,960
$$

# ResNet

- 해당 model의 경우 skip connection을 이용하여 과적합을 해결한 모델이다.
    - Skip connection은 activation function에 넘기기 전에 입력값과 출력값을 더해 넘기게 된다.
- 또한, bottleneck을 이용하여 파라미터의 갯수를 줄였다.
