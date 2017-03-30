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

# Log

Show commit logs

Command :

```
$ git log
```

Output Example

```
commit 9859b29219958ab049ec8047624f36fd73f4c408
Author: Muhammad Ridho K. Pratama <99ridho@gmail.com>
Date:   Tue Mar 28 17:09:21 2017 +0700

    Update README.md

commit 3ae7aa8133c235b51771bff26b7e6c2d75b3e859
Author: Muhammad Ridho K. Pratama <99ridho@gmail.com>
Date:   Tue Mar 28 17:04:59 2017 +0700

    Update README.md

.. truncated for space ..
```

---

# Diff

Show changes between commits, commit and working tree, etc

Command : 

```
$ git diff
```

Output example :

```diff
diff --git a/test.txt b/test.txt
index d0b9b5a..689d72e 100644
--- a/test.txt
+++ b/test.txt
@@ -1 +1,2 @@
 Init file kosong
+Halo ini mencoba menambah string kososng
```

To compare between commits, you can write command as follows

```
$ git diff x_commit_hash y_commit_hash
```

This command is used for comparing between X commit to Y commit.

---

# Managing Remote Repositories

You can manage remote repository (repo from github, gitlab, bitbucket, etc) in Git easily.

Commonly used for collaboration with other developers and syncing local repo with remote repo.

#### List Remote Repository

Command :

```
$ git remote -v
``` 

Example Output

```
origin	https://github.com/99ridho/talks.git (fetch)
origin	https://github.com/99ridho/talks.git (push)
```

---

#### Adding Remote Repository

Command :

```
$ git remote add remote_name remote_url
```

Change `remote_name` and `remote_url` according to your needs

#### Removing Remote Repository

Command :

```
$ git remote remove remote_name
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

# Reverting Changes

Since Git is version control system, we're possible to revert our file to certain commit.

There are 3 different type of revert:

* `--soft` : The staged snapshot and working directory are not altered in any way.
* `--mixed` : The staged snapshot is updated to match the specified commit, but the working directory is not affected. This is the default option.
* `--hard` : The staged snapshot and the working directory are both updated to match the specified commit.

Command :

```
$ git reset (type) (commit_hash) 
```

---

# Push

Push your changes to remote repository.

Command : 

```
$ git push (remote_name) (branch_name)
```

PS: before push, make sure your working tree is clean.

---

# Pull

Pull changes from remote repository.

Command : 

```
$ git pull (remote_name) (branch_name)
```

PS: before pull, make sure your working tree is clean.

---

# Branching

Commonly used for separating your codebase.

* Adding new feature without breaking production codebase;
* Adding hotfix;
* etc...

Each branch has different commit history, and one repository can have one or more branches.

---

Branching ilustration (credit to: nvie.com)

.center[![:scale 60%](http://nvie.com/img/git-model@2x.png)]

---

Assume our commit graph look like below (`*` active branch was `master`)

```
                      *master
                         |
-->(c1)-->(c2)-->(c3)-->(c4)

```

To create new branch, use this command:

```
$ git branch (new_branch_name)
```

For example, we're going to create new `develop` branch

```
$ git branch develop
```

And now our commit graph look like below

```
                      *master
                         |
-->(c1)-->(c2)-->(c3)-->(c4)
                         |
                      develop

```



---

Then we're going to checkout to `develop` branch

```
$ git checkout develop
```

Now active branch changed to `develop`.

```
                       master
                         |
-->(c1)-->(c2)-->(c3)-->(c4)
                         |
                      *develop
```

If we're create new commit named `c5`, now our commit graph look like below

```
                       master
                         |
-->(c1)-->(c2)-->(c3)-->(c4)-->(c5)
                                |
                             *develop
```

---

If we're checkout `master` branch, then we're create new commit named `c6`, our commit graph look like below

```
                          *master
                             |
                         -->(c6)
                         |
-->(c1)-->(c2)-->(c3)-->(c4)-->(c5)
                                |
                              develop
```

So, commit history per branch as follows

```
master  : c1, c2, c3, c4, c6
develop : c1, c2, c3, c4, c5
```

---

# Merge

Commonly used to merge branches, merging PRs, merge new feature to `master`, etc..

Typically, there are 2 different type of merging

* Fast-forward merge
* Merge with merge commit

.center[![:scale 50%](http://nvie.com/img/merge-without-ff@2x.png)]

---

But, I like to merging branches with merge commit, to retain actual branching history :D

```
master  : -->(c1)-->(c2)-->(c3)-->(c4)-->(c6)-------------->(c10)
                                   |                         ^
                                   |                         |
develop :                          -->(c5)-->(c7)-->(c8)-->(c9)


c10 are merge commit from develop, to master
```

This situation can be achieved by using this command

```
$ git checkout master
$ git merge --no-ff develop
```

---

class: center, middle

# Resolving Conflicts

---

# Conflict causes

Case study :

Our repo, consist of 2 files, a.txt and b.txt. And, we have 2 different branches, called master and develop. There are 2 contributors, me and John.

* I modify a.txt at line number 1, then I commit my changes at master branch.

* John create a new branch, so called dev branch, then modify a.txt at line number 1, then commit his changes at dev branch.

* Then I modify a.txt at line number 1 for the second time, then I commit my changes at master branch.

* Okay, then I like to merge dev branch to my master branch.

* Then, merging failed cause by conflicts.


---

# Let's reproduce the conflicts

```
// pwd at learn-git repo

touch file.txt
echo "hello world" > file.txt
git add .
git commit -m "init 1st commit at master"
git checkout -b other_branch
echo "this file altered at other branch" > file.txt
git add .
git commit -m "first commit at ob"
git checkout master
echo "this file altered again at master" > file.txt
git add .
git commit -m "2nd commit at master"
git merge --no-ff other_branch
```

You'll get an error message

```
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

---

And your txt will look like this

```diff
<<<<<<< HEAD
this file altered again at master
=======
this file altered at other branch
>>>>>>> other_branch
```

---

# How to solve that conflict?

Use `git mergetool` to resolve our conflict, or resolve the conflict by hand.

```diff
<<<<<<< HEAD
this file altered again at master
=======
this file altered at other branch
>>>>>>> other_branch
```

Which changes you'll to apply? It depends to your need :)

After resolving the conflict, for example I applied changes from `other_branch`

```
this file altered at other branch
```

Commit your changes.

```
$ git commit
```

Conflict resolving has been completed :D

---

class: center, middle

# Thank You :)

any questions?
