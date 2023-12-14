---
title: "Rewriting history with Git"
teaching: 30
exercises: 20
questions:
- "How can multiple collaborators work efficiently on the same code?"
- "When should I use rebasing, merging and stashing?"
- "How can I reset or revert changes without upsetting my collaborators?"
objectives:
- "Understand the options for rewriting git history"
- "Know how to use them effectively when working with collaborators"
- "Understand the risks associated with rewriting history"
keypoints:
- "There are several ways of rewriting git history, each with specific use cases associated to them"
- "Rewriting history can have unexpected consequences and you risk losing information permanently"
- "Reset: You have made a mistake and want to keep the commit history tidy for the benefit of collaborators"
- "Revert: You want to undo something done in the past without messing too much with the timeline, upsetting your collaborators"
- "Stash: You want to do something else -- e.g. checkout someone else's branch -- without losing your current work"
- "Rebase: Someone else has updated the main branch while you've been working and need to bring those changes to your branch"
---


## Rewriting history with Git

While version control is useful to keep track of changes made to a piece of work over
time, it also lets you to modify the timeline of commits. There are several totally
legitimate reasons why you might want to do that, from keeping the commit history clean
of unsuccessful attempts to do something to incorporate work done by someone else.

This episode explores some of the commands `git` offers to manipulate the commit history
for your benefit and that of your collaborators.

### Amend

This is the simplest method of rewriting history: it lets you amend the last commit you
made, maybe adding some files you forgot to stage or fixing a typo in the commit message.

After you have made those last minute changes - and `staged` them, if needed - all you
need to do to amend the last commit while keeping the same commit message is:

```bash
git commit --amend --no-edit
```

Or this:

```bash
git commit --amend -m "New commit message"
```

if you want to write a new commit message:

Note that this will replace the previous commit with a new one - the commit hash will be
different, so this approach must not be used if the commit was already pushed to the
remote repository and shared with collaborators.

### Reset

The next level of complexity rewriting history is `reset`: it lets you redo the
last (or last few) commit(s) you made so you can incorporate more changes, fix an error
you have spotted and that is worth incorporating as part of that commit and not as a
separate one or just improve your commit message.

```bash
git reset --soft HEAD^
```

This resets the staging area to match the most recent commit, but leaves the working
directory unchanged - so no information is lost. Now you can review the files you
modified, make more changes or whatever you like. When you are ready, you stage and
commit your files, as usual. You can go back 2 commits, 3, etc with `HEAD^2`,
`HEAD^3`... but the further you go, the more chances there are to leave commits
without a parent commit. Resulting in a messy (but potentially recoverable) repository,
as information is not lost. You can read about this recovery process in this [blog post in
Medium](https://www.ocpsoft.org/tutorials/git/use-reflog-and-cherry-pick-to-restore-lost-commits/).

A way more dangerous option uses the flag `--hard`. When doing this, you completely
remove the commits up to the specified one, updating the files in the working directory
accordingly. In other words, **any work done since the chosen commit will be completely
erased**.

To undo just the last commit, you can do:
```
git reset --hard HEAD^
```
{: .commands}

Otherwise, to go back in time to a specific commit, you would do:
```
git reset --hard COMMIT_HASH
```
{: .commands}

> ## Don't mess with the salt
>
> Let's put this into practice! After all the work done in the previous episode adjusting
> the amount of salt, you conclude that it was nonsense and you should keep the original
> amount. You could obviously just create a new commit with the correct amount of salt, but
> that will leave your poor attempts to improve the recipe in the commit history, so you
> decide to totally erase them.
> >
> > ## Solution
> >
> > First, we check how far back we need to go with `git graph`:
> > ```
> > *   c9d9bfe (HEAD -> main) Merged experiment into main
> > |\
> > | * 84a371d (experiment) Added salt to balance coriander
> > * | 54467fa Reduce salt
> > * | fe0d257 Merge branch 'experiment'
> > |\|
> > | * 99b2352 Reduced the amount of coriander
> > * | 2c2d0e2 Merge branch 'experiment'
> > |\|
> > | * d9043d2 Try with some coriander
> > * | 6a2a76f Corrected typo in ingredients.md
> > |/
> > * 57d4505 (origin/main) Revert "Added instruction to enjoy"
> > * 5cb4883 Added 1/2 onion
> > * 43536f3 Added instruction to enjoy
> > * 745fb8b Adding ingredients and instructions
> > ```
> > {: .output}
> >
> > We can see in the example that we want to discard the last three commits from history
> > and go back to `fe0d257`, when we merged the `experiment` branch after reducing the
> > amount of coriander. Let's do it (use your own commit hash!):
> > ```
> > $ git reset --hard fe0d257
> > $ git graph
> > ```
> > {: .commands}
> >
> > Now, the commit history should look as:
> > ```
> > * 84a371d (experiment) Added salt to balance coriander
> > | *   fe0d257 (HEAD -> main) Merge branch 'experiment'
> > | |\
> > | |/
> > |/|
> > * | 99b2352 Reduced the amount of coriander
> > | *   2c2d0e2 Merge branch 'experiment'
> > | |\
> > | |/
> > |/|
> > * | d9043d2 Try with some coriander
> > | * 6a2a76f Corrected typo in ingredients.md
> > |/
> > * 57d4505 (origin/main) Revert "Added instruction to enjoy"
> > * 5cb4883 Added 1/2 onion
> > * 43536f3 Added instruction to enjoy
> > * 745fb8b Adding ingredients and instructions
> > ```
> > {: .output}
> > Note that while the `experiment` branch still mentions the adjustment of salt, that is
> > no longer part of the `main` commit history. Your working directory has become identical
> > to that before starting the salty adventure.
> >
> {: .solution}
>
{: .challenge}

> ## Changing History Can Have Unexpected Consequences
>
> Like with `git commit --amend`, using `git reset` to remove a commit is a bad idea if
> you have **already shared** it with other people. If you make a commit and share it on
> GitHub or with a colleague by other means then removing that commit from your Git
> history will cause inconsistencies that may be difficult to resolve later. We
> only recommend this approach for commits that are only in your local working
> copy of a repository.
{: .callout}

### Removing branches once you are done with them is good practice

Over time, you will accumulate lots of branches to implement different features in you
code. It is good practice to remove them once they have fulfil their purpose. You can do
that using the `-D` flag with the `git branch` command:

```
git branch -D BRANCH_NAME
```
{: .commands}

> ## Getting rid of the experiment
>
> As we are done with the `experiment` branch, let's delete it to have a cleaner history.
> >
> > ## Solution
> >
> > ```
> > $ git branch -D experiment
> > $ git graph
> > ```
> > {: .commands}
> >
> > Now, the commit history should look as:
> > ```
> > *   fe0d257 (HEAD -> main) Merge branch 'experiment'
> > |\
> > | * 99b2352 Reduced the amount of coriander
> > * | 2c2d0e2 Merge branch 'experiment'
> > |\|
> > | * d9043d2 Try with some coriander
> > * | 6a2a76f Corrected typo in ingredients.md
> > |/
> > * 57d4505 (origin/main) Revert "Added instruction to enjoy"
> > * 5cb4883 Added 1/2 onion
> > * 43536f3 Added instruction to enjoy
> > * 745fb8b Adding ingredients and instructions
> > ```
> > {: .output}
> >
> > Now there is truly no trace of your attempts to change the content of salt!
> >
> {: .solution}
>
{: .challenge}

### Reverting a commit

As pointed out, using `reset` can be dangerous and it is not suitable if you need to be
more surgical in what you want to change, affecting just what was done on a commit a
while ago, potentially already shared with collaborators. To address that, we have
`revert`.

`git revert` creates a commit that exactly cancels the changes made by a previous one.
It is a new commit, so it is part of the history, but its purpose is to undo something
done in the past.

The syntax in this case is:
```
git revert --no-edit COMMIT_HASH
```
{: .commands}

You can omit the `--no-edit` flag and use `-m` to give a one-line description for the
process or also omit `-m` to enter into the default text editor to leave a more complete
description and rationale for the revert.

> ## Remove the onion
>
> Let's try this and remove the onion from the recipe. After all, you don't like onion
> that much!
> >
> > ## Solution
> >
> > ```
> > $ git revert --no-edit 5cb4883
> > ```
> > {: .commands}
> > The process, unfortunately, will fail and create a conflict. The reason is that both,
> > adding the onion and the coriander affect the last line of the code, so git is unable
> > to decide on its own how to remove the onion given that something has been added in the
> > same part of the recipe afterwards.
> >
> > The ingredients file now will look like this:
> > ```
> > * 2 avocados
> > * 1 lime
> > * 2 tsp salt
> > <<<<<<< HEAD
> > * 1/2 onion
> > * 1 tbsp coriander
> > =======
> > >>>>>>> parent of 5cb4883 (Added 1/2 onion)
> > ```
> > {: .output}
> >
> > To move forward, fix the conflicts as it was done in the previous section - removing the
> > << and >> lines as well as "1/2 onion" and run:
> > ```
> > $ git add ingredients.md
> > $ git revert --continue --no-edit
> > $ git graph
> > ```
> > {: .commands}
> > ```
> > * 53371e5 (HEAD -> main) Revert "Added 1/2 onion"
> > *   fe0d257 Merge branch 'experiment'
> > |\
> > | * 99b2352 Reduced the amount of coriander
> > * | 2c2d0e2 Merge branch 'experiment'
> > |\|
> > | * d9043d2 Try with some coriander
> > * | 6a2a76f Corrected typo in ingredients.md
> > |/
> > * 57d4505 (origin/main) Revert "Added instruction to enjoy"
> > * 5cb4883 Added 1/2 onion
> > * 43536f3 Added instruction to enjoy
> > * 745fb8b Adding ingredients and instructions
> > ```
> > {: .output}
> > Using `git revert` has added a new commit which reverses *exactly* the changes made in
> > the specified commit (after solving the conflict).
> >
> {: .solution}
>
{: .challenge}

This is yet another good example of why making separate commits for each change is a
good idea, so they can, potentially, be reversed if needed in the future with no fuss.

> ## `reset` vs `revert`
>
> Both commands let you undo things done in the past, but they both have very different
> use cases.
>
> - `reset` uses brute force, potentially with destructive consequences, to
> make those changes and is suitable only if the work has not been shared with others
> already. Use when you want to get rid of recent work you're not happy with and start
> all over again.
> - `revert` is more lightweight and surgical, to target specific changes and
> creating new commits to history. Use when code has already been shared with others or
> when changes are small and clearly isolated.
{: .callout}

### Set aside your work safely with `stash`

It is not rare that, while you are working on some feature, you need to check something
else in another branch. Very often this is the case when you want to try some
contributor's code as part of a pull request review process (see next episodes). You
can commit the work you are doing, but if it is not in a state ready to be committed,
what would you do?

`git stash` is the answer. It lets you put your current, uncommitted work aside in a
special state, turning the working directory back to the way it was in the last commit.
Then, you can easily switch branches, pull new ones or do whatever you want. Once you
are ready to go back to work, you can recover the stashed work and continue as if
nothing had happened.

The following are the `git stash` commands needed to make this happen:

Stash the current state of the repository, giving some message to remind yourself what
was this about. The working directory becomes identical to the last commit.
```
git stash save "Some informative message"
```
{: .commands}

List the stashes available in reverse chronological order (last one stashed goes on
top).
```
git stash list
```
{: .commands}

Extract the **last stash** of the list, updating the working directory
with its content.
```
git stash pop
```
{: .commands}

Extract the stash with the given number from the list, updating the working directory
with its content.
```
git stash pop stash@{NUMBER}
```
{: .commands}

Apply the **last stash** without removing it from the list, so you can apply it to
other branches, if needed.
```
git stash apply
```
{: .commands}

Apply the given stash without removing it from the list, so you can apply it to
other branches, if needed.
```
git stash apply stash@{NUMBER}
```
{: .commands}

If you want more information, you can [read this article on Git
stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash).

> ## Practice stashing
>
> Now try using `git stash` with the recipe repository. For example:
>
> - Add some ingredients then stash the changes (do not stage or commit them)
> - Modify the instructions and also stash those change
>
> Then have a look at the list of stashes and bring those changes back to the
> working directory using `stash pop` and `stash apply`, and see how the list of
> stashes changes in either case.
>
{: .challenge}

### Incorporate past commits with `rebase`

Rebasing is the process of moving or combining a sequence of commits to a new base
commit. In other words, you take a collection of commits that you have created that
branched off a particular commit and make them appear as if they branched off a
different one.

The most common use case for `git rebase` happens when you are working on your feature
branch (let's say `experiment`) and, in the meantime there have been commits done to the
base branch (for example, `main`). You might want to use in your own work some upstream
changes done by someone else or simply keep the history of the repository linear,
facilitating merging back in the future.

The command is straightforward:
```
git rebase NEW_BASE
```
{: .commands}
where `NEW_BASE` can be either a commit hash or a branch name we want to use as the new
base.

The following figure illustrates the process where, after rebasing, the two commits of
the feature branch have been recreated after the last commit of the main branch.

![Rebase process with a feature branch being moved to another branch]({{ site.baseurl }}/fig/rebase.png
"Rebase process with a feature branch being moved to another branch")

For a very thorough description about how this process works, read this [article on Git
rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase).

> ## Practice rebasing
>
> We are going to practice rebasing in a simple scenario with the recipe repository.
> We need to do some preparatory work first:
>
> - Create a `spicy` branch
> - Add some chillies to the list of ingredients and commit the changes
> - Switch back to the `main` branch
> - Add a final step in the instructions indicating that this should be served cold
> - Go back to the `spicy` branch
>
> If you were to add now instructions to chop the chillies finely and put some on top
> of the mix, chances are that you will have conflicts later on when merging back to
> main. We can merge `main` into `spicy`, as we did in the previous episode, but that
> will result in a non-linear history (not a big deal in this case, but things can get
> really complicated).
>
> So let's use `git rebase` to bring the `spicy` branch as it it would have been
> branched off `main` after indicating that the guacamole needs to be served cold.
>
> > ## Solution
> >
> > After the following commands (and modifications to the files) the repository history
> > should look like the graph below:
> > ```
> > $ git checkout -b spicy
> > $ # add the chillies to ingredients.md
> > $ git add ingredients.md
> > $ git commit -m "Chillies added to the mix"
> > $ git checkout main
> > $ # Indicate that should be served cold in instructions.md
> > $ git add instructions.md
> > $ git commit -m "Guacamole must be served cold"
> > $ git graph
> > ```
> > {: .commands}
> > ```
> > * d10e1e9 (HEAD -> main) Guacamole must be served cold
> > | * e0350e4 (spicy) Chillies added to the mix
> > |/
> > * 5344d8f Revert "Added 1/2 onion"
> > *   fe0d257 Merge branch 'experiment'
> > |\
> > | * 99b2352 Reduced the amount of coriander
> > * | 2c2d0e2 Merge branch 'experiment'
> > |\|
> > | * d9043d2 Try with some coriander
> > * | 6a2a76f Corrected typo in ingredients.md
> > |/
> > * 57d4505 (origin/main) Revert "Added instruction to enjoy"
> > * 5cb4883 Added 1/2 onion
> > * 43536f3 Added instruction to enjoy
> > * 745fb8b Adding ingredients and instructions
> > ```
> > {: .output}
> > Now, let's go back to `spicy` and do the `git rebase`:
> > ```
> > $ git checkout spicy
> > $ git rebase main
> > $ git graph
> > ```
> > {: .commands}
> > ```
> > * a34042b (HEAD -> spicy) Chillies added to the mix
> > * d10e1e9 (main) Guacamole must be served cold
> > * 5344d8f Revert "Added 1/2 onion"
> > *   fe0d257 Merge branch 'experiment'
> > |\
> > | * 99b2352 Reduced the amount of coriander
> > * | 2c2d0e2 Merge branch 'experiment'
> > |\|
> > | * d9043d2 Try with some coriander
> > * | 6a2a76f Corrected typo in ingredients.md
> > |/
> > * 57d4505 (origin/main) Revert "Added instruction to enjoy"
> > * 5cb4883 Added 1/2 onion
> > * 43536f3 Added instruction to enjoy
> > * 745fb8b Adding ingredients and instructions
> > ```
> > {: .output}
> >
> > Can you spot the difference with the coriander experiment? Now the commit history is
> > linear and we have avoided the risk of conflicts.
> >
> {: .solution}
>
{: .challenge}

{% include links.md %}
