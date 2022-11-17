---
title: "Using GitHub actions for continuous integration"
teaching: 30
exercises: 15
questions:
- "What is meant by continuous integration (CI) and what are the benefits?"
- "What tasks can be automated in CI?"
- "How do I set up CI using GitHub Actions?"
- "How do I know if CI runs are passing and what should I do if they are failing?"
- "What should I do if I can't replicate failing runs locally?"
objectives:
- "Understand the role of Continuous Integration (CI) in collaborative development"
- "Know how to write a simple GitHub Actions configuration file"
- "Be able to design a CI workflow for a variety of projects"
keypoints:
- "Continuous Integration (CI) is the practice of automating checks of code contributions"
- "GitHub Actions is a CI system provided by GitHub"
- "GitHub Actions is configured via a YAML file in the directory `.github/workflows`"
- "GitHub Actions comprise individual steps combined into workflows"
- "Steps may run a pre-existing action or custom code"
- "The result of a GitHub Actions run can be used to block merging of a Pull Request"
- "CI can be used for a wide variety of purposes"
---

## Explanation of CI

- event-driven computational workflows applied to your code
- CI used as a gate-keeping device to enforce a common set of checks/quality on
  commits
- Many different CI systems e.g. CircleCI, TravisCI we're going to use GitHub
  Actions because it's well integrated with GitHub.
- Mention CD used to share e.g. publish.

## Introduction to GitHub Actions

- Controlled via a special file in a repository (brief intro to yaml)
- Representation in GitHub UI
- Workflows -> steps
- Concept of an "action" vs. running custom code in a step
- Triggers
- Filters
- Limitations on non-public repositories (plug ICL org)
- exercise to setup and run MDL action in Guacamole repository
- requiring passing CI for merging a PR

## Ways to use CI

- many many actions exist
- enforce style and checks
- run builds
- run tests
- interface with external systems e.g. publish
- use GitHub services e.g. Dependabot

{% include links.md %}
