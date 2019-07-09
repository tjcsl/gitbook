# PR Workflow

## Prerequisites

Knowledge of Git is a good to have for Ion development since we store code in Git and use GitHub

## Branching

Production code is stored on the `master` branch in the [Ion repo on GitHub](https://github.com/tjcsl/ion). The `dev` branch stores code used for testing before deploying to production.

Non-Ion maintainers should develop on their own forks of the main GitHub repository. You can read about it [here](https://help.github.com/articles/fork-a-repo/).

## Preparation

When you think your code is ready to reviewed, it is important to ensure your changes comply with the Ion code guidelines. Here are some general guidelines

* First, you should review your code for compliance with the [Ion Style Guide](style-guide.md).  
* Second, you should make sure that only changes you wanted to make exist in your branch.
* Third, you should make sure your code will pass the build by running `./deploy` in the root directory. This will run tests, update `Ion.egg-info` and build the docs. At the end, you will probably get a message saying there are uncommitted changes. **This is fine**. As long as the script ends with an uncommitted changes message, you are ready to commit your code.

{% hint style="info" %}
In order to merge into master, it is recommended to write tests for any new code. If there is missing testing code, you should also write tests. To see some examples, look for `test.py` files in each app. More information can be found in the [Django docs](https://docs.djangoproject.com/en/dev/topics/testing/overview/).
{% endhint %}

* Fourth, commit your changes and write a simple and _descriptive_ commit message.  Messages like `fixes` are **NOT** descriptive.

## Scope

The scope of your PR should be limited.

You should not make PRs changing various unrelated portions of the code.  For example, a PR focused on adding a new feature to printing should not be removing a test case for user authentication  \(although you are free to open a separate PR for that\).  In addition, you should not fix failed CI tests in the same PR \(unless your code was the source of the errors\).

## Opening a PR

After pushing your proposed changes to your own personal fork, it is time for you to open a PR.

* Head over to your fork and navigate to your branch via the drop down menu.  At the top of your code, you should see a button called "Pull request".  After you click it, you should see the opening of PR page.
* On this page, ensure your head fork is set to the branch with your changes, while the base fork is set to the Ion repository's `dev` branch.

{% hint style="warning" %}
The _only_ people should open PRs with the `master` branch as the base branch should be individuals with push access to at least the _dev_ branch.  These PRs against against the master branch should use the `dev` branch as the head fork.
{% endhint %}

* You must see a green check mark indicating that branch is able to merge.  If it is not, you should rebase and make the branch mergeable before proceeding.
* Write an informative title and describe your changes briefly in the description box.
  * Your changes should describe why you are changing something, how you are changing something, and what you are changing.  
  * A good, informative PR description helps the maintainers review your changes faster.
* Make sure only the changes you want to make are described in the PR.
* Submit it!

## Responding to Reviews

After your PR has been submitted, an Ion maintainer will review your PR.  They can take three courses of action after reviewing:

* approving the PR
* closing the PR
* or requesting changes to the PR

If your PR has been approved, there is no need to take any further action unless requested to.  The maintainer may wait for other maintainers to review your the code or may immediately merge the PR.

If the PR has been closed, the reason is generally described by the closing maintainer.  

If a maintainer has requested changes, you should correct your code per the recommendations and push additional commits to your head branch \(or just rebase your branch\).  





