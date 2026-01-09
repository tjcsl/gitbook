---
description: Some Frequently Asked Questions (FAQs) about the VS Code offered on OnDemand
---

# Visual Studio Code (VS Code)

The following FAQ's assume that you can open a VS Code session on OnDemand. For information on how to do this, check out the [.](./ "mention")page.

## How can I open a terminal?

At the bottom of your screen, you should see a solid bar with icons, the keyboard layout, and a few other pieces of information (although the exact color scheme may vary)

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

To open a terminal, drag the top of this bar upwards

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

## How can I configure hidden files?

Some files (like `.git`, or other "dotfiles") are not meant to be shown. To prevent these files from showing, go to "Files ⇾ Preferences ⇾ Settings" and search for the `files.exclude` option. Then, add the following patterns:

```
**/__pycache__/
**/.*
```

This will hide by default any files with a name starting with a period `.`, and also folders named `__pycache__` (which contains compiled python byte code). If you would like to add other patterns, note that this setting uses a glob syntax and not regex.
