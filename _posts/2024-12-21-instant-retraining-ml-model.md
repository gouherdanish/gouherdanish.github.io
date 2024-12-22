---
layout: post
title: "Instant Retraining of ML Models"
date: 2024-12-21
tags: ["Machine Learning"]
---

Retraining is crucial to enable model to keep learning overtime.

---

### Background

- Earlier we had created our own [Digit Recognizer App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html) which has two features
    - it can recognize the single digit present in an image
    - if a given image is classified incorrectly, we can supply the correct label and it can retrain the model as shown below

<img src="{{site.url}}/images/mnist/pred_wrong.png"/>
<img src="{{site.url}}/images/mnist/retrain.png"/>

- In this blog, we will understand how we have built the retraining feature in our app

### Approach

1. Read Misclassfied Image and Provided Correct Label
2. Apply Transforms and prepare DataLoader
3. Load Model weights
4. Perform Model Training over the supplied image with correct label
5. Save Model 

---

### Step 1 - Read Misclassified Image

- Images are read in mainly two ways 

1. Live App
    - Image file is uploaded on the Streamlit App by the user
    - The uploaded image is converted to numpy array and then processed further

```
uploaded_file = st.sidebar.file_uploader("Upload an image file", type=["png", "jpg", "jpeg"])
file_bytes = np.asarray(bytearray(uploaded_file.read()), dtype=np.uint8)
image = cv2.imdecode(file_bytes, cv2.IMREAD_COLOR)
```

2. Local Testing
    - While running the code locally, a local path can be provided by the user in the command line arguments 

```
parser = argparse.ArgumentParser()
parser.add_argument('--img',type=str,required=True,help='path of test image in case of incremental inference')
parser.add_argument('--label',type=int,required=True,help='path of test image in case of incremental inference')
```

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