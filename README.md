# Create a To-do app

# Basic Commands (Before Exploring File State)

Create a new file named `README.txt` and add some texts. For instances:

```
To-do app

Author: HCMUT
```

We initialize our Git project using the command:

```
git init
```

Then perform basic command. First add the file using:

```
git add README.txt
```

Then commit the file with:

```
git commit -m "Create README.txt"
```

Then see the status of the file via:

```
git status
```

# Demo Basic Commands (After Exploring File State)

## Add Usage

Append section **Usage** into `README.txt`:

```
Usage:
- Note everyday tasks
- Track progress
- Due day
```

To commit our changes. First, we add the file:

```
git add README.txt
git commit -m "Add section Usage"
```

Running `git log` now would show our Git history as follow:

```
commit b353032bda22c5f0dd9a81d978e1a9c532945133
Author: TuanKietTran <kyletran101.work@gmail.com>
Date:   Fri Jul 22 13:37:24 2022 +0700

    add section Usage

commit 49621bc977b46bdc98419593d2107c77a7f5f312
Author: TuanKietTran <kyletran101.work@gmail.com>
Date:   Fri Jul 22 13:30:46 2022 +0700

    create README.txt
```

(The result on your machine may be different from this).
Every commit is composed of 4 parts:

- On the first line, `commit` show the ID of our commit in hexadecimal. We can later use it to identify the commit that we need.
- The next line is about the **Author** who made the commit, including their email.
- The third one is the date and time the commit was made.
- The final line is the message of that commit.

For better visualization, we can run the command:

```
git log --oneline --graph
```

`--graph` will display our commit history in a graph-style
`--oneline` means compress our commit message into a single line
`--all` is showing all branches

The result will be easier to understand:

```
* b353032 add section Usage
* 49621bc create README.txt
```

Since there is a typo in our last commit (~~`day`~~ `date`), we will fix it using amend

```
Usage:
- Note everyday tasks
- Track progress
- Due date
```

Amend using the following code:

```
git add README.txt
git commit --amend
```

Continue on, we add some features that our To-do app will support:

```
Feature:
- Feature 1
- Feature 2
- Feature 3
...
```

## .gitignore

During our app development, we may want to store some secrets / configurations that is local from our Git repo. Imagine we have a `.env` file as follow:

```
API_KEY=1234567890
PORT=3000
```

Maybe we have a logging folder, our directory structure would be follow

```
├── logs
│   ├── log1.log
│   ├── log2.log
│   └── ...
├── .env
├── README.txt
```

To prevent Git from tracking these unwanted file, we can create a `.gitignore` file that describe our **ignore** files:

```
.env
logs/

# For mac user
.DS_Store
```

If we use a dedicated IDE / code editor, it will grey down these file to indicate that these are ignore files.

| ❗️ Caution |
| ----------- |

We may accidentally add one or more unwanted files to staging area. To remove these files, we use:

```
git rm --cached <file1> <file2>
```

# Branching

## Create new branch

To allow development without affecting our project mainstream, we can create a new branch (aka branching) for our new feature.

```
git branch feature1
```

Switch to newly created branch with:

```
git checkout feature1
```

These 2 commands are so common in Git that there is a shortcut for this:

```
git checkout -b feature1
```

## Add feature1 -- Fast-Forward merge

Create `feature1.txt` with the following content:

```
The quick brown fox jumps over the lazy dog
```

Then, to add and commit our changes, use:

```
git add .
git commit -m "Create feature1.txt"
```

Our boss want to change the color from **brown** to **red**. So, let's make it happen:

First, modify `feature1.txt`:

```
The quick red fox jumps over the lazy dog
```

Then, add and commit our changes:

```
git add .
git commit -m "Change fox color"
```

Feature1 is now complete, we might want to merge it back to our `main` branch.

First, we need to checkout to the branch that we want to merge into, i.e: `main`

```
git checkout main
```

Then run

```
git merge feature1
```

`feature1` will then be merge into main using fast-forward method.

## Add feature2 -- Three-way merge

To begin, create and checkout to our new branch:

```
git checkout -b feature2
```

Create `feature2.txt` as follow:

```
May the force be with you.
```

Then, commit our changes:

```
git add .
git commit -m "Create feature2.txt"
```

Your boss now want to change from **dog** to **cat** :) Let's do it.

Checkout to our main branch:

```
git checkout main
```

Then, modify `feature1.txt` as follow:

```
The quick red fox jumps over the lazy cat
```

Commit our change and we are done:

```
git add .
git commit -m "From dog to cat"
```

`git log --all --oneline --graph` shows that our Git graph will be diverged into 2 branches: `main` and `feature2`

`merge` command now would be a **three-way merge** instead of a **fast-forward**.

```
git merge feature2 -m "Add feature2"
```

A new commit is created with parents of both `master` & `feature2`

## Add feature3 - Stash & Conflict

To create another feature, i.e, `feature3`, create and checkout to this new branch:

```
git checkout -b feature3
```

Create `feature3.txt` with the content:

```
To infinity, and beyond!!!
```

Because `README.txt` doesn't list this new feature, we need to include it as well:

```
- Features 1
- Features 2
- Features 3
```

We also want a new usage for this feature, so our **Usage** section would be

```
Usage:
- ...
- Embed files
```

But your changes aren't ready to be committed (testing, reviewing, ...). At the mean time, your boss want you to add a new usage at the `master`. Checkout to `master` also brings all the changes that we have made before, which make it harder to add/commit. You could `stash` your changes before checkout.

```
git add .
git stash -m "Create feature3"
```

Then checkout to `master` and handle your boss task:

```
Usage:
- ...
- Embed links
```

Add and commit our change to `master`

```
git add .
git commit -m "Add new usage"
```

Return to `feature3` to finish our job:

```
git checkout feature3
git stash apply
```

Add and commit our new feature:

```
git add .
git commit -m "Create feature3.txt"
```

To remove our save stash, run:

```
git stash pop
```

`git log` now show us 2 branches.

### Merge and resolve conflict

After our development, we checkout back to `master` to perform merge:

```
git checkout master
git merge feature3
```

A conflict will now occur because the last line of **Usage** section, `README.txt` is conflicted (both are add with a new line)

In VSCode, we can choose which changes from a commit that we want to keep using the built-in link. In this case, we will choose `Accept Both Changes`.

We have now successfully resolved a conflict. We can now re-commit our changes:

```
git add .
git commit -m "Resolve conflict"
```
