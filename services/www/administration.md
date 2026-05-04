# Administration

This page describes how to accomplish certain administration tasks on [WWW](./)

## SSL

We use Let's Encrypt for SSL, using Certbot. Let's Encrypt certificates expire every 90 days and are renewed every 60 days. Renewal is automated, but several other servers use the wildcard certificate and must pull the updated one. The most important of these are the mail servers, which use the certificate for SMTP. The script `update-ssl.sh` in the root home directory of Casey and Smith should handle this. After certificates are renewed, run the update-ssl script on:

* Mail servers (Smith and Casey)
* IPA servers, for the web ui
* Monitor/Grafana

## Scripts

This section contains various other scripts to do useful things on [WWW](./).

### What to do if the webserver goes down

1. Log in to remote.tjhsst.edu (or if you're already on the internal network, that's fine too)
2. `ssh root@www`
3.  `systemctl restart nginx`

    This restarts nginx and ensures that the service manager is still in a consistent state. The website should work after this (if not, try clearing cache/etc, it's possible a redirect to an error page might've been cached, although it shouldn't be).

### If SSL doesn't renew automatically

The certbot command is `certbot certonly`\
`--manual \`\
`--preferred-challenges dns \`\
`--manual-auth-hook /usr/local/bin/certbot-ipa-dns-update.sh \`\
`--deploy-hook "nginx -s reload" \`\
`--manual-cleanup-hook /usr/local/bin/certbot-ipa-dns-cleanup.sh \`\
`-d tjhsst.edu \`\
`-d '*.tjhsst.edu' \`\
`--non-interactive --agree-tos -m lead-sysadmins@tjhsst.edu --no-eff-email \`\
`--expand`

You can try running this manually to see the error. You can also look at the script in `/usr/local/bin/certbot-ipa-dns-update.sh`  to see what it's supposed to do.&#x20;
