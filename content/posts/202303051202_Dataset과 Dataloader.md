---
title: Dataset과 Dataloader
date: 2023-03-05T12:02:00Z
draft: false
katex: true
---

# Dataset과 Dataloader의 관계

- 각각에 대한 설명은 다음과 같다.
    - Dataset: 데이터 덩어리 자체를 말한다. 데이터를 들여오게 되는 형태와 표준을 잡아준다.
        - 데이터 형태에 따라 함수를 각기 다르게 정의.
        - dataset에 대해 표준화된 방법으로 정의하는것을 권장.
        - 최근, HuggingFace 등 표준화 라이브러리를 사용
    - Dataloader: 데이터 덩어리인 Dataset을 정의한 loader에 정의한 방식대로 나누어서 제공해준다.
        - 데이터에 대해 tensor 변환과 batch를 생성해주는 역할.
        - 학습 직전에 데이터 제공까지 역할.
        - 데이터의 병렬처리에 대해 생각 후 제작 필요.

# Dataset

아래와 같은 방식으로 Dataset에 대해 정의할수 있다. 

```python
import torch
from torch.utils.data import Dataset

class CustomDataset(Dataset):
	def __init__(self, text, labels):
		self.labels = labels
		self.data = data

	def __len__(self):
		return len(self.labels)

	def __getitem__(self, idx):
		label = self.labels[idx]
		text = self.data[idx]
		sample = {"Text": text, "Class": label}

		return sample
```

# Dataloader

아래와 같은 방식으로 Dataloader에 대해 정의할수 있다. 

```python
text = ["happy", "amazing", "sad"]
label = [True, True, False]

c_dataset = CustomDataset(text, label)
dataloader = DataLoader(c_dataset, batch_size, shuffle=True)

for dataset in dataloader:
	print(dataset)
```

## sampler, batch_sampler

위의 인자값은 데이터를 가져올 때 index를 지정할 때 사용하는 인자값이다. 

## collate_fn

- 데이터의 형태에 대해 아래와 같이 반환 형태를 변환하여 준다.
    - [[D, L], [D, L] …] → [[D, D, D…], [L, L, L…]]
