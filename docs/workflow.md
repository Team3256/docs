---
sidebar_position: 2
---

# Workflow [WIP]

We use a variety of tools to manage our codebase, and this page will give you an overview of how we use them.

## Linear

Linear is a project management tool that we use to keep track of our tasks and issues. We use Linear to manage our backlog, plan our sprints, and track our progress.

## Github

Github is where we store our code - it's a version control system\* that allows us to keep track of changes to our codebase, and collaborate with others.

### Version Control

We use Git for version control. Git is a distributed version control system that allows us to track changes to our codebase over time. We use GitHub to host our repositories, and we use GitHub Actions for continuous integration. Ideally, we would follow the [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow) where you would create a branch with your changes, make a pull request, [ask for reviewers](#code-review), and then get it merged. If you have multiple changes that need to be built on each other, we encourage separating your dependent changes as smaller branches off of what you wrote. This way, you can create a small pull request for every dependent feature you wrote (many small pull requests is faster to review than 1 giant pull request) which allows us to merge them one after another. This is also known as the [stacking workflow](https://www.stacking.dev/), sort of an extension of GitHub Flow.

### Continuous integration

Continuous integration is basically a fancy term for the process that's configured to run on every time code changes (i.e. on a new commit). Our current CI currently only:

- Checks if the code follows consistent formatting (via [Spotless](https://github.com/diffplug/spotless))
- Compiles our code
- Runs the tests that we configured in our code. Testing is a whole other topic, but you don't need to worry about it because we don't do that many unit tests in our code (because you could just test if it works using the robot or sim).

### Code Review

We don't like having one central person do code review - we prefer to have everyone review each other's code. We use GitHub Pull Requests for code review, and we have a policy of requiring at least one approval from anyone before merging a Pull Request.

<details>
<summary>
Why do I have administrative overrides for merging Pull Requests? 
</summary>

We have a policy of requiring at least **one** approval from anyone before merging a Pull Request.

However, we also have a policy of allowing the person who opened the Pull Request to merge it if they feel that the code is ready to be merged. This is because we trust our team members to know when their code is ready to be merged, and we don't want to slow down development by requiring multiple approvals for every Pull Request.

</details>

