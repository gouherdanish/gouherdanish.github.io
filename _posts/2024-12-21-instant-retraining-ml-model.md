---
layout: post
title: "Instant Retraining of ML Models"
date: 2024-12-21
tags: ["Machine Learning"]
---

Retraining is crucial to enable model to keep learning overtime.

---

### Background

Earlier we had created our own [Digit Recognizer App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html) which has two features

- It can recognize the single digit present in an image

<img src="{{site.url}}/images/mnist/pred_wrong.png"/>

- If a given image is classified incorrectly, we can supply the correct label and it can retrain the model as shown below
<img src="{{site.url}}/images/mnist/retrain.png"/>

In this blog, we will understand how we have built the retraining feature in our app

---
### Approach

1. Model misclassifies an image
2. Read Misclassfied Image and Provided Correct Label
3. Apply Transforms and prepare DataLoader
4. Load Model weights
5. Model Retraining
6. Save Model 

---

#### Step 0 - Model misclassifies an image

- Below we supply the handwritten image of number `7` from the App

<img src="{{site.url}}/images/mnist/pred_wrong_7a_1.png"/>

- Internally, the uploaded bytearray image is converted to numpy array as shown in code snippet

```
uploaded_file = st.sidebar.file_uploader("Upload an image file", type=["png", "jpg", "jpeg"])
file_bytes = np.asarray(bytearray(uploaded_file.read()), dtype=np.uint8)
image = cv2.imdecode(file_bytes, cv2.IMREAD_COLOR)
```

- Notice how the image is misclassified as `3` with 99% confidence

- Running the inference on this file locally also yields the same result since it is basically using the same trained model

```
>>> python single_inference.py --img ./data/sample/7a.png
{'confidence': tensor([0.9999]), 'pred_label': tensor([3])}
```

Refer [single_inference.py](https://github.com/gouherdanish/mnist_classification/blob/main/single_inference.py)

**Reason for Misclassification**

The misclassification might be attributed to two reasons

1. Underfitting - Model was not trained on wide variety of images, so it did not understand the underlying features of the new data properly

2. Overfitting - Model was too concerned or over-trained on limited training data. So, it could not generalize well when faced with any new data 

---

#### Step 1 - Provide Correct Label for Misclassified Image

- We can provide correct label for the misclassified image from the App directly and press `Use for Retraining` as follows

<img src="{{site.url}}/images/mnist/retrain_7a.png"/>

- This is invariably same as executing following command via terminal

```
python retraining.py --img ./data/sample/7a.png --label 7
```

Refer [retraining.py](https://github.com/gouherdanish/mnist_classification/blob/main/retraining.py)

Retraining entails the following steps in the code internally

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
img = cv2.imread(image,cv2.IMREAD_GRAYSCALE)
dataset = SingleDataset(img=img,label=label)
loader = torch.utils.data.DataLoader(dataset=dataset,shuffle=False,batch_size=1)
```

--- 

#### Step 3 - Load Model weights

- Next step is to instantiate the model as follows

```
from model.lenet import LeNet
model = LeNet()
```

- And then, we load the model weights from the last checkpoint that the model was saved at earlier during training

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
- First we do the forward pass over the model using the image and label
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

- For implementation details, refer [training_factory.py](https://github.com/gouherdanish/mnist_classification/blob/main/factory/training_factory.py)

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

- For implementation details, refer [model_checkpoint.py](https://github.com/gouherdanish/mnist_classification/blob/main/checkpint/model_checkpoint.py)

---

#### Step 6 - Check Inference Again

- After retraining the model with the misclassified image, we again check the inference to verify if the model has actually learned 

```
>>> python single_inference.py --img ./data/sample/7a.png
{'confidence': tensor([0.9998]), 'pred_label': tensor([7])}
```

- As observed above, the retrained model correctly classifies the image now

Note: 
- There may be cases where even after retraining the misclassified image with correct label, it is still misclassified. 
- We might need to retrain for multiple iterations on the same image

---

### Pros and Cons

**Pros**

- Mitigate Data Drift / Model Drift
    - Retraining with new data captures evolving data patterns 
    - Used as a mitigation mechanism to resolve Data Drift and Model Drift scenarios

**Cons**

- Catastrophic Forgetting:
    - There might be cases where the model might misclassify the images which it was earlier classiying correctly. This happens since the retrained model may overwrite or lose knowledge learned from older data.
    - Techniques like replay mechanisms or regularization can help mitigate this.

- Data Imbalance:
    - New data might not represent the overall data distribution.

---

### Conclusion

- We explored online retraining of ML models in which we provide the capability to retrain for one mis-classified item
- We observed the pros/cons of performing online retraining as well