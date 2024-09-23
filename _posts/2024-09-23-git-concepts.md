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
- There can be two approaches for this as shown below

#### Approach 1 - Creating repo first then starting development 
- We can create a fresh git repo from start itself, then clone it locally and then start development
- This usually is standard operating procedure in conventional SDE teams.

**Step 1 - Create a git repo on remote**

- Login to your github account
- Click New Repository
- Give suitable unique name of repository (say _my-unique-repo_)
- Click create

**Step 2 - Clone repo locally**

- Click `clone` on the Github repository you just created and copy the command
- Start terminal and `cd` to change current directory to wherever you want to start development
- Paste the command on the terminal

```
git clone <REPO_URL>
```

**Step 3 - Start Development**

- If you are using Visual Studio, then you can use following to start VS code editor and start development

```
code .
```

---
#### Approach 2 - Starting developement first then creating repo

- In second approach, we can start developing locally first and then after we have a proof of concept ready, we think of converting it into a service. At this time, we think of creating a repo and committing the code.
- This is the case of most Data Science, Data Analytics or Business Analytics teams where engineers do lot of experimentations locally before committing their code
- PS - Although this can be taken care by branching out (we will discuss branching later)

**Step 1 - Start Development**

- We create a new directory and cd to it

```
mkdir my-repo
cd my-repo
```

- Then we use VS code editor and start development

```
code .
```

**Step 2 - Initialize a git folder**

Create a local repo**


```
git init
```
