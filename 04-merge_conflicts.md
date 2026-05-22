---
title: Merge conflicts
teaching: 10
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Know what a merge conflict is
- Know how to abort a merge in the case of a conflict
- Know how to resolve a merge conflict by manually editing the conflicting file or files

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What is a merge conflict?
- How do I resolve a merge conflict?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Merge conflicts

Whilst Git is good at automatic merges it is inevitable that situations arise
where incompatible sets of changes need to be combined. In this case it is up to
you to decide what should be kept and what should be discarded.

Let's try this by artificially creating a conflict:

```bash
git switch main
# change line to 1 tsp salt in ingredients.md
git stage ingredients.md
git commit -m "Reduce salt"
git switch experiment
# change line to 3 tsp salt in ingredients.md
git stage ingredients.md
git commit -m "Added salt to balance coriander"
git graph
```

```output
* d5fb141 (HEAD -> experiment) Added salt to balance coriander
| * 7477632 (main) Reduce salt
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

![Repository with a merge conflict](fig/branch8.png){alt='Repository with a merge conflict'}

Now we try and merge `experiment` into `main`:

```bash
git switch main
git merge --no-edit experiment
```

```output
Auto-merging ingredients.md
CONFLICT (content): Merge conflict in ingredients.md
Automatic merge failed; fix conflicts and then commit the result.
```

As suspected we are warned that the merge failed. This puts Git into a special
state in which the merge is in progress but has not been finalised by creating a
new commit in main. Fortunately `git status` is quite useful here:

```bash
git status
```

```output
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

```output
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

```output
* 2 avocados
* 1 lime
* 1 tsp salt
* 1/2 onion
* 1 tbsp coriander
```

Now stage, commit and check the result:

```bash
git stage ingredients.md
git commit --no-edit
git graph
```

```output
*   e361d2b (HEAD -> main) Merge branch 'experiment'
|\
| * d5fb141 (experiment) Added salt to balance coriander
* | 7477632 Reduce salt
* |   567307e Merge branch 'experiment'
|\|
| * 9a4b298 Reduced the amount of coriander
* |   40070a5 Merge branch 'experiment'
|\|
| * 96fe069 try with some coriander
* | d4ca89f Corrected typo in ingredients.md
|/
* ddef60e (origin/main) Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
```

![Repository with third merge](fig/branch9.png){alt='Repository with third merge'}

:::::::::::::::::::::::::::::::::::::::  challenge

## Now you try

You should now be on the `main` branch. Try creating another merge conflict of your
own and resolving it.

You will need to follow these steps:

1. Create a new branch and check it out (e.g. called `experiment2`)
2. Commit a change to this branch
3. Check out the `main` branch again
4. Make and commit a change that is incompatible with the previous one (e.g. modify
  the same line in a different way)
5. Attempt to merge your topic branch (e.g. `experiment2`)
6. Resolve any merge conflicts and finalise the merge with `git commit`
7. Confirm for yourself that the merge has completed with `git graph`

NB: If the two sets of changes you made *aren't* incompatible (e.g. you changed
separate parts of the file) you will not get a merge conflict!

:::::::::::::::  solution

## Solution

```bash
git switch -c experiment2
# make changes to ingredients.md (say 1/2 lime)
git stage ingredients.md
git commit -m "Reduced lime"
git switch main
# make changes to the same line in ingredients.md (say 1/4 lime)
git stage ingredients.md
git commit -m "Reduce lime to balance coriander"
git merge --no-edit experiment2
```

This should give rise to a merge conflict:

```output
Auto-merging ingredients.md
CONFLICT (content): Merge conflict in ingredients.md
Automatic merge failed; fix conflicts and then commit the result.
```

Resolve the conflicts. Then stage the file again, and view the graph:

```bash
git stage ingredients.md
git commit
git graph
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Summary

Let us pause for a moment and summarise what we have learned:

- You might come across situations where incompatible sets of changes
  need to be combined.
- If you don't want to perform a merge (or try it again later) then
  you can use `git merge --abort`.
- To resolve a merge conflict, edit the conflicted file into the state
  that you would like to keep, then stage and commit it.
- In the conflicted file, the tags `<<<<<<< HEAD`, `=======`
  and `>>>>>>> branch-name` indicate which branch each version came from.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Merge conflicts result when Git fails to merge files automatically because of mutually incompatible changes
- Merge conflicts must be resolved by manually editing files to specify the desired changes
- After resolving a merge conflict you must finalise the merge with `git stage` and `git commit`

::::::::::::::::::::::::::::::::::::::::::::::::::
