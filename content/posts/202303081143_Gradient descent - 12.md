---
title: Gradient descent - 1/2
date: 2023-03-08T11:43:00Z
draft: false
katex: true
---

# 미분이란?

- 변수의 움직임에 따른 함수값의 변화를 측정하기 위한 도구.
- 식으로는 아래와 같이 표현.
    
$$
f\prime (x)=\lim_{h \to 0}\frac{f(x+h)-f(x)}h
$$
    
- 이를 그래프로 표현하면 다음과 같다.
    
%20%20%20%20![스크린샷%202023-03-08%20오전%2011.49.52.png](/Gradient%20descent%20-%201%202%2001279799e41b49cab8efd935ba765dbb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-03-08_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.49.52.png)
    
    - 먼저, $f\prime (x)$의 경우 기울기가 음수이다.
    - 이를 $x$에 빼게 되면 기존의 $x$ 보다 커지게 된다.
    - 이러한점은 그래프의 특성과 해당 지점의 미분값에 따라 다르게 된다.
- 이러한 미분의 특성을 이용하여 미분값을 더하게되면 경사 상승법, 빼게되면 경사하강법으로 부르며 사용한다.

# 미분의 코드화

```python
var = init
grad = gradient(var)

# esp의 경우 grad값이 0이 아닌 "0"에 가까운 값으로 나오기 때문에 
# 임이의 "사실상 0" 즉 매우 작은 값으로 할당해주어 0인지를 판단한다. 
while abs(grad) > esp:
	# 지속적으로 경사와 learning rate를 이용하여 var를 갱신한다. 
	var = var - learning_rate * grad
	# 갱신된 var를 이용해 기울기 갱신. 
	grad = gradient(var)
```

# 벡터에 대한 미분

- 이때는 편미분을 이용하여 계산하게 된다. 수식은 아래와 같다.
    
$$
\delta_{x_i}f(x)=\lim_{h \to 0}\frac{f(x+he_i)-f(x)}h
$$
    
- 위의 식을 이용하여 각 변수에 대해 하나로 모으게 되면 Gradient Vertor가 된다. 이는 아래와 같다.
    - 아래의 역삼각형은 “Nabla”로 부른다.
    
$$
\nabla f = (\delta_{x_1}f, \delta_{x_2}f, \delta_{x_3}f ... , \delta_{x_n}f)
$$
    
- 위의 수식을 이용하여 그래프로 그리게 되면 다음과 같다.
    
%20%20%20%20![스크린샷%202023-03-08%20오후%201.03.45.png](/Gradient%20descent%20-%201%202%2001279799e41b49cab8efd935ba765dbb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-03-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.03.45.png)
    
- 이를 top-view에서 2차원으로 보게되면 다음과 같다.
    
%20%20%20%20![스크린샷%202023-03-08%20오후%201.05.00.png](/Gradient%20descent%20-%201%202%2001279799e41b49cab8efd935ba765dbb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-03-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.05.00.png)
    
    - 이를 수학적으로 해석하게 되면 $-\nabla f = \nabla (-f)$ 이며, 각 점에서 가장 빨리 감소하게 되는 방향과 같다.
    - 그 방향 중 하나를 빨간색으로 표기한것이다.
- 이를 코드화 시키면 다음과 같다.

```python
var = init(x)
grad = gradient(var)

# 위의 1개의 변수와 다른 사항은 같으나, 변수가 벡터인 경우, 
# 이전 수업에서 들은것과 같이 L2 norm을 이용하여 두 점 사이의 
# 거리를 기준으로 계산한다. 따라서 norm을 사용한다. 
while norm(grad) > eps:
	var = var - learning_rate * grad
	grad = gradient(var)
```
