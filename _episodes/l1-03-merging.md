---
title: "Merging"
teaching: 30
exercises: 15
questions:
- How can changes from parallel tracks of work be combined?
objectives:
- Explain what is merging
- How to incorporate a feature from a branch into your code
- Describe a scalable workflow for development with git
keypoints:
- Once a branch is complete the changes made can be integrated into the project using `git merge branch_name`
- Merging creates a new commit in the target branch incorporating all of the changes made in a branch
- Conflicts arise when two branches contain incompatible sets of changes and must be resolved before a merge can complete
- Identify the details of merge conflicts using `git diff` and/or `git status`
- A merge conflict can be resolved by manual editing followed by `git add [conflicted file]`... and `git commit -m "commit_message"`
---

## Merging

Now that we have our two separate tracks of work they need to be combined back
together. We should already have the `main` branch checked out (double check
with `git branch`). The below command can then be used to perform the merge.
```
git merge --no-edit experiment
```
{: .commands}
```
Merge made by the 'ort' strategy.
 ingredients.md | 1 +
 1 file changed, 1 insertion(+)
```
{: .output}

now use:
```
git graph
```
{: .commands}
```
*   40070a5 (HEAD -> main) Merge branch 'experiment'
|\
| * 96fe069 (experiment) try with some coriander
* | d4ca89f Corrected typo in ingredients.md
|/
* ddef60e (origin/main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```
{: .output}

![Git collaborative]({{ site.baseurl }}/fig/branch6.png
"Repository with first merge"){:class="img-responsive"}

Merging creates a new commit in whichever branch is being **merged into** that
contains the combined changes from both branches. The commit has been
highlighted in a separate colour above but it is the same as every commit we've
seen so far except that it has two parent commits. Git is pretty clever at
combining the changes automatically, combining the two edits made to the same
file for instance. Note that the experiment branch is still present in the
repository.

> ## Now you try
>
> As the experiment branch is still present there is no reason further commits
> can't be added to it. Create a new commit in the `experiment` branch adjusting
> the amount of coriander in the recipe. Then merge `experiment` into `main`.
> You should end up with a repository history matching: ![Git
> collaborative]({{ site.baseurl }}/fig/branch7.png "Repository with second
> merge"){:class="img-responsive"}
>
> > ## Solution
> >
> > ```
> > git checkout experiment
> > # make changes to ingredients.md
> > git add ingredients.md
> > git commit -m "Reduced the amount of coriander"
> > git checkout main
> > git merge --no-edit experiment
> > git graph
> > ```
> > {: .commands}
> > ```
> > *   567307e (HEAD -> main) Merge branch 'experiment'
> > |\
> > | * 9a4b298 (experiment) Reduced the amount of coriander
> > * |   40070a5 Merge branch 'experiment'
> > |\ \
> > | |/
> > | * 96fe069 try with some coriander
> > * | d4ca89f Corrected typo in ingredients.md
> > |/
> > * ddef60e (origin/main) Revert "Added instruction to enjoy"
> > * 8bfd0ff Added 1/2 onion to ingredients
> > * 2bf7ece Added instruction to enjoy
> > * ae3255a Adding ingredients and instructions
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

## Conflicts

Whilst Git is good at automatic merges it is inevitable that situations arise
where incompatible sets of changes need to be combined. In this case it is up to
you to decide what should be kept and what should be discarded. First lets set
up a conflict:
```
git checkout main
# change line to 1 tsp salt in ingredients.md
git add ingredients.md
git commit -m "Reduce salt"
git checkout experiment
# change line to 3 tsp in ingredients.md
git add ingredients.md
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
```
git checkout main
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
```
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
merge and try it again later then we can use `git merge --abort.`. This will
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
```
git add ingredients.md
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

## Summary

Let us pause for a moment and recapitulate what we have just learned:

```shell
git merge <name>         # merge branch <name> (to current branch)
```

### Typical workflow

These commands can be used in a typical workflow that looks like the below:

```shell
$ git checkout -b new-feature  # create branch, switch to it
$ git commit                   # work, work, work, ...
                               # test
                               # feature is ready
$ git checkout main            # switch to main
$ git merge new-feature        # merge work to main
$ git branch -d new-feature    # remove branch
```

{% include links.md %}
