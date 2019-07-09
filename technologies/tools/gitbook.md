# GitBook

GitBook is the documentation platform used by the Computer Systems Lab. It is the central hub of knowledge for the Lab and is hosted on [gitbook.com](https://gitbook.com), a proprietary platform.

## History

The CSL's GitBook came into existence after frustration with inadequate documentation on Livedoc. Initially began by Theo Ouzhinski in late August of 2018, GitBook now serves as the CSL's official documentation platform.

## Structure

GitBook, the website, allows the creation of a GitBook account via e-mail, GitHub, or Google. Every account can be a member of a different organizations which in turn have their own workspaces. Workspaces can be either publicly available or restricted.

The TJCSL organization contains the TJ CSL workspace which contains all of our public documentation for Sysadmins.

The official GitBook documentation can be found [here](https://docs.gitbook.com/).

## Access Control

The Lead Sysadmins, Faculty Sponsor, and Documentation Lead should be owners/admins of the TJCSL organization on GitBook. Other Sysadmins should have write access to the documentation on GitBook. The expectation is that you do not violate the trust that is given to you.

The Lead Sysadmins, Faculty Sponsor, and Documentation Lead should be admins of the `gitbook` repository on GitHub. At their discretion, they may grant write access to other Sysadmins, but there is always the option of relying on pull requests for contributions.

## Editing

Editing for GitBook can be done via a git repo or the visual editor. Changes to the organizational structure \(adding pages, moving pages, removing pages, etc.\) should generally be made through the online visual editor. Editing/adding to current documents can be done through Markdown. GitBook-specific features \(page links, hints, tables\) rely on custom solutions that cannot be easily done in Markdown. Hence, you should use the visual editor for creating/editing those features.

GitBook does a pretty good job at explaining their setup in their [docs](https://docs.gitbook.com).

## GitHub Integration

GitBook's GitHub integration allows editors to not only edit via the visual editor but through a git repo. Currently, the documentation's GitHub repository is located at [https://github.com/tjcsl/gitbook](https://github.com/tjcsl/gitbook). Pushes to the GitHub repo are mirrored to GitBook while published changes on GitBook are mirrored to GitHub. You can see the status of the sync at the bottom right of the editing screen: `GitHub syncing`. This sync can take up to one minute to complete. Generally, changes in reverse should be avoided to prevent conflicts.

