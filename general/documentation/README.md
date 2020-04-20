# Documentation

## Overview

Overall documentation in the Computer Systems Lab is done through GitBook.

{% page-ref page="../../technologies/tools/gitbook.md" %}

Writing good documentation is key to ensuring that knowledge can continue to be passed onto future Sysadmins.

## Organization

The CSL's documentation is organized into five main sections:

* Services: User-facing services
* Technologies: Technologies used by the Lab and Sysadmins
* Machines: Various machines \(servers, switches\) used by the Lab
* General: Anything that doesn't fall under another section
* Procedures: Various procedures

## Contributing

There are two ways to contribute to the GitBook documentation:

* tjcsl.gitbook.io
  * This is the preferred  Ask a GitBook admin for an invite link on Mattermost if you don't have one.
* GitHub
  * You can open up a pull request to change the documentation.  It will be synced to GitBook once it is merged into master.

In the GitBook web editor, use `Ctrl+/` to display various commands. Read the GitBook article for more info.

In the GitHub repo, make commits as you work in Markdown.

## Runbooks



## Writing Good Documentation

Here are a few pieces of advice to writing good documentation:

* Assume readers know nothing.
  * Documentation will most likely be used much later.  If you talk about items that future readers' don't know about, they won't be able to understand your documentation.
* Simple is better than overly complex. 
  * Using complex terminology and straying too much off topic leaves readers perplexed.  Simple explanations are just better.
* Keep it organized.
  * Use headers to keep relevant topics.  Also remember to write docs in the write docs.  Good organization makes it easier to find the information that matters the most. 
* Document what you had wanted to know.
  * When you started interacting/working in a certain area, what pieces of information would you want to know?  Documentation is constantly evolving and needs to be updated to keep up with the latest changes in the Lab.
* Use explicit commands / source code along with explanations. Clearly differentiate what is generic to the technology versus what is specific to our setup.

## Historical Documentation

### Livedoc

Livedoc is a self-hosted MediaWiki installation hosted on Director at [livedoc.tjhsst.edu](https://livedoc.tjhsst.edu). It was originally set up on November 2, 2005 by Dan Tran. It is currently not being updated.

### The Book

**The Syslab Book** was a documentation project numerous years in the works, with contributions from Andy Barros, Matthew Colyer, Andrew Deason, Susan Ditmore, Jeffrey Grafton, John Livingston, Ilia Mirkin, Kyle Moffett, and Dan Tran. Prior to Livedoc, this was essentially the only documentation on the SysLab.

The Book used DocBook format for all of its pages \(which is SGML, but mostly looks like XML\). It was thought by some to be a rather cumbersome format to update and use in general, which is one of the reasons Livedoc exists.

