# Troubleshooting Proxy Issues

## On all operating systems:

1. Make sure you've cleared your cache as per the instructions for your browser/operating system.
2. When you are prompted to enter your username and password, make sure to enter the password you use to login to Ion.

## On Windows:

1. If you're using Chrome or Edge, try these steps:
   1. Open the Settings app.
   2. Navigate to Network & Internet > Proxy.
   3. Enable "Use setup script."
      1. Enter "https://pac.tjhsst.edu/" into the "Script address" box.
   4. Click "Save."
   5. Close Settings, reopen it, navigate back to the proxy settings, and make sure that the settings are still there.
   6. If the settings have been reset, then this is a known Windows issue. Unfortunately, the only confirmed solution is to [use Firefox](firefox.md), which allows you to use different proxy settings from the rest of the system.
      1. Temporarily disabling Windows Defender (and any other antivirus software you have installed) while you access databases _might_ work.

If you require further assistance, please email [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu). Please provide a detailed description of your issue, including your operating system/browsers, in order for us to better assist you.
