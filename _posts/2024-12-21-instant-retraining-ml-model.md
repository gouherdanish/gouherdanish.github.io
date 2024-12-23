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

0. Model misclassifies an image
1. Read Misclassfied Image and Provided Correct Label
2. Apply Transforms and prepare DataLoader
3. Load Model weights
4. Model Retraining
5. Save Model 

---

#### Step 0 - Model misclassifies an image

<img src="{{site.url}}/images/mnist/pred_wrong_7.png"/>

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/7a.png
{'confidence': tensor([0.5713]), 'pred_label': tensor([3])}
```

---

#### Step 1 - Read Misclassified Image

- Images are read in mainly two ways 

1. Live App
    - Image file is uploaded on the Streamlit App by the user
    - The uploaded image is converted to numpy array which can be read by the 

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

- Next we create `SingleDataPreparation` class which can handle reading single image either from a path or numpy array as shown below

```

class SingleDataPreparation(DataPreparation):
    def __init__(
            self,
            img:Union[str,Path,np.ndarray],
            label:int=-1) -> None:
        ...

    def _load_data(self):
        """
        Function to load data into a PyTorch Dataset object
        - if path string is provided, it uses cv2 to read the image from the path into a numpy array
        - if numpy array is provided directly, it is used as is
        """
        if isinstance(self.img,str):
            self.img = Path(self.img)
        if isinstance(self.img,Path):
            self.img = cv2.imread(self.img,cv2.IMREAD_GRAYSCALE)
        return SingleDataset(
            img=self.img,
            label=self.label,
            transform=self.transform
        )
```

Refer [retraining.py](https://github.com/gouherdanish/mnist_classification/blob/main/retraining.py)

---
#### Step 2 - Prepare DataLoader

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

- Using the above class, we create a DataLoader which will be fed to the model later

```
dataset = SingleDataset()
loader = torch.utils.data.DataLoader(
    dataset=dataset, 
    shuffle=False, 
    batch_size=1)
```

--- 

#### Step 3 - Load Model weights

- First, we instantiate up the model 

```
from model.lenet import LeNet
model = LeNet()
```

- Then, we load the model weights from the last checkpoint

```
class ModelTraining:
    def __init__(self,model,...):
        ...
        self.checkpoint_path = PathConstants.MODEL_PATH(model.model_name)
        self.checkpoint = ModelCheckpoint.load(checkpoint_path=self.checkpoint_path)
        if self.checkpoint:
            self.last_epoch = self.checkpoint['epoch']
            self.model.load_state_dict(self.checkpoint['model_state'])
            self.optimizer.load_state_dict(self.checkpoint['optimizer_state'])
```
---
#### Step 4 - Model Retraining

- In this step, we perform Model Training over the misclassfied image with correct label
- First we pass the image forward through the model
- Next, we find the Cross entropy loss and do backpropagation to find gradients
- Finally we update model weight

All these steps are encapsulated in this function below

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

---

#### Step 5 - Save Model 

- Model is saved so that modified weights can be used further
```
ModelCheckpoint.save(
    epoch=self.last_epoch+1,
    model=self.model,
    optimizer=self.optimizer,
    checkpoint_path=self.checkpoint_path
)
```

---

### Pros and Cons

**Pros**

1. Mitigate Data Drift / Model Drift

- Retraining with new data captures evolving data patterns and mitigates 

**Cons**

1. Catastrophic Forgetting:

- The model may overwrite or lose knowledge learned from older data.
- Techniques like replay mechanisms or regularization can help mitigate this.

2. Data Imbalance:

- New data might not represent the overall data distribution.

---

### Conclusion

- We explored online retraining of ML models in which we provide the capability to retrain for every mis-classified item
- We observed