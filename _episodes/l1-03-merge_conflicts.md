---
title: "Merge conflicts"
teaching: 10
exercises: 10
questions:
  - What is a merge conflict?
  - How do I resolve a merge conflict?
objectives:
  - Know what a merge conflict is
  - Know how to abort a merge in the case of a conflict
  - Know how to resolve a merge conflict by manually editing the conflicting file or files
keypoints:
  - Merge conflicts result when Git fails to merge files automatically because of mutually incompatible changes
  - Merge conflicts must be resolved by manually editing files to specify the desired changes
  - After resolving a merge conflict you must finalise the merge with `git stage` and `git commit`
---

## Merge conflicts

Whilst Git is good at automatic merges it is inevitable that situations arise
where incompatible sets of changes need to be combined. In this case it is up to
you to decide what should be kept and what should be discarded.

Let's try this by artificially creating a conflict:

```sh
git switch main
# change line to 1 tsp salt in ingredients.md
git stage ingredients.md
git commit -m "Reduce salt"
git switch experiment
# change line to 3 tsp in ingredients.md
git stage ingredients.md
git commit -m "Added salt to balance coriander"
git graph
```
{: .commands}
```
* d5fb141 (HEAD -> experiment) Added salt to balance coriander
| * 7477632 (main) reduce salt
| *   567307e Merge branch 'experiment'
| |\
| |/
|/|
* | 9a4b298 Reduced the amount of coriander
| *   40070a5 Merge branch 'experiment'
| |\
| |/
|/|
* | 96fe069 try with some coriander
| * d4ca89f Corrected typo in ingredients.md
|/
* ddef60e (origin/main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

![Git collaborative]({{ site.baseurl }}/fig/branch8.png
"Repository with merge conflict"){:class="img-responsive"}

Now we try and merge `experiment` into `main`:

```sh
git switch main
git merge --no-edit experiment
```
{: .commands}
```
Auto-merging ingredients.md
CONFLICT (content): Merge conflict in ingredients.md
Automatic merge failed; fix conflicts and then commit the result.
```
{: .output}

As suspected we are warned that the merge failed. This puts Git into a special
state in which the merge is in progress but has not been finalised by creating a
new commit in main. Fortunately `git status` is quite useful here:

```sh
git status
```
{: .commands}
```
On branch main
Your branch is ahead of 'origin/main' by 6 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
 both modified:   ingredients.md

no changes added to commit (use "git add" and/or "git commit -a")
```

This suggests how we can get out of this state. If we want to give up on this
merge and try it again later then we can use `git merge --abort`. This will
return the repository to its pre-merge state. We will likely have to deal with
the conflict at some point though so may as well do it now. Fortunately we don't
need any new commands. We just need to edit the conflicted file into the state
we would like to keep, then add and commit as usual.

Let's look at ingredients.md to understand the conflict:

```
* 2 avocados
* 1 lime
<<<<<< HEAD
* 1 tsp salt
=======
* 3 tsp salt
>>>>>> experiment
* 1/2 onion
* 1 tbsp coriander
```

Git has changed this file for us and added some lines which highlight the
location of the conflict. This may be confusing at first glance (a good editor
may add some highlighting which can help), but you are essentially being asked
to choose between the two versions presented. The tags `<<<<<<< HEAD`, `=======`
and `>>>>>>> experiment` are used to indicate which branch each version came
from (HEAD here corresponds to `main` as that is our checked out branch).

The conflict makes sense, we can either have 1 tsp of salt or 3. There is no way
for Git to know which it should be so it has to ask you. Let's resolve it by
choosing the version from the main branch. Edit `ingredients.md` so it looks
like:
```
* 2 avocados
* 1 lime
* 1 tsp salt
* 1/2 onion
* 1 tbsp coriander
```

Now stage, commit and check the result:

```sh
git stage ingredients.md
git commit -m "Merged experiment into main"
git graph
```
{: .commands}
```
*   e361d2b (HEAD -> main) Merged experiment into main
|\
| * d5fb141 (experiment) Added salt to balance coriander
* | 7477632 reduce salt
* |   567307e Merge branch 'experiment'
|\ \
| |/
| * 9a4b298 Reduced the amount of coriander
* |   40070a5 Merge branch 'experiment'
|\ \
| |/
| * 96fe069 try with some coriander
* | d4ca89f Corrected typo in ingredients.md
|/
* ddef60e (origin/main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

![Git collaborative]({{ site.baseurl }}/fig/branch9.png
"Repository with third merge"){:class="img-responsive"}

> ## Now you try
>
> You should now be on the `main` branch. Try creating another merge conflict of your
> own and resolving it.
>
> You will need to follow these steps:
>
> 1. Create a new branch and check it out (e.g. called `experiment2`)
> 1. Commit a change to this branch
> 1. Check out the `main` branch again
> 1. Make and commit a change that is incompatible with the previous one (e.g. modify
>    the same line in a different way)
> 1. Attempt to merge your topic branch (e.g. `experiment2`)
> 1. Resolve any merge conflicts and finalise the merge with `git commit`
> 1. Confirm for yourself that the merge has completed with `git graph`
>
> NB: If the two sets of changes you made *aren't* incompatible (e.g. you changed
> separate parts of the file) you will not get a merge conflict!
{: .challenge}
