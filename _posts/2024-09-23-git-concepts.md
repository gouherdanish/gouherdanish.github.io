---
layout: post
title: "Git: Creating a Repository"
date: 2024-09-23
tags: ["Software Development"]
---

---
### Introduction

- Git is an important tool when it comes to software development.
- Git is used in all the avenues and steps of software lifecycle
- Knowledge of version control using Git is of paramount importance for any software engineer

---
### Background
- Let's say a business requirement comes and we need to start implementing a new feature or service
- There can be two approaches for this which we discuss in detail below

---
### 1. Creating repo first then starting development 
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
### 2. Starting developement first then creating repo

- In second approach, we can start developing locally first and then after we have a proof of concept ready, we think of converting it into a repository and committing the code to Github.
- This is the case of most Data Science, Data Analytics or Business Analytics teams where engineers do lot of experimentations locally before committing their code
- PS - Although this can be taken care by branching out (we will discuss branching later)

**Step 1 - Start Development**

- We create a new directory and cd to it

```
mkdir my-unique-repo
cd my-unique-repo
```

- Then we use VS code editor and start development

```
code .
```

**Step 2 - Initialize a git folder locally**

- Say we have completed lot of experimentations and our PoC is ready
- We can use Git to track this codebase even now
- Go to your local terminal and cd to your local codebase
- To enable git to track the development in this directory, we initialize local git here

```
git init
```

- PS: Ensure `git` is installed before doing this

**Step 3 - Create Repo in Github Account**

- Login to your github account
- Click New Repository
- Give suitable unique name of repository (say _my-unique-repo_)
- Click create

PS - Note the `git remote add origin ...` which we will use in Step 4

**Step 4 - Add remote origin**

- We have created repo locally and on Github remote as well but we have yet to connect the two
- To achieve this, we add remote origin which lets the local git know that where it needs to commit the code remotely

```
git remote add origin <...>
```

---
## Conclusion

- We explored two ways of creating Git repositories
- Having knowledge of the different ways of creating a repo can help developers master version control and enable greater flexibility in software development
