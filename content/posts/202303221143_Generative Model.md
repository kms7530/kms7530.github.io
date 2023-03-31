---
title: Generative Model
date: 2023-03-22T11:43:00Z
draft: false
katex: true
---

# Generative Model?

- 쉽게 말하면, 무언가를 학습해서 model이 다시 만드는 것을 말한다.
- 조금 더 엄밀히 말하면, 데이터의 distribution에 대해 학습한다는 것이다.
    - 이때, $p(x)$를 만들게 되는데, 이 함수는 예시로, 강아지 사진을 구별하는 함수인데 사진을 넣게 되면 확률이 나오게 되는 함수다.

# 예시

- MNIST 데이터를 이용하여 예시를 들어보자.
- 먼저 MNIST의 각 픽셀을 모두 $X_1, ..., X_n$ 이라 한다.
    - 이때 경우의 수는 $2^n$개, 특정하기 위한 파라미터는 $2^n-1$개이다.
    - 이때 각 픽셀은 서로 독립적인 요소기 때문에 $P(X_1,...,X_n)$과 같은 방법으로 유의미한 값을 얻을 수 없다.
    - 그래서 다음과 같은 방식을 사용한다.
        - Chain rule: $p\left(x_1, \ldots, x_n\right)=p\left(x_1\right) p\left(x_2 \mid x_1\right) p\left(x_3 \mid x_1, x_2\right) \cdots p\left(x_n \mid x_1, \cdots, x_{n-1}\right)$
        - Bayes’ rule: $p(x \mid y)=\frac{p(x, y)}{p(y)}=\frac{p(y \mid x) p(x)}{p(y)}$
        - Conditional independence: $\text { If } x \perp y \mid z \text {, then } p(x \mid y, z)=p(x \mid z)$

## Using Chain Rule

- 기본 식은 다음과 같다.

$$
p\left(x_1, \ldots, x_n\right)=p\left(x_1\right) p\left(x_2 \mid x_1\right) p\left(x_3 \mid x_1, x_2\right) \cdots p\left(x_n \mid x_1, \cdots, x_{n-1}\right)
$$

- 위의 식을 이용하여 보았을 때 다음과 같은 갯수의 파라미터가 필요하다.
    - $P(X_1): 1$
    - $P(X_2|X_1): 2 \rightarrow P(X_2|X_1 = 0), P(X_2|X_1 = 1)$
    - $P(X_3|X_1, X_2): 4$
    - 그러므로 필요하게 되는 파라미터의 갯수는 다음과 같다.
    
$$
1+2+2^2+\cdots+2^{n-1}=2^n-1
$$
    

## Using Markov assumption

- 기본 식은 다음과 같다.

$$
X_{i+1} \perp X_1, \ldots, X_{i-1} \mid X_i
$$

- 위의 식은 다음과 같이 해석 할수 있다.

$$
p\left(x_1, \ldots, x_n\right)=p\left(x_1\right) p\left(x_2 \mid x_1\right) p\left(x_3 \mid x_2\right) \cdots p\left(x_n \mid x_{n-1}\right)
$$

- 이러한 점으로 보게되면 필요한 파라미터의 수는 $2n-1$개다.

# Autoregressive Model

- 다시 돌아와서, 위와 같이 MNIST 데이터를 예시로 들게되면 다음과 같다.
- 먼저, 각각의 픽셀은 서로 독립적인 데이터기 때문에 joint distribution을 chain rule을 이용해 생각해보자.
- 이를 이용해 식을 만들게 되면 다음과 같다.

$$
P\left(X_{1: 784}\right)=P\left(X_1\right) P\left(X_2 \mid X_1\right) P\left(X_3 \mid X_2\right) \cdots
$$
