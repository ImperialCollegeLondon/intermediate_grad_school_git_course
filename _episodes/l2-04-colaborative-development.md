---
title: "Collaborative development"
teaching: 5
exercises: 35
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
> Form groups of 3-4 people and choose one to act as Administrator.
>
> **Housekeeping** (Administrator's task):
>
> - Create a new repository using the template [Book of Recipes repository](https://github.com/ImperialCollegeLondon/book_of_recipes) which has the skeleton of the book. (**Note**: Click on the green  "Use this template" button on the top left of the Book of Recipes repository and then select "Create a new repository")
>
>   ![Use this template]({{ site.baseurl }}/fig/use_this_template.png "Use this template"){:class="img-responsive" style="float: left; margin-right: 10px; width: 20%;"}
>
>   <div style="clear: both;"></div>
>
> - Set the owner of the new repository as your own GitHub username and the name of the repository the same ("book_of_recipes").
> - Set the description as "Repository for the exercises of the Grad School Git Course".
> - Make sure to select the "Public" option for your reppsitory's visibility and final click on "Create repository".
> - Pass the link to the repo to your colleagues (who will be the contributors).
>
> Now, start collaborating!
>
> **Create a new release** (Administrator's task)
>
> - Create a new release, let's say `1.0.0`, as the starting point of the book.
>
> **Creating new issues** (to be done by everyone):
>
> - Open new issues with recipes for sauces you would like to have in the book.
>
> **Explore existing issues** (Contributor's task):
>
> - Comment on the issues that you want to be assigned to. Remember, you neither have write access to the repository nor you are not part of the same organisation of the repository (in this case the repository belongs to the Administrator is not a part of an organisation), the only way of being assigned to an issue is by making a comment on the issue.
>
> **Task assignment** (Administrator's task):
>
> - Add some tags, prioritise some of the recipes, and assign yourself or one of your colleagues as responsible for each of them.
>
> **Working on a branch, pushing the changes, and opening a PR** (Contributor's tasks):
>
> - Clone the Administrator's repository.
> - Create a branch and work on the recipes you have been assigned. Practice the concepts learnt in previous episodes about making the changes locally and pushing those changes back to the remote repository. You can even try a [gitflow approach](https://nvie.com/posts/a-successful-git-branching-model/) if you feel ambitious!
> - When ready, open a PR to the Administrator's repo and request their review.
>
> **Reviewing and Merging PRs** (Administrator's tasks):
>
> - Review the PR, request some changes and accept others. Make sure the relevant checks performed by the continuous integration system are all passing.
> - When ready, merge the PR.
>
> **Creating a new release** (Administrator's task):
>
> - Whenever there is a new recipe added, create a new release.
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
