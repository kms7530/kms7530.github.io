---
title: Paper Review - Dropout
date: 2023-03-21T15:30:00Z
draft: false
katex: true
---

# Dropout의 생성 배경

- 논문이 나올 당시에는 적은 데이터에 대해 지속적으로 학습을 시키게 될 때 과적합이 일어났었다.
- 또한 모델이 커지게 되면서 각각의 모든 최적의 hyperparameter를 실험하기에는 너무나도 많은 시간이 걸린다.
- 그래서 이를 해결하기 위해 생긴것이 Dropout 이다.

# Dropout이 하는 것

- 먼저, 이름 그대로 유닛 자체를 out 시키는 것이다.
- Out 시키기에 앞서 먼저 각 유닛에 p(=0.5)를 할당해 준다.
    - 단, input의 경우 1에 가까운 확률을 할당해 준다.
- 단, dropout은 test시 사용하지 않는다.

# 어떻게 생각해냈나?

- 저자는 인간의 자손 번식(Livnat et al., 2010)에서 영감을 얻었다고 한다.
- 인간의 자손 번식은 각 부모의 유전자 절반씩을 base로 약간의 mutation을 통해 생산되게 된다.
    - 이때 두 부모의 유전자 절반에서 오랜 시간동안 남아 강화된 부분의 경우 자손에 전달이 되게 된다.
- 이러한 자연의 섭리를 이용하여, 강화된 부분을 다음 레이어에 전달해주는 것 처럼 의미가 적은 유닛은 도퇴되도록 한것이다.

# 관련 연구

- Dropout은 hidden unit에 노이즈를 줌으로써 regularizing 하는 방법이다.
- 이러한 점은 Denoising Autoencoders(Vincent et al., 2008, 2010)에서 input unit에 노이즈를 추가하는 방법과 유사하다.
- 여기서 나아가 dropout은 hidden layer에 노이즈를 추가하였다.
    - 이로 인해 비지도 학습 뿐만 아니라 지도학습에서 좋은 성능을 내었다.
