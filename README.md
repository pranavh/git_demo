# git_demo
Play with this repository to learn git

## Starting with git/github

So you've decided you want to improve life with version control. Yay! Step 0: done. This readme is a quick guide to get you up and running with the basics of Git(the actual version control software) and Github(one of the many remote hosts for git repositories). For more advanced stuff, or if something goes wrong, Google is your best friend..

## TL;DR

If you don't have the patience to read through, or already kinda sorta know your way around git, this summary should refresh your memory.

![If that doesn't fix it, ask a Magic 8-ball. Or Jeeves. Yeah, I know that isn't xkcd's original alt text, but I _really_ want you to find out yourself.](readme-img/git.png)

<sup>[Source](https://xkcd.com/1597/)</sup>

Memorize these commands:

You'll use these very often:
```bash
git add * # Adds all changes not excluded by gitignore to Index
git rm <file-to-delete> # Removes a file from working tree and Index (Use this to delete files)
git mv <file-name> <new-name> # Moves a file.
git commit -m "Commit message" # Commits changes to HEAD
git push <remote-name> <branch-name> # or simply, git push. Pushes <branch-name> (or current local branch) to <remote-name> (default: origin).
```

You'll use these once in a while:
```bash
git clone <remote-url> # clone repo at remote-url
git status # Check status. Very useful
git checkout -b <branch-name> # make new branch, switch to it
git checkout <branch-name> # switch to existing branch
git branch -d <branch-name> # Delete local branch
git push <remote-name> --delete <branch-name> # Delete remote-name/branch-name
```

You'll use these if you're pulling other people's code / resolving conflits:
```bash
git pull <remote-name> <branch-name> # pulls remote-name/branch-name, merges it into current local branch
git merge <branch-name> # Merges local branch-name into current local branch
git merge --continue # Continues merge process if it was previously interrupted to resolve conflicts
```

![Just kidding, it's just more of the same](readme-img/different.gif)

### Introduction
Git keeps track of your files using three "trees". The first is your working directory<sup>haha, get it? Direc<i>tree</i></sup>. This is where you make changes to all your files. Inside your working directory, git creates a hidden directory called `.git` where it keeps track of your changes. **Do _not_ mess with the `.git` directory.**

Second, is the "Index", which is a staging area for changes until you've decided you have enough to commit. This is where your changes go when you use `git add` .


Third, the "HEAD". This keeps track of the latest commit you made.

These three trees make up your "local repository". Just like your local repository, there's a "remote repository" that lives on a server somewhere out there. Let's call the remote repo "origin". Why? Because I said so.

### Step 0.5: Install and set up git

[Installation instructions are here.](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) \
[First-time setup instructions are here](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

More help for any command is available online, or by doing
```bash
$ git help command
e.g.: git help commit
```

### Step 1: Create your repository
You first need to create a repository locally and connect it to the origin. To do this, first pick a name for your repository. Then, create your working directory and navigate into it. Then, initialize git.

```bash
$ mkdir wow-such-repo
$ cd wow-such-repo
$ git init
```

Now, create your remote repository on github.com or equivalent, then link it to the local repo like so:

```bash
$ git remote add <remote-name> <remote-url>
```

`<remote-name>` is whatever you want it to be, but it's conventionally called `origin`.

You can get the remote url from the "Clone or download" button on your repository's github page. It's usually something like `git@github.ncsu.edu:username/wow-such-repo.git` or `https://github.ncsu.edu/username/wow-such-repo.git`

At this stage, you have an empty local repo and an empty origin, and _local_ knows _origin_ exists.

#### Shortcut
Create a remote repository first. Remember to add a readme file (github's interface allows you to add one while creating the repository). Then, simply do

```bash
$ git clone git@github.ncsu.edu:username/wow-such-repo.git
```
This clones the remote repository into `./wow-such-repo`. The clone command already adds a link to the remote repository and calls it origin, so you should be good to go

### Step 2: Write code
How to write code is outside the scope of this tutorial.

![Program, n. A magic spell cast over a computer allowing it to turn one's input into error messages; v. tr. To engage in a pastime similar to banging one's head against a wall, but with fewer opportunities for reward.](readme-img/monkeys.gif)

If you did something wrong, you can undo those changes to the latest HEAD using
```bash
$ git checkout -- <filename>
```

To drop all your local changes and revert to the remote copy, do
```bash
$ git fetch <remote-name>
$ git reset --hard <remote-name>/<branch>
```

### Step 3: Propose changes
Once you've made a change, you can add it to Index by
```bash
$ git add <filename>
```

[Wildcards](http://www.linfo.org/wildcard.html) work like you expect them to, so do `git add *` to add all changes to all files, for example.

After you're more or less confident that you want to keep a set of changes, you can commit them to the HEAD. Do this by
```bash
$ git commit -m "Commit message"
```

Commit messages should be meaningful descriptors of what has changed in this commit. This is for your own good.


Now, the changes you added are committed to HEAD. You still need to push them to origin though. Do that by
```bash
$ git push -u origin master
```

This tells git to push the current HEAD commit to the `master` branch at the remote location `origin`.
You only need to specify the remote location and the branch the first time you push to that branch. Subsequently, just `git push` will get the job done.

### Other fancy stuff
That previous workflow is enough to work with git, but only if you want a very basic version control / backup system. Learning this fancy stuff allows you to do some useful stuff, like comparing different versions of code, (and some other things too, but I can't quite remember them now).

#### 1. Branching
Creating a branch allows you to develop code isolated from the main code. Until you merge the branch with the master branch, you essentially have two separate versions of the code: the latest version, in the child branch, and the version in the master branch from before you created the child branch. Create a branch and switch to it using
```bash
$ git checkout -b <branch-name>
```

![Branching](readme-img/atlassian-assets/branch.svg)
Figure: Branching creates a new branch that is isolated from its parent.<sup>[Image source](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)</sup>

To switch to a branch, do
```bash
$ git checkout <branch-name>
```

To switch back to the master branch, do
```bash
$ git checkout master
```

A newly created branch isn't yet linked to anything on `origin`, so the first time you push this branch, you'll need to type the full `push` command.
```bash
$ git push -u <remote-name> <branch-name>
```
Subsequently, you can get by with
```bash
$ git push
```

Merge the changes in a branch back into master using
```bash
$ git checkout master
$ git merge <branch-name>
```

![Merging](readme-img/atlassian-assets/merge-branch.svg)
Figure: Merging joins the edit history of the new branch to the base branch.<sup>[Image source](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)</sup>

If you have merge conflicts, git will tell you. Check which files conflict using
```bash
$ git status
```
When you open these files, you'll see git has marked the conflicts using the markers `<<<<<<<` and `>>>>>>>`. Look for them, fix the conflicts, and save the file. Good editors like VS Code (I use this to write python and C++) have a nice way to show you the conflicts and resolve them, but <kbd>Ctrl</kbd>+<kbd>F</kbd> in any editor will do the job.

After you've fixed conflicts, do
```bash
$ git merge --continue
```

Although this shouldn't happen very often if you're doing everything correctly, you might have to resolve multiple conflicts per merge.

Ideally, you want to pull the latest `origin/master` before merging into it. To do this,
```bash
$ git pull origin master
```

Once you're done with a branch, you can delete it locally using
```bash
$ git branch -d <branch-name>
```

This does __NOT__ delete the branch on `origin`. To delete the remote branch, do
```bash
$ git push origin --delete <branch-name>
```

For more details, take a look at the [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)

#### 2. Forking repositories and collaborative git
Git is a nice way for multiple people to work on the same project.  The ["forking workflow"](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow) allows you to "fork" someone else's repository, which creates a separate copy of the repo on your account. Then, you make changes to your own repository. When you're ready with your changes, you send a "pull request" to the owner of the main repository. They can then merge your changes with their repository.

To fork someone's repository, go to its github page and click "Fork". This will create a copy in your account. An extra step you'll end up going through is linking another remote location to your repository. Once you've cloned your repo (it should already have a link to `origin` because you cloned it), do
```bash
$ git remote add upstream main-repo-url
```
This links the main repository under the alias `upstream`.

Then you can follow the instructions elsewhere in this tutorial to push your changes to a development branch on your origin.
These are the commands you'll end up using
```bash
$ git clone <repo-url>
$ git checkout -b new-branch
$ git add
$ git commit -m "Commit message"
$ git push -u origin new-branch
    # or simply git push
```

Once you're done with your changes, initiate a pull request from the webpage of your repository. Remember to select the correct base branch (usually `upstream/master`) and HEAD branch (usually `origin/new-branch`).

If `upstream/master` has changed in the meantime, you can now begin the process of fixing conflicts. To do this, do
```bash
$ git pull upstream master
```

This will fetch `upstream/master` and merge it into your current branch.
If there aren't any conflicts, it'll merge automatically. If there are conflicts, use the procedure outlined above to fix them. Once you've fixed conflicts, do
```bash
$ git merge --continue
```

After all conflicts are fixed, push the latest commit to your origin.
```bash
$ git push origin new-branch
```
New commits are added to your open pull request automatically.

#### 3. .gitignore

Over the course of your work, you _will_ create files you don't want to<sup>or shouldn't</sup> track with git.
Doing `git add *` adds _everything_ to the Index, and individually adding files every time is not feasible. Solution: [use .gitignore files.](https://help.github.com/articles/ignoring-files/)
Create a file called `.gitignore` in the tracked git directory, and fill it with filenames and patterns you want git to ignore.

__Pro tip:__ Github lets you select a preset .gitignore file (based on the language you're using) to your repository when you're creating it. You can also find a bunch of common gitignore files [here](https://github.com/github/gitignore)

#### Other useful git commands
(a.k.a. stuff I will ~~definitely~~ maybe add to this tutorial in the future, but you should look it up in your free time.)

1. `git diff`
1. `git tag`
1. `git stash`
1. `git reset`
1. `git log`


## Contributing to this document
Found something wrong here? Something missing? Solved a common problem and want to save other people some time and headache?

![Uncle Sam wants you to contribute!](readme-img/unclesam.jpg)

You can improve this tutorial by forking this repository, making your changes to the `README.md` file, and sending us a pull request.
This file is formatted using Markdown. [Here's a cheat-sheet on Markdown syntax.](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)

Good luck, and have fun!
