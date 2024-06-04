# Firefox

{% hint style="info" %}
You MUST have set your Ion/CSL password before you can set up your TJ Proxy.
{% endhint %}

## Updating Firefox

This guide uses **Firefox Quantum (Firefox version 57.0 and above)**.  Menu designs changed and look very different from every version below that, so the guide will not accurately describe the steps you should take if your version is lower.

Steps to checking what version you have&#x20;

For Windows

1. Press `Alt`
2. Mouse over `Help`
3. Click `About Firefox`

For Macs

1. Click `Firefox` at the top left
2. Click `About Firefox`

![Windows Example](../../.gitbook/assets/updatefirefox-1.png)

![Mac Example](<../../.gitbook/assets/Screen Shot 2019-05-06 at 9.40.44 PM.png>)

If your About Firefox window shows Firefox Quantum, you have a late enough version.  Otherwise, you will need to use this menu to restart and update Firefox until the About Firefox window shows Firefox Quantum.

![Version of Firefox used in this guide](../../.gitbook/assets/updatefirefox-2.png)

![An old version of Firefox ](../../.gitbook/assets/updatefirefox-3.png)

## Clearing Cache

Follow the instructions/screenshots below to setup your proxy:

Open the menu by clicking the three bars at the top-right and click on `Options`

![](../../.gitbook/assets/firefox-1.png)

Locate the search bar to the top right of the screen.

![](../../.gitbook/assets/firefox-2.png)

Search `clear history` and select the `Clear History` button.

![](../../.gitbook/assets/firefox-3.png)

1. Change the time range to `Everything`.
2. Make sure that `Cache` is selected under the History header
3. Select `Clear Now`.

![](../../.gitbook/assets/firefox-4.png)

## Configure Proxy

Now, search `Network Settings` and select `Settings`.

![](../../.gitbook/assets/firefox-5.png)

1. Under the Configure Proxy Access to the Internet header, select `Automatic proxy configuration URL`.
2. Put `https://pac.tjhsst.edu` in the text field below.
3. Select `Reload` and then `OK`.

![](../../.gitbook/assets/firefox-6.png)

## Test it Out

Navigate to the Library Databases page a [https://sites.google.com/view/tjlibraryresources/ejournals-databases](https://sites.google.com/view/tjlibraryresources/ejournals-databases).  Under the Science Databases, select `Access Science`.

If the link provided showed up a s a 404, you are probably logged into your fcpsschools account.  Try [https://sites.google.com/fcpsschools.net/tjlibraryresources/databases-ejournals](https://sites.google.com/fcpsschools.net/tjlibraryresources/databases-ejournals) instead.

![](../../.gitbook/assets/firefox-7.png)

{% hint style="info" %}
You may be presented with a password prompt. If so, type the username and password you use to log in to Ion.
{% endhint %}

![Authentication prompt to access the proxy](../../.gitbook/assets/firefox-8.png)

Once you have successfully authenticated, you should see `Access via Thomas Jefferson High School` at the top right of the site.

![Successful connection to Access Science via the proxy](../../.gitbook/assets/firefox-9.png)

If you encounter any issues while setting up the proxy, please see [Troubleshooting Proxy issues](troubleshooting-proxy-issues.md).
