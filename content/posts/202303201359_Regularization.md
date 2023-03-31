---
title: Regularization
date: 2023-03-20T13:59:00Z
draft: false
katex: true
---

# Early Stop

- Validation 을 통해나온 generalization gap이 너무 커지기 전에 학습을 중단하는 것이다.

# Parameter Norm Penalty

- Paramater들이 너무 커지지 않도록 방지하는 방법이다.
- 수식은 다음과 같다.

$$
total\ cost=loss(D;W)+\frac\alpha2||W||^2_2
$$

# Data Augmentation

- 데이터가 적게 되면 전통적인 ML 방식이 문제 해결에 더 뛰어난데, 이를 해결하는 방법이다.
- 원본 데이터에 distortion(rotation, flip 등)을 주어 *“마치”* 새로운 데이터를 만들어 학습시키는 것 이다.
- 단, 좌우가 바뀌었을 때 데이터의 의미가 변경되는 경우 주의해야한다.

# Noise Robustness

- 위의 Data Arugmentation과는 달리, 데이터 뿐만 아니라 weight에도 노이즈를 주는 것 이다.
- “노이즈”를 주는 것이다.

# Label Smoothing

- 말 그대로 라벨을 모호하게 주는 것이다.
- 아래의 사진을 보자.

![스크린샷%202023-01-12%20오전%201.30.36.png](/Regularization%2096b16bd50c8f46d4a2f35241cce0ddd3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-12_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.30.36.png)

- Mixup: 여러 객체의 이미지를 블렌딩시켜 데이터를 만든다.
- Cutout: 이미지 일부를 누락시켜 데이터를 만든다.
- Cutmix: 이미지 일부를 다른 객체의 이미지로 채워 데이터를 만든다.

# Dropout

- 흔이 아는 그것이다. 뉴런들을 무작위로 0으로 할당하여 진행하는 것 이다.

# Batch Normalization

- 강사분도 말했다. 정확한 원리보단 레이어가 깊어질 때 쓰면 성능이 좋아진다는 것에 초점을 두라.
- 현재 아직 논란이 많은 방식으로 모두가 동의하고 있는 상태는 아니다.
- 파생품이 많은데 다음과 같다.
    - Layer Norm
    - Instance Norm
    - Group Norm: 얘는 위의 두 친구를 섞어 만든 것이다.
