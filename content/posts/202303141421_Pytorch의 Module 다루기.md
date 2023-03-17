---
title: Pytorch의 Module 다루기
date: 2023-03-14T14:21:00Z
draft: false
katex: true
---

# nn.Module

- Module은 pytorch의 모델의 최소 단위라고 볼수있다.
- 여기서 forward, backward 계산이 일어나게 된다.
- 생성 후 객체 자체를 함수로 부르게되면 forward에 정의된 연산을 수행하게 된다.

# nn.Sequential

- 위의 Module의 forward 연산을 연쇄적으로 수행하게 해주는 함수다.
- 먼저 Sequential이 없이 코드를 작성하면 다음과 같다.

```python
class Add(nn.Module):
    def __init__(self, value):
        super().__init__()
        self.value = value

    def forward(self, x):
        return x + self.value

# y = x + 3 + 2 + 5
x = torch.tensor([1])
y = Add(3)(x)
y = Add(2)(y)
y = Add(5)(y)
```

- 하지만 Sequential을 이용하면 아래와 같이 깨끗한 코드를 작성할수 있다.

```python
calculator = nn.Sequential(Add(3), Add(2), Add(5))
x = torch.tensor([1])
output = calculator(x)
```

# nn.ModuleList

- 위의 Sequential의 경우 무조건 연쇄적으로 작동하나, 이것의 경우 일반 list 사용과 같이 콕 찝어서 사용이 가능하다.
- 또한 일반 list와 같이 append 혹은 insert가 동작한다.

```python
nn.ModuleList([Add(2), Add(3), Add(5)])
self.add_list[0] # Add(2)
self.add_list[1] # Add(3)
self.add_list[2] # Add(5)
```

- ModuleDict 또한 존재하며 일반 dict와 동일하게 사용하면 된다.

## 그냥 list로 Module을 관리하면 안되는가?

- 해도 상관 없다. 다만, 당신이 pytorch가 container에서 관리해주는 기능이 필요 없다면.
- 아래의 예시를 봐보자.

```python
class PythonList(nn.Module):
    """Python List"""
    def __init__(self):
        super().__init__()

        # Python List
        self.add_list = [Add(2), Add(3), Add(5)]

    def forward(self, x):
        x = self.add_list[1](/x)
        x = self.add_list[0](/x)
        x = self.add_list[2](/x)
        
        return x

class PyTorchList(nn.Module):
    """PyTorch List"""
    def __init__(self):
        super().__init__()

        # Pytorch ModuleList
        self.add_list = nn.ModuleList([Add(2), Add(3), Add(5)])

    def forward(self, x):
        x = self.add_list[1](/x)
        x = self.add_list[0](/x)
        x = self.add_list[2](/x)
        
        return x
```

- 위와 같이 각각 일반 list와 ModuleList를 이용해 모듈을 관리하는 Module을 만들었다.
- 각각 모듈을 생성해서 출력해보면 다음과 같이 ModuleList로 관리하는 경우 정보가 뜨지만, list를 이용한 경우 그렇지 않다.

```python
print(PythonList())
# PythonList()

print(PyTorchList())
# PyTorchList(
#   (add_list): ModuleList(
#     (0): Add()
#     (1): Add()
#     (2): Add()
#   )
# )
```

# torch.nn.parameter.Parameter

- torch.nn.parameter에 있는 class로 Module 내의 파라미터를 생성해준다.
- tensor와 같은 구조지만, 해당 변수가 “Parameter”임을 명시적으로 알려주어, pytorch가 자동으로 관리가 가능하게 한다.

# extra_repr

- 해당 함수는 Module을 구성할때 만들어주면 repr() 함수를 이용해 Module의 정보를 뽑을 때 추가 정보를 제공하게 해주는 함수이다.
- 아래의 예시는 모델의 이름을 출력하는 함수이다.

```python
# ... class 정의 생략 ...
def extra_repr(self):
    return "name=" + self.name

model = Model("duck")
model_repr = repr(model)

print(model_repr)
# Model(
#   (ab): Layer_AB(
#     (a): Function_A(name=duck)
#     (b): Function_B()
#   )
#   (cd): Layer_CD(
#     (c): Function_C()
#     (d): Function_D()
#   )
# )
```
<!-- 
# hook

# apply -->
