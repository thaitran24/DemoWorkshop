# Create a website for sales
# Demo Basic Commands (Before Exploring File State)
Create a new website named `index.html`.

In VSCode, to create a new template html file, type
```
html:5
```
Then press `Enter` to generate a new html template. 

## Add navigation bar
First access [Bootstrap5](https://getbootstrap.com/docs/5.0/getting-started/introduction/) to get template.

Then add stylesheet for Bootstrap5 in `<head>` and `<body>`.

Create new tag `<header>`. Search for `Navbar` in Bootstrap5 website, pick one template, then paste in the `<header>` tag.

Then perform basic command. First add the file using:
```
git add index.html
```

Then commit the file with:
```
git commit -m "header"
```

Then see the status of the file via:
```
git status
```

# Demo Basic Commands (After Exploring File State)
## Add some heroes
Search for `Heroes` then press `Enter` in Bootstrap5 website, pick one hero section by `F12` and copy the template to `<body>`.

Then perform basic command. First add the file:
```
git add index.html
```

Then commit the file with:
```
git commit -m "hero"
```

Then add logo for the heros and performing commit. But now we don't want to create new commit message, we can use:
```
git commit --amend
```

## .gitignore
We want to add new logo for the heroes. But we don't want to include test image in our working directory.

Create new file `.gitignore`, then use:
```
*.png
```
to ignore all png file when adding.

## Add some features
Search for `Features` then press `Enter` in Bootstrap5 website, pick one feature section by `F12` and copy the template to `<body>`.

Then perform basic command. First add the file:
```
git add index.html
```

Then commit the file with:
```
git commit -m "feature"
```

But now we find out that the feature section is not beauty as we though. But what do we do? `Ctrl + Z` or delete the part then commit again? We can undo our commit by
```
git reset <commit_hash>
```
or we can reset a file by
```
git reset <commit_hash> <file_name>
```

This reset the state of working directory to a particular commit but do not delete the update part.

To reset and delete the part, use `git reset --hard <commit_id>` or `git checkout <commit_hash> -- <file_name>`

# Demo Branch
Now we want to create product page but not doing with our current work.

Create a new website named `product.html`. Copy the navigation bar like `index.html`.

## Create new branch
We now create new branch for product using:
```
git branch product
```

Switch to new branch with:
```
git checkout product
```

## Add album
Search for `Album` then press `Enter` in Bootstrap5 website, pick one album section by `F12` and copy the template to `<body>`.

## Fast forward
Now we switch to branch `master` and perform merge with
```
git merge product
```

## Three-way merge
Switch to branch product, add some heroes before album in `product.html` similar to add heroes above and commit.
Then switch to master, add something new to `index.html` and commit.
Then merge with product
```
git merge product
```

## Conflict
Switch to branch product, add some heroes before album in `product.html` similar to add heroes above and commit.
Switch to branch master, do the same.
Then merge with product
```
git merge product
```

We see a conflict here because git don't now what version to choose. Pick one version with VSCode and then add the file and commit again to save the merge:
```
git add product.html
git commit -m"conflict resolved"
```

# Demo Stash
Now we doing with `product.html`, we haven't finished yet to commit but we need to switch to main and do something. So we need to save the current state of our branch with stash:
```
git stash
```

To view all the stash we have:
```
git stash list
```

To get the stash we have many ways:
1. Restore the work in the latest stash and remove it:
```
git stash pop
```
2. Restore the work in the latest stash:
```
git stash apply <stash_code>
```