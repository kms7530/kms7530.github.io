---
title: Multi GPU 학습의 필요성과 pytorch에서의 방법
date: 2023-03-16T10:34:00Z
draft: false
katex: true
---

# Multi GPU 학습의 필요성

- 이전의 모델의 경우 비교적 적은 데이터의 양을 요구했다.
- 그러나 현재에 와서 GPT-3의 $16 \times 10^{10}$ 크기의 파라미터와 이전과는 비교할수 없는 데이터를 가지고 학습되고 있다.
- 이러한 이유로 인해 속도와 하드웨어의 한계를 계선하기 위해 Multi GPU를 이용하게 되었다.

# 다중화 방식

- 다중화에는 Model 혹은 Data를 분화하는 두 가지 방식이 있다.

## Model Parallel

- 모델 자체를 적절한 부분에서 나누어 GPU에 수행시키는 방법이다.
- 이는 GPU의 메모리가 부족하던 이전부터 존재했다.
    - 가장 대표적인 예시가 AlexNet 이다.
- 이 기법은 모델의 병목, 파이프라인 설계의 어려움으로 인해 상당히 어려운 방식이다.
- 아래는 두 개의 GPU를 이용하여 병렬처리는 하는 방법이다.

![Untitled](/Multi%20GPU%20%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%E1%84%8B%E1%85%B4%20%E1%84%91%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%AD%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20pytorch%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%E1%84%8B%E1%85%B4%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20f72196b5d3224ea7b9161488f2c1a1bf/Untitled.png)

- 코드는 아래와 같다.

```python
class ModelParallelResNet50(ResNet):
	def __init__(self, *args, **kwargs):
		super (ModelParallelResNet50, self).__init__(
				Bottleneck, 
				[3, 4, 6, 3], 
				num_classes=num_classes, 
				*args, **kwargs)

		# 첫 번째 모델을 GPU 0에 할당. 
		self.seq1 = nn.Sequential(
			self.conv1, self.bn1, 
			self.relu, self.maxpool, 
			self. layer1, self.layer2).to('cuda:0')

		# 두 번째 모델을 GPU 1에 할당. 
		self.seq2 = nn.Sequential(
			self.layer3,
			self.layer4, 
			self.avgpool).to("cuda: 1")

		# 두 모델을 병합. 
		self.fc.to("cuda:1")

	def forward(self, x):
		× = self.seq2(self.seq1(x).to('cuda: 1'))
		return self.fc(x.view(x.size(0), -1))
```

## Data Parallel

- 데이터를 분할하여 GPU에 학습을 수행시킨 후 결과를 취합하여 평균을 구하는 방식이다.
    - 이는 MiniBatch의 수식과 유사한데, 차이점은 단일 GPU에서 다중 GPU로 병렬처리를 한다는 점이다.
- 이는 위의 Model Parallel과 비교적 쉬운 방식이다.

### DataParallel

- 단순히 데이터를 분해한 후 결과에 대해 평균을 취한다.
    - 해당 방식의 문제로 GPU 사용이 한 GPU에 몰릴수 있다는 점이다.
- 아래는 작동방식을 설명한 그림과 예시 코드다.

![Untitled](/Multi%20GPU%20%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%E1%84%8B%E1%85%B4%20%E1%84%91%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%AD%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20pytorch%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%E1%84%8B%E1%85%B4%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20f72196b5d3224ea7b9161488f2c1a1bf/Untitled%201.png)

```python
parallel_model = torch.nn.DataParallel(model) 	# Encapsulate the model
predictions = parallel_model(inputs)			# Forward pass on multi-GPUs
loss = loss_function(predictions, labels) 		# Compute loss function
loss.mean().backward()							# Average GPU-losses +backward pass
optimizer.step()								# Optimizer step
predictions = parallel_model(inputs) 			# Forward pass with new parameters
```

### DistributedDataParallel

- 각 CPU마다 프로세스를 생성하여 개별 GPU에 할당한다.
    - 이는 Forward / Backward 계산을 수행 후 gradient를 취합하여 평균을 낸다.

```python
def main():
    ngpus_per_node = torch.cuda.device_count()
    world_size = ngpus_per_node
 
    torch.multiprocessing.spawn(main_worker, nprocs=ngpus_per_node, args=(ngpus_per_node, ))

def main_worker(gpu, ngpus_per_node):
    image_size = 224
    batch_size = 512
    num_worker = 8
    epochs = 1

    batch_size = int(batch_size / ngpus_per_node)
    num_worker = int(num_worker / ngpus_per_node)

    torch.distributed.init_process_group(
            backend='nccl',
            init_method='tcp://127.0.0.1:3456',
            world_size=ngpus_per_node,
            rank=gpu)

    model = baseline.ResnetModel()
    torch.cuda.set_device(gpu)
```
