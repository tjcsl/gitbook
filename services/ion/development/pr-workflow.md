---
description: This page describes how to create PRs against Ion
---

# PR Workflow

## Prerequisites

Knowledge of Git is a good to have for Ion development since we store code in Git and use GitHub

## Branching

Work for production is stored in the `master` branch in the [Ion repo on GitHub](https://github.com/tjcsl/ion). The `dev` branch stores information for Ion developers to test code for production.

Non-Ion maintainers should develop on their own forks of the main GitHub repository. You can read about it [here](https://help.github.com/articles/fork-a-repo/).

## Preparation

When you think your code is ready to reviewed, it is important to ensure your changes comply with the Ion code guidelines. Here are some general guidelines

* First, you should review your code for compliance with the [Ion Style Guide](style-guide.md).  
* Second, you should make sure that only changes you wanted to make exist in your branch.
* Third, you should make sure your code will pass the build by running `./deploy` in the root directory. This will run tests, update `Ion.egg-info` and build the docs. At the end, you will probably get a message saying there are uncommitted changes. **This is fine**. As long as the script ends with an uncommitted changes message, you are ready to commit your code.

{% hint style="info" %}
In order to merge into master, it is recommended to write tests for any new code. If there is missing testing code, you should also write tests. To see some examples, look for `test.py` files in each app. More information can be found in the [Django docs](https://docs.djangoproject.com/en/dev/topics/testing/overview/).
{% endhint %}

* Fourth, commit your changes and write a simple and _descriptive_ commit message.  Messages like `fixes` are NOT descriptive.

