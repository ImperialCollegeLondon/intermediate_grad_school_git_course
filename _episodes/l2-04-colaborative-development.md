---
title: "Collaborative development"
teaching: 5
exercises: 25
questions:
- "How do I put into practice all the previous knowledge at once?"
- "What caveats might I find in a real collaborative scenario?"
objectives:
- Create a pull request from a branch within a repository.
- Create a pull request from a forked repository.
- Manage other people's contributions.
- Create releases at key points of the development.
keypoints:
- Working collaboratively requires coordination - use Issues to discuss with your colleagues who is doing what.
- Notifications from GitHub are very useful but also overwhelming when there are many contributions - you will need to manage them.
---

## Collaborating in real life

This final episode is just a single exercise in which you will put into practice all the
knowledge acquired so far.

> ## Enabling issues in forks
>
> By default, when you fork a repository, Issues are disabled. To enable
> them go to `Settings` in the upper right corner, then to `General` in the
> left panel and, finally, scroll down to the `Features` section. There
> click the Issues tickbox to enable them.
{: .callout}

> ## Making a book of recipes
>
> Together with some colleagues, you are writing a book of recipes for sauces
> and you are using git for version control and GitHub to collaborate in the
> writing of the book.
>
> Form groups of 3-4 people and choose one to act as administrator. This
> person should:
>
>- Fork the [Book of Recipes repository](https://github.com/ImperialCollegeLondon/book_of_recipes)
> with the skeleton of the book.
>- Pass the link to the repo to your colleagues.
>
> Now, start collaborating!
>
> - The administrator creates a new release, let's say `1.0.0`, as the starting point of
>   the book.
> - All, including the administrator, open new issues with recipes for sauces you
>  will like to have in the book.
> - Administrator, add some tags, prioritising some of the recipes, and assign
>  yourself or one of your colleagues as responsible for each of them.
>  Remember that if you do not have write access to the repository (which is the case here)
>  and you are not part of the same organisation of the repository, the only way of being
>  assigned to an Issue is by making a comment on the Issue.
> - Fork the administrator's repository. Administrator, did you notice how the
>  number of forks increases? Which GitHub users forked it from you? And from
>  the original repo?
> - Work on the recipes you have been assigned. Practice the concepts learnt
>  in previous episodes about cloning a repository, making the changes locally
>  and pushing those changes back to the remote repository. You can even try a
>  [gitflow approach](https://nvie.com/posts/a-successful-git-branching-model/)
>  if you feel ambitious!
> - When ready, open a PR to the administrator's repo and request their review.
> - Administrator, review the PR, request some changes and accept others. Make sure the
>   relevant checks performed by the continuous integration system are all passing.
> - When ready, merge the PR.
> - Whenever there is a new recipe added, the administrator creates a new release.
>
> These exercises can be repeated with the other members of the group acting
> now as administrators and choosing a different topic for the recipes (eg
>. pasta, roasts, cocktails, etc.).
{: .challenge}

## Bonus: Keeping your fork in sync with the original repo

In the previous exercise, the individual forks will be outdated as you
contribute with content to the administrator's repo. Follow these
instructions to make sure that your own forks are kept up to date.

- [Configuring a remote for a fork](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/configuring-a-remote-for-a-fork)
- [Syncing a fork](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork)

{% include links.md %}
