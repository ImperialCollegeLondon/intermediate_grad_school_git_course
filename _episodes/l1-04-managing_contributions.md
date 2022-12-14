---
title: "Managing contributions to code"
teaching: 0 # TBD
exercises: 0 # TBD
questions:
- "What is the difference between forking and branching?"
- "How can my group use GitHub pull requests to manage changes to a code?"
- "How can I suggest changes to other people's code?"
- "What makes a good pull request review?"

objectives:
- Create a pull request from a branch within a repository.
- Create a pull request from a forked repository.
keypoints:
- TODO
---

## Pull Requests
Pull requests are a GitHub feature which allows collaborators tell each other about changes that have been pushed to a branch in a repository. Similar to **issues**, an open pull request can contain discussions about the requested changes and allows collaborators to follow up with proposed ammendments and follow-up commits before changes are either rejected or accepted and merged into the base branch.

## Branching and Forking
There are two main workflows when creating a pull request which reflect the type of development model used in the project you are contributing to;
1. Pull request from a branch within a repository and,
2. Pull request from a forked repository.

> ## About forks
> Before we get into understanding pull requests, we should first get to grips with what a fork is, and how it differs from a branch.
> - By default, a public repository can be seen by anyone but only the owner can make changes e.g. create new commits or branches.
> - `Forking` a repository means creating a copy of it in your own GitHub account.
> - This copy is fully under your control, and you can create branches, push new commits, etc., as you would do with any other of your repos.
> - `fork` is a GitHub concept and not Git.
> - Forks are related to the original repository, and the number of forks a given repository has can be seen in the upper right corner of the repo page.
> - If you have some changes in your fork that you want to contribute to the original repo, you open a `pull request`.
> - You can bring changes from an upstream repository to your local fork.
>
>Essentially, the way you use pull requests will depend on what permissions you have for the repository you are contributing to. If the repository owner has not granted you write permission, then you will not be able to create and push a branch to that repository. Conversely, anyone can fork an existing repository and push changes to their personal repository.
{: .callout}

Now let's take a closer look at those two types of development models;

### 1. Pull request from a branch within a repository
This type of pull request is used when working with a **shared repository model**. Typically, with this development model, you and your collaborators will have access (and write permission) to a single shared repository. We saw in a previous episode how branches can be used to separate out work on different features of your project. With pull requests, we can request that work done on a feature branch be merged into the `main` branch after a successful review. In fact, we can specify that the work done on our feature branch be merged into *any* branch, not just `main`. We'll see how this can be done a bit later in this episode.

Pull requests can be created by visiting the `Pull request` tab in the repository.

>#### Changing *head* and *base* branch
>
>By default, pull requests are based on the parent repository's default branch. You can change both the parent repository and the branch in the drop-down lists. It's important to select the correct order here; the *head branch* contains the changes you would like to make, the *base branch* is where you want the changes to be applied.

>![Open a pull request]({{ site.baseurl }}/fig/create_pull_request.png "Open a pull request"){:class="img-responsive"}
{: .callout}

> ## Now you try
> Let's revisit our recipe repository. If you have lost your copy of the recipe repository you can download a completed copy [here](https://imperialcollegelondon.github.io/introductory_grad_school_git_course/code/recipe_with_history.zip)
> 1. Create a remote repository with your recipe repository.
> 2. Create a new branch with some changes and push the branch to the remote repository.
> 3. Create a pull request with a suitable title and description to merge the branch containing your changes into the main branch.
>
>> ## Solution
>> 1. On GitHub.com, navigate to your repository and choose your branch which contains your changes from the "Branch" menu.
>> ![Choose branch]({{ site.baseurl }}/fig/choose_branch.png "Choose branch"){:class="img-responsive"}
>> 2. From the "Contribute" drop-down menu, choose the "Open pull request" button.
>> ![Open pull request]({{ site.baseurl }}/fig/pull_request_button.png "Open pull request"){:class="img-responsive"}
>> 3. From the *base* branch drop-down menu, choose the branch you want your changes to be merged into, and in the *compare* drop-down menu, choose the branch which contains your changes.
>> ![Choose the base and compare branches from the drop-down]({{ site.baseurl }}/fig/base_compare_drop_down.png "Choose the base and compare branches from the drop-down"){:class="img-responsive"}
>> 4. After giving a suitable title and description for your pull request, click the "Create pull request" button.
>> ![Pull request title and description fields and create pull request button]({{ site.baseurl }}/fig/pr_title_description.png "Pull request title and description fields and create pull request button"){:class="img-responsive"}
> {: .solution}
{: .challenge}

### 2. Pull request from a forked repository
Forks are often used in large, open-source projects where you do not have write access to the upstream repository (as opposed to smaller project that you may work on with a smaller team). Proposing changes to someone else's project in this way is called the **fork and pull model**, and follows these three steps;
1. Fork the repository.
2. Make the fix.
3. Submit a pull request to the project owner.

This fork and pull model is a key aspect of open-source projects, allowing community contributions whilst reducing the amount of friction for new contributors in terms of being able to work independently without upfront coordination. Another benefit of forking is that it allows you to use someone else's propject as a starting point for your own idea.

Pull requests from forked repositories work slightly differently to that of pull requests from a branch within a repository.

> ## Forking a repository
> Let's have a go at forking the *book_of_recipes* repository on the Imperial College London GitHub organisation.
> 1. First, navigate to the repository at [https://github.com/ImperialCollegeLondon/book_of_recipes](https://github.com/ImperialCollegeLondon/book_of_recipes) and in the top-right corner click **Fork**.
> ![Fork button]({{ site.baseurl }}/fig/fork_button.png "Pull request title and description fields and create pull request button"){:class="img-responsive"}
> 2. Select an owner for the forked repository (if you belong to any GitHub organisations, they will appear here) and give it a suitable name.
> ![Create a fork with repository name emphasised]({{ site.baseurl }}/fig/fork_choose_owner_name.png "Create a fork with repository name emphasised"){:class="img-responsive"}
> 3. Adding a description for your fork is optional. There is also a checkbox asking if you want to copy only the default branch of the repository (in this instance this is called `master`) or whether you want to copy all the branches. In most cases you will only want to copy the default branch. This option is selected by default. Finally, click **Create fork**.
> ![Create a fork with description and create button]({{ site.baseurl }}/fig/fork_description_create.png "Create a fork with description and create button"){:class="img-responsive"}
{: .challenge}

{% include links.md %}
