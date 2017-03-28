class: center, middle

# Introduction to Git

main coding BCC 2017

---

# What is Git

- Version Control System
- Developed by Linus Torvalds
- Distributed

---

# Before we start

Firstly, download Git at [https://git-scm.com/](https://git-scm.com/) (for Windows or Mac)

For linux user : can be installed via package manager (e.g apt, dnf, yast)

Usually, Git has been installed on many popular distros. 

---

# Configure Git

Simple, you only need to set your name and e-mail which included in your commit history.

```
$ git config --global user.name="your_name"
$ git config --global user.email="your_email"
```

---

# Let's init our repo!

Type this commands...

```
$ mkdir learn-git
$ cd learn-git
$ git init
```

For example, you'll get this message after initialize new Git repository

```
Initialized empty Git repository in /Users/ridho/code/learn-git/.git/
```

---

class: center, middle

# Git Key Commands

---

# Clone

Cloning the repository from another source (github, local git server, etc)

Command :

```
$ git clone repository_url
```

Change `repository_url` to git repo's URL which you'll clone.

Example :

```
$ git clone https://github.com/fulanah/awesome-repo.git
```

---

# Commit

Record file changes history (diff, mode, etc)

Command :

```
$ git commit -m "message commit here"
```

Before committing file changes, we need to add changed file to staging area

```
$ git add -i
```

This command is used to add changed/untracked file to staging area interactively.

---

# To be available soon...