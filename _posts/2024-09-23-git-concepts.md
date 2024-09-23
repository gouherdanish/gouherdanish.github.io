---
layout: post
title: "Git Commands"
date: 2024-09-23
tags: ["Git"]
---

Git is an important tool when it comes to software development.

---

## Introduction

- Git is used in all the avenues and steps of software lifecycle
- Knowledge of version control using Git is of paramount importance for any software engineer

## Concepts

### Creating a Git Repo

- Let's say a business requirement comes and we need to start implementing a new feature or service
- There can be two approaches for this
    - We can create a fresh git repo from start itself, then clone it locally and then start development
        - This usually is standard operating procedure in conventional SDE teams.
    - We can start development locally first and then after we have a proof of concept ready, we think of scaling it into a service. At this time, we think of creating a repo and committing the code.
        - This is the case of most Data Science, Data Analytics or Business Analytics teams where engineers do lot of experimentations locally before committing their code
        - PS - Although this can be taken care by branching out (we will discuss branching later)

#### Approach 1 - Starting afresh
- When we have not yet started working on a feature


#### Create a git repo on remote

- Login to your github account
- Click New Repository
- Give suitable and unique name of repository and Click create

- Create a local repo

```
mkdir my-repo
cd my-repo
```

- After this we creat

#### Initialize a git folder

```
git init
```
