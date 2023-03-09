---
title: 통계학의 개념 그리고 딥러닝에서의 사용
date: 0001-01-01T12:00:00+09:00
draft: false
katex: true
---

# 모수

- 먼저 통계적 모델링의 경우 적절한 가정을 통해 확률분포를 추정하는 것에 목표를 둔다.
    - 하지만, 유한한 데이터를 통해 정확히 알아내는것은 불가능하기 때문에, 근사적인 확률분포를 추정한다.
- 모수를 어떠한 방식으로 추정하냐에 따라 방법론의 이름이 다르다.
    - 모수적 방법론: 데이터가 특정 확률분포를 따른다고 먼저 가정하고 그 분포를 결정하는 모수를 추정하는 방법이다.
    - 비모수 방법론: 특정 확률분포를 따른다 가정하지 않은체 데이터에 따라 모델 구조와 모수 개수를 유연히 바꾸는 방법이다.

# 확률분포 Types

- 무조건적으로 해당 내용을 따라가는것이 아닌, 데이터 생성 원리를 참고하여 선택하는것이 좋다.
    - 데이터가 바이너리 값을 가지는 경우 → 베르누이분포
    - n개의 이산적인 값을 가지는 경우 → 카테고리분포
    - [0, 1] 사이의 값을 가지는 경우 → 베타분포
    - 0 이상의 값을 가지는 경우 → 감마분포, 로그정규분포 등
    - R 전체에서 값을 가지는 경우 → 정규분포, 라플라스분포 등

# 데이터를 통해 모수를 추정해보기

- 먼저 정규분포의 모수는 평균 $\mu$ 와 분산 $\sigma^2$ 으로 다음의 식과 같다.

$$
\bar X=\frac1N\sum ^N_{i=1}X_i \ \ \ 
S^2=\frac1{N-1}\sum 1{i=1}(X_i-\bar X)^2 \newline
E[\bar X]=\mu \ \ \ \ \ E[S^2]=\sigma^2
$$

- 좌우 각각 표본평균과 표본분산이다.
- 여기서 N이 아닌 N - 1을 사용하는 이유는 unbased 추정량을 구하기 위해서다.
- 이러한 통계량의 확률분포를 Sampling Distribution(표집분포)라 부르며, N이 커질수록 표집 분포는 정규분포를 따르게 된다.
- 아래는 표본의 숫자가 많아질수록 정규분포에 수렴하는 모습이다.
    

# Maximum Likeihood Estimation - MLE

- 위의 표본평균 혹은 표본분산도 중요한 통계량이나, 확률분포마다 모수가 다르기때문에 universal하게 사용하기 힘들다.
- 그리하여 나온것이 MLE다.
    - 이론적으로 가장 가능성이 높은 모수를 추정하는 방법이다.
    - 여기서 가능도는 데이터 X에 대해 모수 $\theta$에서 관측될 가능성(상대적인 값)이지 확률이 아니다.
- MLE의 기본적인 식은 다음과 같다.

$$
\hat \theta_{MLE} = \argmax_\theta L(\theta;x)=\argmax_\theta P(x|\theta)
$$

## 하지만 그냥 MLE를 쓰지 않는다.

- 대부분 Log-MLE를 사용한다.
    - 이유는 데이터 숫자가 크게 높아지는 경우 컴퓨터의 정확도로 가능도를 계산하는것이 불가능해진다.
    - 또한 연산량을 $O(n^2)$ 에서 $O(n)$ 으로 줄어든다.
    - 따라서 계산의 용이성으로 인해 로그 연산을 하여 계산한다.
    - 손실함수에서 경사하강법을 사용하는경우 음의 로그가능도를 이용해 최적화 한다.
- Log-MLE는 다음과 같다.

$$
L(\theta;x)=\Pi ^n_{i=1}P(x_i|\theta) \rightarrow logL(\theta;x)=\sum^n_{i=1}logP(x_i|\theta)
$$

# MLE 예제 - 정규분포

- 정규분포를 따르는 확률변수 $X$ 로부터 독립적인 표본 $\{x_1, ... , x_n\}$ 을 얻었을때 MLE를 이용하여 모수를 추정해보자.
    
$$
\hat \theta_{MLE} = \argmax_\theta L(\theta;x)=\argmax_\theta P(x|\theta)
$$
    
- 먼저 $\theta=(\mu, \sigma)$으로 변형하여 최적화가 가능하다. 이를 바탕으로 식을 변형하면, 다음과 같다.
    
$$
\hat \theta_{MLE} = \argmax_\theta L(\theta;x)=\argmax_\theta P(x|\mu, \sigma^2)
$$
    
- 위의 식에 log를 취해주면 다음과 같이 변형된다.

$$
\sum^n_{i=1}log\frac1{\sqrt{2\pi\sigma^2}}e^{-\frac{|x_i-\mu|^2}{2\sigma^2}} \newline
=-\frac n2log2\pi\sigma^2-\sum^n_{i=1}\frac{|x_i-\mu|^2}{2\sigma^2}
$$

- 우리는 가능도를 최대화 하는것이 목표다. 따라서 $\mu, \sigma$ 각각의 기울기가 0인 지점을 찾으면 된다.
    - 0인 지점이 바로 정규분포의 극대점이기 때문이다.
- 식은 아래와 같다.

$$
0=\frac{\delta logL}{\delta \mu}=-\sum^n_{i=1}\frac{x_i-\mu}{\sigma^2}\newline
0=\frac{\delta log L}{\delta \theta}=-\frac n\sigma + \frac 1{\sigma^3}\sum^n_{i=1}|x_i-\mu|^2
$$

- 단, 표본평균과 표본분산과 다르게 **불편추정량(unbiased)**을 보장하지 않는다.

# MLE 예제 - 카테고리 분포

- 카테고리 분포 Multinoulli$(x;p_1, ..., p_d)$를 따르는 확률변수 X로 부터 독립적인 표본 $\{x_1, ..., x_n\}$을 얻었을 때 MLE를 이용하여 모수를 추정해보자.
    - 이때 카테고리 분포의 모수는 모두 합했을 때 1이 되어야 한다.
- 먼저, 식은 다음과 같다.

$$
\hat \theta_{MLE}=\argmax_{p_1,...,p_d}logP(x_i|\theta) = \argmax_{p_1,...,p_d}\ log(\Pi^n_{i=1}\Pi^n_{k=1}p_k^{x_ik})
$$

$$
log(\Pi^n_{i=1}\Pi^n_{k=1}p_k^{x_ik})=\sum^d_{k=1}(\sum^n_{i=1}x_{i, k})log\ p_k
$$

- 위의 식에 대해 아래의 식을 만족하는 최대 값을 찾는것이 카테고리 분포에서 MLE를 계산하는 방법이다.
    
$$
\sum^d_{k=1}p_k=1
$$
    
- 위의 식은 분포의 합이 1이라는 설명에 대한 수식이다.
- 위의 수식을 라그랑주 승수법을 통해 변형이 가능하다.
    - 수식
    
$$
\mathcal{L}(p_1,...,p_k, \lambda)=\sum^d_{k=1}n_klog\ p_k+\lambda(1-\sum_kp_k)\newline
0 = \frac{\delta\mathcal{L}}{\delta p_k}=\frac{n_k}{p_k}=-\lambda \newline
0=\frac{n_k}{p_\lambda}=1-\sum^d_{k=1}p_k
$$
    

# 딥러닝에서의 최대가능도 추정법

- 결론적으로 one-hot vector에 대해 괒찰데이터를 통해 확률분포인 소프트맥스 벡터의 로그가능도를 최적화 할수있다.
- 수식은 아래와 같다.

$$
\hat\theta_{MLE}=\argmax_\theta\frac1n\sum^n_{i=1}\sum^K_{k=1}y_{i, k}log(MLP_\theta(x_i)_k)
$$

# KL Divergence

- 기계학습에 사용되는 손실함수들은 학습하는 확률분포와 데이터에서 관찰되는 확률분포의 거리를 통해 유도한다.
- 그 중 쿨백-라이블러 발산을 알아보자.
- 수식은 아래와 같이 정의한다. 각각 이산확률변수, 연속확률변수에 대한 수식이다.

$$
KL(P||Q)=\sum_{x\in X}P(x)log (\frac{P(x)}{Q(x)})\newline
KL(P||Q)=\int_XP(x)log (\frac{P(x)}{Q(x)})dx
$$

- 이 식은 정답레이블을 P. 모델 예측을 Q라 할 때 해당 식의 발산을 최소화 하는 뱡향으로 사용한다.

$$
KL(P||Q)=-E_{X\sim P(X)}[log\ Q(x)]+E_{X\sim P(X)}[log\ P(x)]
$$