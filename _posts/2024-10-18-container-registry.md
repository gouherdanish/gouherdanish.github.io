---
layout: post
title: "Using Github Container Registry "
date: 2024-10-18

tags: ["Software Development"]
---

A container registry is a repository—or collection of repositories—used to store and access container images
---

### Background of existing App

- Previously, in [this](https://gouherdanish.github.io/2024/10/07/dockerfile.html) article, we created a Dockerfile for our Streamlit [App](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html), built a docker image and ran it as a container
- However, the images that we created still reside in our local system and sharing these images to other developers is a painful task as there is no central place to store these images in one place

--- 

### Introducing Container Registry

- When developing container images, you need somewhere to save, share, and access them as they are created, and that’s where a container registry comes in
- A container registry essentially acts as a place for developers to store container images and share them out via a process of uploading (pushing) to the registry and downloading (pulling) into another system
- Container registries save developers valuable time in the creation and delivery of cloud-native applications, acting as the intermediary for sharing container images between systems

---
### Types of Container Registries

**Public Registry**
- used by individuals or small teams that want to get up and running with their registry as quickly as possible
- as organizations grow, this can bring more complex security issues like patching, privacy, and access control

**Private Registry**
- Private registries provide a way to incorporate security and privacy into enterprise container image storage, either hosted remotely or on-premises. 
- These private registries often come with advanced security features and technical support
- e.g. AWS ECR, Google Container Registry

---
### Creating a Container Registry

**Step 1 - Create Github Token**
- Go to Github -> Account -> Settings -> Developer Settings
- Click Personal access tokens -> Tokens (classic)
- Create a new Personal Access Token (PAT) giving necessary privileges
  - write:packages
  - delete:packages
  - repo

<img src="{{site.url}}/images/ghcr/pat.png">

**Step 2 - Enable Docker Login to Github**

```
docker login \
--username <your-github-username> \
--password <your-pat-created-in-step1> \
ghcr.io
```
---