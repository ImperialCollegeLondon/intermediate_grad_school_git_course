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

TODO: Include example

A way more dangerous option uses the flag `--hard`. When doing this, you completly
remove the commits up to the specified one, updating the files in the working directory
accordingly. In other words, **any work done since the chosen commit will be compltely
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
* ddef60e Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
~~~
{: .output}

We can see in the example that we want to discard the last three commits from history and go back to
`567307e`, when we merged the `experiment` branch after reducing the amount of
coriander. Let's do it (use your own commit hash!):
~~~
$ git reset --hard 567307e
$ git graph
~~~
{: .commands}

Now, the commit history should look as:
~~~
* 567307e (HEAD -> main) Merge branch 'experiment'
|\ 
| * 9a4b298 (experiment) Reduced the amount of coriander
* |   40070a5 Merge branch 'experiment'
|\ \  
| |/  
| * 96fe069 try with some coriander
* | d4ca89f Corrected typo in ingredients.md
|/  
* ddef60e Revert "Added instruction to enjoy"
* 8bfd0ff Added 1/2 onion to ingredients
* 2bf7ece Added instruction to enjoy
* ae3255a Adding ingredients and instructions
~~~
{: .output}

Not only the commit history now shows no trace of the salty adventure, but your working
directory has become identical to before starting that adventure.

> ## Changing History Can Have Unexpected Consequences
>
> Like witht he `--amend` flag, using `git reset` to remove a commit is a bad idea if
> you have **shared** it yet with other people. If you make a commit and share it on
> GitHub or with a colleague by other means then removing that commit from your Git
> history will cause inconsistencies that may be difficult to resolve later. We
> only recommend this approach for commits that are only in your local working
> copy of a repository.
{: .callout}

### Reversing a commit

TODO: everything

### Set aside your work safely with `stash`

TODO: everything

### Incorporate past commits with `rebase`

TODO: everything



{% include links.md %}
