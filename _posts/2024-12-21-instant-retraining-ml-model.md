---
layout: post
title: "Instant Retraining of ML Models"
date: 2024-12-21
tags: ["Machine Learning"]
---

Retraining is crucial to enable model to keep learning overtime.

---

### Approach

- Read Misclassfied Image and Provided Correct Label
- Apply Transforms and prepare DataLoader
- Load Model weights
- Perform Model Training over the supplied image with correct label
- Save Model 
---

### Prepare DataLoader

- The first point of entry is preparing the data in such a way that model can consume it
- For this, we create a new class `SingleDataset` by extending the PyTorch `Dataset` class and implement the `__len__` and `__getitem__` magic methods

```
import torch
import numpy as np
from torch.utils.data import Dataset

class SingleDataset(Dataset):
    def __init__(
            self,
            img:np.ndarray,
            label:int,
            transform=None) -> None:
        self.img = img
        self.label = label
        self.transform = transform

    def __len__(self):
        return 1
    
    def __getitem__(self, index):
        if self.transform: 
            img = self.transform(self.img)
        return img, torch.tensor(self.label)
```

- Next, we create a DataLoader

```
dataset = SingleDataset()
loader = torch.utils.data.DataLoader(
    dataset=dataset, 
    shuffle=False, 
    batch_size=1)
```

### Model Retraining

- 

```
class SingleTraining(ModelTraining):
    def _train_loop(self,train_loader):
        self.model.train()
        X,y = next(iter(train_loader))
        pred = self.model(X)
        print(pred,y)
        loss = self.loss_fn(pred,y)
        self.optimizer.zero_grad()
        loss.backward()
        self.optimizer.step()
```