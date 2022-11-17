---
title: "Intermediate git concepts"
teaching: 0 # TBD
exercises: 0 # TBD
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
- "Rewriting history can have unexpected consequences and you risk loosing information irremedaibly"
- "Reset: You have made a mistake and want to keep the commit history tidy for the benefit of collaborators"
- "Revert: You want to undo something done in the past without messing too much with the timeline, upsetting your collaborators"
- "Stash: You want to do somnething else - eg. checkout someone else's branch - without loosing your current work"
- "Rebase: Someone else has updated the main branch while you've been working and need to bring those changes to your branch"
---


## Rewriting history with Git

While version control is useful to keep track of changes made to a piece of work over
time, it also let you to modify the timeline of commits. There are several totally
ligitimate reasons why you might want to do that, from keeping the commit history clean
of unseccesful attempts to do something to incorporate work done by someone else.

This episode explores some of the commands `git` offers to manipulate the commit history
for your benefit and that of your collaborators.

### Commit --amend

This is the simplest method of rewriting history: it lets you (guess what?) amend the
last commit you made so you can incorporate more changes, fix an error you have spotted
and that is worth incorporating as part of that commit and not as a separate one or just
improve your commit message.

TODO: Include example

Do not amend commits that other people might already have as the ament commit is an
entirely new commit altogether that will be in your history but not in theirs partly
doing the same thing - and therefore resulting in conflicts!

### Reset

In it simplest form, `git reset` can be similar to the `--amend` option above: it let
you change the last commit to add or modify things done there. This is achieved with

```bash
$ git reset --soft HEAD^
```

This reset the staging area to match the most recent commit, but leaves the working
directory unchanged - so no information is lost. Now you can review the files you
modified, make more changes or whatever you like. When you are ready, you stag and
commit your files, as usual.

A way more dangerous option uses the flag `--hard`. When doing this, you completely
remove the commits up to the specified one, updating the files in the working directory
accordingly. In other words, **any work done since the chosen commit will be completely
erased**.

To undo just the last commit, you can do:
~~~
$ git reset --hard HEAD^
~~~
{: .commands}

Otherwise, to go back in time to a specific commit, you woudl do:
~~~
$ git reset --hard COMMIT_HASH
~~~
{: .commands}

Let's put this into practice! After all the work done in the previous episode adjusting
the amount of salt, you conclude that it was nonesense and you should keep the original
amount. You could obviusly just create a new commit with the correct amount of salt, but
that will leave your poor attempts to improve the recipe in the commit history, so you
decide to totally erase them.

First, we check how far back we need to go with `git graph`:
~~~
*   c9d9bfe (HEAD -> master) Merged experiment into master
|\
| * 84a371d (experiment) Added salt to balance coriander
* | 54467fa Reduce salt
* | fe0d257 Merge branch 'experiment'
|\|
| * 99b2352 Reduced the amount of coriander
* | 2c2d0e2 Merge branch 'experiment'
|\|
| * d9043d2 Try with some coriander
* | 6a2a76f Corrected typo in ingredients.md
|/
* 57d4505 Revert "Added instruction to enjoy"
* 5cb4883 Added 1/2 onion
* 43536f3 Added instruction to enjoy
* 745fb8b Adding ingredients and instructions
~~~
{: .output}

We can see in the example that we want to discard the last three commits from history
and go back to `fe0d257`, when we merged the `experiment` branch after reducing the
amount of coriander. Let's do it (use your own commit hash!):
~~~
$ git reset --hard fe0d257
$ git graph
~~~
{: .commands}

Now, the commit history should look as:
~~~
* 84a371d (experiment) Added salt to balance coriander
| *   fe0d257 (HEAD -> master) Merge branch 'experiment'
| |\
| |/
|/|
* | 99b2352 Reduced the amount of coriander
| *   2c2d0e2 Merge branch 'experiment'
| |\
| |/
|/|
* | d9043d2 Try with some coriander
| * 6a2a76f Corrected typo in ingredients.md
|/
* 57d4505 Revert "Added instruction to enjoy"
* 5cb4883 Added 1/2 onion
* 43536f3 Added instruction to enjoy
* 745fb8b Adding ingredients and instructions
~~~
{: .output}
Note that while the `experiment` branch still mentions the adjustment of salt, that is
no longer part of the `main` commit history. Your working directory has become identical
to before starting the salty that adventure.

> ## Changing History Can Have Unexpected Consequences
>
> Like witht he `--amend` flag, using `git reset` to remove a commit is a bad idea if
> you have **shared** it yet with other people. If you make a commit and share it on
> GitHub or with a colleague by other means then removing that commit from your Git
> history will cause inconsistencies that may be difficult to resolve later. We
> only recommend this approach for commits that are only in your local working
> copy of a repository.
{: .callout}


### Removing branches once you are done with them is good practice
Over time, you will accumulate lots of branches to implement different features in you
code. It is good practice to remove them once they have fulfil their purpose. You can do
that using the `-D` flag with the `git branch` command:

~~~
$ git branch -D BRANCH_NAME
~~~
{: .commands}

As we are done with the `experiment` branch, let's delete it to have a cleaner history.

~~~
$ git branch -D experiment
$ git graph
~~~
{: .commands}

Now, the commit history should look as:
~~~
*   fe0d257 (HEAD -> master) Merge branch 'experiment'
|\
| * 99b2352 Reduced the amount of coriander
* | 2c2d0e2 Merge branch 'experiment'
|\|
| * d9043d2 Try with some coriander
* | 6a2a76f Corrected typo in ingredients.md
|/
* 57d4505 Revert "Added instruction to enjoy"
* 5cb4883 Added 1/2 onion
* 43536f3 Added instruction to enjoy
* 745fb8b Adding ingredients and instructions
~~~
{: .output}

Now there is truly no trace of your attempts to change the content of salt!

### Reversing a commit

As pointed out, using `reset` can be dangerous and it is not suitable if you need to be
more surgical in what you want to change, affecting just what was done on a commit a
while ago, potentially already shared with collaborators. To address that, we have
`revert`.

`git revert` creates a commit that exactly cancels the changes made by a previous one.
It is a new commit, so it is part of the history, but its purpose is to undo something
done in the past.

The syntax in this case is:
~~~
$ git revert --no-edit COMMIT_HASH
~~~
{: .commands}

Let's try this and remove the onion from the recipe. After all, you don't like onion
that much (use your own hash!).
~~~
$ git revert --no-edit 5cb4883
~~~
{: .commands}
The process, unfortunately, will fail anc create a conflict. The reason is that both,
adding the onion and the coriander affect the last line of the code, so git it is unable
to decide on its own how to remove the onion given that something has been added in the
same part of the recipe afterwards.

The ingredients file now will look like this:
~~~
* 2 avocados
* 1 lime
* 2 tsp salt
<<<<<<< HEAD
* 1/2 onion
* 1 tbsp coriander
=======
>>>>>>> parent of 5cb4883 (Added 1/2 onion)
~~~
{: .output}

To move forward, fix the conflics as it was done in the previous section - rmeoving the
<< and >> lines as well as "1/2 onion" and run:
~~~
$ git add ingredients.md
$ git revert --continue --no-edit
$ git graph
~~~
{: .commands}
~~~
* 53371e5 (HEAD -> master) Revert "Added 1/2 onion"
*   fe0d257 Merge branch 'experiment'
|\
| * 99b2352 Reduced the amount of coriander
* | 2c2d0e2 Merge branch 'experiment'
|\|
| * d9043d2 Try with some coriander
* | 6a2a76f Corrected typo in ingredients.md
|/
* 57d4505 Revert "Added instruction to enjoy"
* 5cb4883 Added 1/2 onion
* 43536f3 Added instruction to enjoy
* 745fb8b Adding ingredients and instructions
~~~
{: .output}
Using `git revert` has added a new commit which reverses *exactly* the changes made in
the specified commit (after solving the conflict).

This is yet another good example of why making separate commits for each change is a
good idea, so they can, potentially, be reversed if needed in the future with no fuss.

### Set aside your work safely with `stash`

TODO: everything

### Incorporate past commits with `rebase`

TODO: everything



{% include links.md %}
