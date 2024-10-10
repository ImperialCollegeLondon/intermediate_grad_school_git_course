---
title: "Branching"
teaching: 20
exercises: 0
questions:
- How can I or my team work on multiple features in parallel?
objectives:
- Explain what git branches are and when they should be used
- Use a branch to develop a new feature
- Identify the branches in a project and which branch is currently in use
keypoints:
- Git allows non-linear commit histories called branches
- A branch can be thought of as a label that applies to a set of commits
- Branches can and should be used to carry out development of new features
- Branches in a project can be listed with `git branch` and created with `git branch branch_name`
- The `HEAD` refers to the current position of the project in its commit history
- The current branch can be changed using `git checkout branch_name`
---

## Motivation for branches

> ## Differing Goals and Objectives
>
> Developer 1 - "I need a new type of analysis to finish my thesis"
>
> Developer 2 - "My problem is bigger. I need better performance to process all my
> data"
{: .discussion}

For simple projects, working with a single branch where you keep adding commits is good
enough. But chances are that you will want to unleash all the power of `git` at some
point and start using branches.

In a linear history, we have something like:

![Linear]({{ site.baseurl }}/fig/branch1.png "Linear git
repository"){:class="img-responsive"}

* Commits are depicted here as little boxes with abbreviated hashes.
* Here the branch `main` points to a commit.
* "HEAD" is the current position (remember the recording head of tape
  recorders?).
* When we talk about branches, we often mean all parent commits, not only the
  commit pointed to.

### Now we want to do this

<div style="border:2px solid #000000;"> <img src="{{ site.baseurl
  }}/fig/octopus.jpeg" width="60%" alt="Example of git merge"> <br> Source: <a
  href="https://twitter.com/jay_gee/status/703360688618536960">https://twitter.com/jay_gee/status/703360688618536960</a>
  </div>

Software development is often not linear:

* We typically need at least one version of the code to "work" (to compile, to
  give expected results, ...).
* At the same time we work on new features -- possibly several features concurrently.
  Often they are unfinished.
* We need to be able to easily separate out work on different features.

The strength of version control is that it permits the researcher to **isolate
different tracks of work**, which can later be merged to create a composite
version that contains all changes:

![Git collaborative]({{ site.baseurl }}/fig/branching_full_example.png "Example
of commit history with multiple branches and merges"){:class="img-responsive"}

* We see branching points and merging points.
* Main line development is often called `main`.
* Other than this convention there is nothing special about `main`, it is just
  a branch.
* Commits form a directed acyclic graph (arrows point from parent commits to
  child commits).
* Commits are **relative** to the preceding (parent) commit. Whilst
  Git can be described as taking "snapshots" of your project this is
  slightly misleading. Git actually records *the changes made since the last
  commit*. The difference is subtle but powerful, it makes commands like `git
  revert` possible.

A group of commits that create a single narrative are called a **branch**.
There are different branching strategies, but it is useful to think that a
branch tells the story of a feature, e.g. "fast sequence extraction" or "Python
interface" or "fixing bug in matrix inversion algorithm".

> ## Starting point
>
> Navigate to your `recipe` directory, containing the guacamole recipe
> repository.
>
> If you then type `git log --oneline`, you should see something like:
>
> ```
> 09c9b3b (HEAD -> main, origin/main) Revert "Added instruction to enjoy"
> 366f4b5 Added 1/2 onion to ingredients
> 1171d94 Added instruction to enjoy
> 6ff8aa5 adding ingredients and instructions
> ```
> {: .output}
>
{: .callout}

### Which Branch Are We Using?

To see where we are (where HEAD points to) use `git branch`:
```
git branch
```
{: .commands}
```
* main
```
{: .output}

* This command shows where we are, it does not create a branch.
* There is only `main` and we are on `main` (star represents the `HEAD`).

In the following we will learn how to create branches, how to switch between
them and how to merge changes from different branches.

---

> ## A useful alias
>
> We will now define an *alias* in Git, to be able to nicely visualise branch
> structure in the terminal without having to remember a long Git command
> (more details about what aliases are can be found
> [here](https://linuxize.com/post/how-to-create-bash-aliases/) and the full <!-- markdown-link-check-disable-line -->
> docs on how to set them up in Git are
> [here](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)):
>
> ```
> git config --global alias.graph "log --all --graph --decorate --oneline"
> ```
> {: .commands}
{: .callout}

## Creating and Working with Branches

Firstly let's take stock of the current state of our repository:

```
git graph
```
{: .commands}
```
* ddef60e (HEAD -> main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

We have four commits and you can see that we are working on the main branch
from `HEAD -> main` next to the most recent commit. This can be represented
diagrammatically:

![Git collaborative]({{ site.baseurl }}/fig/branch1.png
"Repository before branching"){:class="img-responsive"}

Let's create a branch called `experiment` where we try out adding some
coriander to `ingredients.md`.

```
git branch experiment
git graph
```
{: .commands}
```
* ddef60e (HEAD -> main, origin/main, experiment) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

Notice that the name of our new branch has appeared next to latest commit. HEAD
is still pointing to main however denoting that we have created a new branch but
we're not using it yet. This looks like:

![Git collaborative]({{ site.baseurl }}/fig/branch2.png
"Repository after experiment branch creation"){:class="img-responsive"}

To start using the new branch we need to check it out:
```
git checkout experiment
git graph
```
{: .commands}
```
* ddef60e (HEAD -> experiment, origin/main, main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

Now we see `HEAD -> experiment` next to the top commit indicating that we are
now working with, and any commits we make will be part of the `experiment`
branch. As shown before which branch is currently checked out can be confirmed
with `git branch`.

![Git collaborative]({{ site.baseurl }}/fig/branch3.png
"Repository with HEAD at new experiment branch"){:class="img-responsive"}

Now when we make new commits they will be part of the `experiment` branch. To
test this let's add 1 tbsp coriander to `ingredients.md`. Stage this and commit
it with the message "try with some coriander".

```
git add ingredients.md
git commit -m "try with some coriander"
git graph
```
{: .commands}
```
* 96fe069 (HEAD -> experiment) try with some coriander
* ddef60e (origin/main, main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

![Git collaborative]({{ site.baseurl }}/fig/branch4.png
"Repository with one commit on experiment branch"){:class="img-responsive"}

Note that the main branch is unchanged whilst a new commit (labelled `e1`) has
been created as part of the experiment branch.

As mentioned previously, one of the advantages of using branches is working on
different features in parallel. You may have already spotted the typo in
`ingredients.md` but let's say that we've only just seen it in the midst of
our work on the `experiment` branch. We could correct the typo with a new commit
in `experiment` but it doesn't fit in very well here - if we decide to discard
our experiment then we also lose the correction. Instead it makes much more
sense to create a correcting commit in `main`. First, move to (checkout) the main branch:

```
git checkout main
```
{: .commands}

Then fix the typing mistake in `ingredients.md`. And finally, commit that change (hint:
'avo' look at the first ingredient):

```
git add ingredients.md
git commit -m "Corrected typo in ingredients.md"
git graph
```
{: .commands}
```
* d4ca89f (HEAD -> main) Corrected typo in ingredients.md
| * 96fe069 (experiment) try with some coriander
|/
* ddef60e (origin/main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}
![Git collaborative]({{ site.baseurl }}/fig/branch5.png
"Repository with one commit on main and experiment branches"){:class="img-responsive"}

## Summary

Let us pause for a moment and summarise what we have just learned:

```shell
git branch               # see where we are
git branch <name>        # create branch <name>
git checkout <name>      # switch to branch <name>
```

Since the following command combo is so frequent:

```shell
git branch <name>        # create branch <name>
git checkout <name>      # switch to branch <name>
```

There is a shortcut for it:

```shell
git checkout -b <name>   # create branch <name> and switch to it
```

{% include links.md %}
