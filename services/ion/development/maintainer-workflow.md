# Maintainer Workflow

Maintainers of the Ion repository are responsible for the effective functioning of Ion.  Generally, the Ion maintainers are the Ion leads for the years plus other senior Ion developers if applicable.  

## Responsibilities

Ion maintainers need to ensure that the code base remains good quality and nothing will break.

Maintainers have the following responsibilities:

* Triaging issues \(labeling, closing issues as appropriate\)
* Reviewing pull requests
* Maintaining our CI pipeline \(run via Travis\)
* Ensuring the CI pipeline does not fail 
* Ensuring the code's compliance with the Ion Style Guide
* Protecting Ion from security threats
* Redacting private information from the Git repository
* Determining the best course of action to take \(being the final arbiters of decisions affecting Ion\)

## Issues

We have a system of labeling issues that are opened in the Ion repository.  Examples of labels that we have include:

* Browser compat
* Bug
* Cache
* Client-side
* Docs
* Enhancement
* Feature
* Feedback
* Help Wanted
* Invalid
* Question
* Server-side
* Vagrant
* Upstream
* Wontfix

Issues out of the repository's scope should be closed.  Duplicates of other issues should also be closed.  Maintainers should however be respectful of people who open issues.

## Pull Requests

Determining what code should be included in the Ion repository is a decision that lies ultimately with the Ion Lead.  Together with the Lead Sysadmins and the Faculty Sponsor, they serve as the FINAL decision makers on whether to include or remove from Ion.

Pull requests should be reviewed fairly regularly so that contributors can respond to the feedback that is given by the reviewers.  The process for determining who should authorize/approve merges into the `dev` and `master` branches is to be determined each year by the Ion Lead.

## Evaluating Pull Requests

When evaluating pull requests, maintainers look at various factors including:

> * Correctness: Does the code do what it claims to do? Is the code correct in both the nominal case and the boundary cases? As a reviewer, this is your opportunity to point out edge conditions of which the original developer may not have been aware.
> * Complexity: Does the code accomplish its task in a reasonably straightforward way? If you can point out simpler approaches that do not compromise the correctness or performance of the code, you should.
> * Consistency: Does the code achieve its basic goals in a way that is consistent with how similar code in our codebase achieves those goals? Is it re-using the available libraries and utility classes? Where possible, has code been refactored for re-use instead of just copying and pasting?
> * Maintainability: Could the code be extended by another developer on the team with a reasonable amount of effort? More than any item on the list, this is the karma investment you make by doing code reviews - the code you review today may be the code you have to update tomorrow, so taking the time to make sure it’s maintainable by others pays itself back to you.
> * Scalability: Will the code be performant at the expected volumes? It is important that this question always be asked in the context of expected volumes. When building a new product in an untested market, it is fine to write code that works for 10,000 users but not 1M; if the product should be that successful, we will profile, optimize, and, when necessary, re-write the critical bits. The corollary is that we should not spend time optimizing code when the market demand is unproven.
> * Style: Does the code match the team style guide? This should rarely be controversial.
>
> Adapted from [http://engblog.yext.com/post/effective-code-reviews](http://engblog.yext.com/post/effective-code-reviews)

PRs that add new features **should** include unit tests that cover all added code.



 

