---
layout: post
title: "Streamlit app for Digit Recognition"
date: 2024-12-09
tags: ["Machine Learning"]
---

Digit Recognition is a fundamental vision problem involving image classification

---

### Data Source
- MNIST Data is used inherently as a PyTorch Dataset

--- 
### Streamlit App
- The app is started by running 

```
streamlit run app.py
```

- This is the landing page
<img src="{{site.url}}/images/mnist/landing_page.png"/>

- We can upload an image using the upload button in the sidebar
<img src="{{site.url}}/images/mnist/upload.png"/>

- Uploaded Image is rendered on the page with the Digit Predicted along with the Model Confidence
<img src="{{site.url}}/images/mnist/pred.png"/>

- For some image, the prediction might be wrong as shown below
<img src="{{site.url}}/images/mnist/pred_wrong.png"/>

- In such cases, we can supply correct label to retrain the model on this data
<img src="{{site.url}}/images/mnist/retrain.png"/>

---
### Reference

Implementation and code can be found on Github
- [Digit Recognition](https://github.com/gouherdanish/mnist_classification)

---
### Conclusion
- We created a Streamlit app for Digit recognition in uploaded images

