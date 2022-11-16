---
title: "Collaborating with Git and GitHub"
teaching: 10
exercises: 0
questions:
- "How does collaborative working differ from individual working?"
- "What are the challenges of working collaboratively with Git?"
objectives:
- "Explain some challenges and benefits of collaborative working"
keypoints:
- "Collaborative working poses additional challenges compared to individual working"
- "Git and GitHub provide powerful tools to help teams to work together"
---

## Working in a Team

Collaborating effectively as part of team brings additional challenges and
opportunities compared to solo development.

### Access and Permissions

Developer 1 - "Just email me your changes. I'll save them into the master copy."

Developer 2 - "Ok... so why do all of my changes have to go through you?"

An important practical consideration is where to store the code that you're
collaborating on. Usually you want everyone's contributions to end up in one
place and that place only being accessible by a particular individual is
unsustainable. On the other hand you do need to be able to control how
contributions are made.

Hosting your code on GitHub allows configuration of user access and granular
permissions. This allows shared responsibility and the clear definition of roles
within a project.

### Differing Goals and Objectives

Developer 1 - "I need a new type of analysis to finish my thesis"

Developer 2 - "My problem is bigger. I need better performance to process all my
data"

Even when working independently you might find you need to need to work on
different things at different times. This is greatly compounded however when you
have multiple developers all wanting to contribute to the same Git repository.

We will see how Git allows you to have multiple simultaneous streams of work via
**branching and merging**. You can use branches to organise your individual work
and as a way to ensure your Git history doesn't clash with other
contributors. The ability to merge branches even supports working on the same
part of the code as somebody else so you can work on whatever you want and worry
about sorting out conflicts later.

### Different Points of View

Developer 1 - "Here's what I've been working on for the last month."

Developer 2 - "Hmmm... if we tweak things here then it might be faster."

Two heads, as they say, are better than one and writing software is no
exception. There is no greater benefit to collaboration than being able to pick
someone else's brain about a problem. In software development this is usually
called peer review and it's considered good practice for all code to be
independently looked over.

GitHub provides functionality for peer review via **Pull Requests**.

### Need to Coordinate Efforts

Developer 1 - "I'm still waiting on those changes to the data analysis workflow."

Developer 2 - "Huh? I added those a month ago."

Successfully coordinating the efforts of multiple contributors is a key
challenge to avoid delay and duplication of work. GitHub can help here via
**Issues** that track planned, on-going and completed work and who is doing it.

### Individual styles and preferences

Developer 1 - "Tabs!"

Developer 2 - "Spaces!"

Developer 1 - "TABS!"

Developer 2 - "SPACES!"

Whilst it may seem trivial the tabs vs. spaces controversy is a long standing
debate. A quick google search will reveal any number of discussions on the
topic. Ultimately, it doesn't matter, but trouble can arise when everyone
follows their own preference and you end up with a messy combination. The same
logic applies in many places - consistency is king!

To get around these sorts of issues it's a good idea to make a choice and then
automatically enforce it. GitHub Actions is a **Continuous Integration** system
that can be used to automate many kinds of checks to ensure a consistent set of
preferences or standards for all code.

## Summary

Coding as a team presents a number of challenges and opportunities. Both Git and
GitHub were specifically designed to help you mitigate those challenges and
embrace those opportunities. In the rest of this course we'll be looking in
detail at the range of functionality that is touched on above.
