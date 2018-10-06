# PR Workflow

## Prerequisites

Knowledge of Git is a good to have for Ion development since we store code in Git and use GitHub 

## Branching

Work for production is stored in the `master` branch in the [Ion repo on GitHub](https://github.com/tjcsl/ion).  The `dev` branch stores information for Ion developers to test code for production. 

Non-Ion maintainers should develop on their own forks of the main GitHub repository.  You can read about it [here](https://help.github.com/articles/fork-a-repo/).

## Preparation

When you think your code is ready to reviewed, it is important to ensure your changes comply with the Ion code guidelines.  Here are some general guidelines

* First, you should review your code for compliance with the [Ion Style Guide](style-guide.md).  
* Second, you should make sure that only changes you wanted to make exist in your branch.
* Third, you should run the Ion test suite.  You can run the tests by running `./setup.py test` in the root work directory.  If the test fail, you should fix your code.

{% hint style="info" %}
In order to merge into master, it is recommended to write tests for any new code. If there is missing testing code, you should also write tests.  To see some examples, look for `test.py` files in each app. More information can be found in the [Django docs](https://docs.djangoproject.com/en/dev/topics/testing/overview/).
{% endhint %}

* Fourth, you should ensure that all files within the Git repository are listed in `Ion.egg-info/SOURCES.txt` and in your commit(s).  This can be automatically done by running `./setup.py test`.
* Fifth, commit your changes and write a simple and *descriptive* commit message.  Messages like `fixes` are NOT descriptive.
