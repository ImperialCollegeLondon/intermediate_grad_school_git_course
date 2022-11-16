---
title: "Code versions, releases and tags"
teaching: 0 # TBD
exercises: 0 # TBD
questions:
- "What is a Git tag and how does it differ from a branch?"
- "How and when should I release a new version of my code?"
- "What is the difference between major and minor version changes?"
- "How can I effectively communicate what has changed between code version?"
objectives:
- "Understand what is meant by a release and a version"
- "Understand how to give your software meaningful version numbers with semantic versioning"
keypoints:
- "A version of your code with a release number (e.g. v13.4.2) is referred to as a *release*"
- "A version of your code represented by a commit hash (e.g. 047e4fe) is just referred to as a *commit*"
---

## Background: To release or not to release?

All of you will already be familiar with the concept of software versioning. Often when
you download a piece of software from its website it'll tell you that it's `v13.4.2` (or
whatever) that you're downloading. [!!!!!!1]

Releasing software with an explicit version number like this is a common practice and
one that you may eventually consider for some of your own projects. We will show you how
to do this using Git and GitHub. Even if you never end up making a release yourself,
rest assured that sooner or later you will have to work with a repository which uses
releases like this, so it's important to understand the concepts at least.

The first question you might have is:

### Why do I need a "version" for my software? Isn't Git tracking the version anyway?

Sort of. The problem here is that the word "version" can mean several different things
in the context of Git. Ordinarily, when people talk about a particular version of a
piece of software, they mean a version with a particular release number, such as
`v13.4.2`. However, in another sense, each commit in your repository represents a
different version of the code and can be represented by a unique commit hash (e.g.
`047e4fe`).

To avoid confusion, people usually refer to the first kind of version as a "release" and
the second as a "commit", which is what we'll do here.

### When *don't* I need to create releases for my software?

You probably don't need to create releases for your software if:

- The code never changes
- You're the only developer and the only user (e.g. it's some analysis scripts that only
  you use)
- (OR there are only a few of you and you all just use the latest commit etc.)
- The software is simple and doesn't change much from one commit to the next

In this case, if you just need to clarify which commit you're working from (e.g. to a
colleague), remember that you can always obtain the current commit hash like so:

~~~sh
$ git rev-parse --short HEAD
~~~
{: .commands}
~~~
047e4fe
~~~
{: .output}

### When should I consider creating releases?

You *might* consider creating releases if:

- You have a large, distributed user base and you don't want them just using some
  random commit from your repository
- You periodically add or change features in a non-backwards-compatible way
- You want to bundle these features together and advertise them to your users
- Your software is complex and you can't guarantee that every commit on `main` has been
  thoroughly tested
- You want to be able to support several versions of your software simultaneously (e.g.
  you intend to maintain a v2 of your software, even after v3 has been released)
- You want to provide a place to download files that you've generated (such as `.exe`
  files) and distribute them to users

In general, though, creating a release is also just a convenient way to label a
particular commit, so it's a useful way to ensure that the users you're sharing it with
-- who may not be technically savvy -- are using a specific commit, rather than simply
whatever the latest commit on `main` is.

## Labelling a particular commit with `git tag`



{% include links.md %}
