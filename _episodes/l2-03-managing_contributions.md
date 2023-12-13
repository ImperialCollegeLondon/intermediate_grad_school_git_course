---
title: "Managing contributions to code"
teaching: 20
exercises: 10
questions:
- "What is the difference between forking and branching?"
- "How can my group use GitHub pull requests to manage changes to a code?"
- "How can I suggest changes to other people's code?"
- "What makes a good pull request review?"
objectives:
- Create a pull request from a branch within a repository.
- Create a pull request from a forked repository.
keypoints:
- Forks and pull requests are GitHub concepts, not git.
- Pull request can be opened to branches on your own repository or any other fork.
- Some branches are restricted, meaning that PR cannot be open against them.
- Merging a PR does not delete the original branch, just modifies the target one.
- PR are often created to solve specific issues.
---

## Pull Requests

Pull requests are a GitHub feature which allows collaborators tell each other about changes that have been pushed to a branch in a repository. Similar to **issues**, an open pull request can contain discussions about the requested changes and allows collaborators to review proposed amendments and follow-up commits before changes are either rejected or accepted and merged into the base branch.

> The term "Pull Request" may sound counterintuitive because, from your perspective, you're not actually requesting to pull anything. Essentially it means “Hey, I have some changes I would like to contribute to your repo. Please, have a look at them and pull them into your own.”
>
> You may see the term `merge request` instead of `pull request`. These are exactly the same thing. Different platforms use different terms but they're both asking the receiver of the request to review those changes prior to merging them.
{: .callout}

There are two main workflows when creating a pull request which reflect the type of development model used in the project you are contributing to;

1. Pull request from a branch within a repository and,
2. Pull request from a forked repository.

Essentially, the way you use pull requests will depend on what permissions you have for the repository you are contributing to. If the repository owner has not granted you write permission, then you will not be able to create and push a branch to that repository. Conversely, anyone can fork an existing repository and push changes to their personal repository.

> ## About forks
>
> Before we get into understanding pull requests, we should first get to grips with what a fork is, and how it differs from a branch.
>
> - By default, a public repository can be seen by anyone but only the owner can make changes e.g. create new commits or branches.
> - `Forking` a repository means creating a copy of it in your own GitHub account.
> - This copy is fully under your control, and you can create branches, push new commits, etc., as you would do with any other of your repos.
> - `fork` is a GitHub concept and not Git.
> - Forks are related to the original repository, and the number of forks a given repository has can be seen in the upper right corner of the repo page.
> - If you have some changes in your fork that you want to contribute to the original repo, you open a `pull request`.
> - You can bring changes from an upstream repository to your local fork.
{: .callout}

Now let's take a closer look at those two types of development models;

### 1. Pull request from a branch within a repository

This type of pull request is used when working with a **shared repository model**. Typically, with this development model, you and your collaborators will have access (and write permission) to a single shared repository. We saw in a previous episode how branches can be used to separate out work on different features of your project. With pull requests, we can request that work done on a feature branch be merged into the `main` branch after a successful review. In fact, we can specify that the work done on our feature branch be merged into *any* branch, not just `main`.

Pull requests can be created by visiting the `Pull request` tab in the repository.

>#### Changing *head* and *base* branch
>
>By default, pull requests are based on the parent repository's default branch. You can change both the parent repository and the branch in the drop-down lists. It's important to select the correct order here; the *head branch* contains the changes you would like to make, the *base branch* is where you want the changes to be applied. The arrow between the drop-downs is a useful indicator for the direction of the "pull".
{: .callout}

> ## Now you try
>
> Let's revisit our `recipe` repository.
>
> 1. Create a new branch, make some changes and push the branch to the remote repository.
> 2. Create a pull request with a suitable title and description to merge the branch containing your changes into the main branch.
> {: .callout}
>
>> ## Solution
>>
>> 1. `$ git branch more_avacados`
>> 2. `$ git checkout more_avacados`
>> 3. `$ # make, stage and commit changes`
>> 4. On GitHub.com, navigate to your repository and choose your branch which contains your changes from the "Branch" menu.
>> ![Choose branch]({{ site.baseurl }}/fig/choose_branch.png "Choose branch"){:class="img-responsive"}
>> 5. From the "Contribute" drop-down menu, choose the "Open pull request" button.
>> ![Open pull request]({{ site.baseurl }}/fig/pull_request_button.png "Open pull request"){:class="img-responsive"}
>> 6. From the *base* branch drop-down menu, choose the branch you want your changes to be merged into, and in the *compare* drop-down menu, choose the branch which contains your changes.
>> ![Choose the base and compare branches from the drop-down]({{ site.baseurl }}/fig/base_compare_drop_down.png "Choose the base and compare branches from the drop-down"){:class="img-responsive"}
>> 7. After giving a suitable title and description for your pull request, click the "Create pull request" button.
>> ![Pull request title and description fields and create pull request button]({{ site.baseurl }}/fig/pr_title_description.png "Pull request title and description fields and create pull request button"){:class="img-responsive"}
> {: .solution}
{: .challenge}

> For a deeper dive into this "feature branch workflow", have a read of the Atlassian example - [Git Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
{: .callout}

### 2. Pull request from a forked repository

Forks are often used in large, open-source projects where you do not have write access to the upstream repository (as opposed to smaller project that you may work on with a smaller team). Proposing changes to someone else's project in this way is called the **fork and pull model**, and follows these three steps;

1. Fork the repository.
2. Make the changes.
3. Submit a pull request to the project owner.

This fork and pull model is a key aspect of open-source projects, allowing community contributions whilst reducing the amount of friction for new contributors in terms of being able to work independently without upfront coordination. Another benefit of forking is that it allows you to use someone else's project as a starting point for your own idea. Let's have a go at working through the three steps of the fork and pull model. First step is forking the repository;

> ## Forking a repository
>
> Let's have a go at forking the *book_of_recipes* repository on the Imperial College London GitHub organisation.
>
> 1. First, navigate to the repository at [https://github.com/ImperialCollegeLondon/book_of_recipes](https://github.com/ImperialCollegeLondon/book_of_recipes) and in the top-right corner click **Fork**.
> ![Fork button]({{ site.baseurl }}/fig/fork_button.png "Pull request title and description fields and create pull request button"){:class="img-responsive"}
> 2. Select an owner for the forked repository (if you belong to any GitHub organisations, they will appear here) and give it a suitable name.
> ![Create a fork with repository name emphasised]({{ site.baseurl }}/fig/fork_choose_owner_name.png "Create a fork with repository name emphasised"){:class="img-responsive"}
> 3. Adding a description for your fork is optional. There is also a checkbox asking if you want to copy only the default branch of the repository (in this instance this is called `master`) or whether you want to copy all the branches. In most cases you will only want to copy the default branch. This option is selected by default. Finally, click **Create fork**.
> ![Create a fork with description and create button]({{ site.baseurl }}/fig/fork_description_create.png "Create a fork with description and create button"){:class="img-responsive"}
> **Note:** This fork will be used in the final exercise at the end of this course
{: .challenge}

After forking the repository, the second step is to make our fix/changes. First we will need to clone **our fork** so that we have the files in that repository locally on our computer (`clone` command was covered in the [introductory course](https://imperialcollegelondon.github.io/introductory_grad_school_git_course/l2-02-remote_repositories/index.html)). From here we can go ahead and create a new fix/feature branch and make our changes. When we are happy with the changes we have made, we can `commit` and `push` our upstream, forked repository.

The third and final step in the workflow is to create a pull request. This is done in the same way as in the shared repository model above (navigate to your forked repository, click on the "Contribute" drop-down menu, then click the "Open pull request" button), only this time instead of the base branch being one in your repository, it is a branch in the upstream repository that you forked.

![Drop-down menus for choosing the base fork and branch]({{ site.baseurl }}/fig/fork_pr_choose_branch.png "Drop-down menus for choosing the base fork and branch"){:class="img-responsive"}

Another difference with pull requests from forked repositories is that you can allow anyone with push access to the upstream repository to make changes to your pull request. This is done by selecting **Allow edits from maintainers**.

![Allow maintainers to make edits checkbox]({{ site.baseurl }}/fig/maintainer_edits.png "Allow maintainers to make edits checkbox"){:class="img-responsive"}

> As with the **shared repository model**, Atlassian has a nice [Forking Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow) example if you want a deeper dive.

### Requesting reviewers

- When opening a PR, you can request it to be reviewed by someone else, so there is another pair of eyes making sure that your contribution is correct and does not introduce any bugs.
- Reviewers can just comment on the PR, approve it, or request changes before it can be approved.
- Some repositories might require the approval of one or more reviewers before the changes can be merged into the target branch. This can be set up by the repository manager(s) as a [branch protection rule](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule).
- Only maintainers of the target repository can merge a PR.

### Reviewing a PR

- When reviewing a PR, you will be shown, for each file changed, a comparison between the old and the new version, much like the `git diff` command (indeed, it is `git diff` between the original and target branches, just nicely formatted).
- You can add comments and suggest changes to specific lines in the code.
- Comments and suggestions must be constructive and help the code to become better. Comments of the type “this can be done better” are discouraged. The CONTRIBUTING or the CODE_OF_CONDUCT files often contain information on how to make a good review.

> ## Closing GitHub Issues
>
> The introductory course - [Using GitHub Issues](https://imperialcollegelondon.github.io/introductory_grad_school_git_course/%7C1-06-issues/index.html) - describes how issues work on GitHub, but one handy functionality that is specific to pull requests is being able to automatically close an issue from a pull request.
>
> If a PR tackles a particular issue, you can automatically close that issue
> when the PR is merged by indicating `Close #ISSUE_NUMBER` in any commit
> message of the PR or in a comment within the PR.
 {: .callout}

{% include links.md %}
