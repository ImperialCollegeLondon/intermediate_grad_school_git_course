---
title: "Multiple branches in remotes"
teaching: 10
exercises: 5
questions:
- "How do I keep my local branches in sync with the remote branches?"
- "What is the difference between forking and branching?"
objectives:
- Push changes to a remote repository.
- Pull changes from a remote repository.
keypoints:
- With `git push` you can push any committed changes in your current branch to its upstream branch.
- If the current branch has no upstream yet, you can configure one by doing `git push -u origin BRANCH_NAME`.
- Using `git pull` will bring changes in the upstream branch to the local branch.
---

## Multiple branches in remotes

The same way you might have different branches in your local repository, you could
manage different branches in your remote - the same branches or different ones.

As a reminder, remote and local repositories are not automatically synchronised, but
rather it is a manual process done via `git pull` and `git push` commands. This
synchronisation needs to be done **branch by branch** with all of those you want to keep
in sync.

### Pushing

* Its basic use is to synchronise **any committed changes** in your current
 branch to its upstream branch: `git push`.
* Changes in the staging area will not be synchronised.
![Git collaborative]({{ site.baseurl }}/fig/push.png "Push a branch
."){:class="img-responsive"}
* If the current branch has no upstream yet, you can configure one by doing
`git push -u origin BRANCH_NAME`, as done with `main` in the exercise
 above.
![Git collaborative]({{ site.baseurl }}/fig/push_u.png "Push a branch without
 upstream yet."){:class="img-responsive"}
* `push` only operates on your current branch. If you want to push another
 branch, you have to `checkout` that branch first.
* If the upstream branch has changes you do not have in the local branch, the
 command will fail, requesting you to pull those changes first.

## Pulling

* Opposite to `push`, `pull` brings changes in the upstream branch to the local
 branch.
* You can check if there are any changes to synchronise in the upstream
 branch by running `git fetch`, which only checks if there are changes, and then
`git status` to see how your local and remote branch compare in terms of commit history.
* It's best to make sure your repository is in a clean state with no staged or
  unstaged changes.
* If the local and upstream branches have diverged - have different
 commit history - the command will attempt to merge both. If there are conflicts, you
 will need deal with them in the same way described above.
* You can get a new branch existing only in `origin` directly with `git checkout
  BRANCH_NAME` without the need of creating the branch locally and then pulling the
  remote.

![Git collaborative]({{ site.baseurl }}/fig/pull.png "Pull remote changes")
{:class="img-responsive"}

{% include links.md %}
