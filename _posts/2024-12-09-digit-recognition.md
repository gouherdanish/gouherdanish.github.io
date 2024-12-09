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

---
### Reference

Implementation and code can be found on Github
- [Urban Flooding](https://github.com/gouherdanish/urban_flooding)

---
### Conclusion
- Urban flooding is a growing cause of concern in Bangalore
- This app aims to identify the low-lying areas in Bangalore which are at risk of Urban flooding

