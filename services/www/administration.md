# Administration

This page describes how to accomplish certain administration tasks on [WWW](./)

## SSL

Occasionally, SSL certificates need to be renewed. We renew certificates yearly. The next time we will need to renew the SSL certificates is late this November.

### Before certificate renewal comes up

Generate a new private key and CSR with:

```text
openssl req -new -newkey rsa:2048 -nodes -keyout "tjhsst-1718.key" -out "tjhsst-1718.csr"
```

Substitute 1718 with the appropriate school year. This command should be run in `/etc/apache2/ssl` -- be very careful to not overwrite existing files \(i.e. make sure `tjhsst-1718.{key,csr}` don't already exist\).

Get the public key pin information using

```text
openssl rsa -in tjhsst-1718.key -outform der -pubout | openssl dgst -sha256 -binary | openssl enc -base64
```

You'll need to add this to the Public-Key-Pins header in `/etc/nginx/ssl.conf`, following the existing format. Do this _**before**_ you actually rotate keys. Without doing this, browsers will be unable to access the website, and this is a bad thing. Read \(the documentation on MDN\)\[[https://developer.mozilla.org/en-US/docs/Web/HTTP/Public\_Key\_Pinning](https://developer.mozilla.org/en-US/docs/Web/HTTP/Public_Key_Pinning)\] for more information about public key pinning.

Alternatively, generate a CSR using an already existing key, with:

```text
openssl req -out tjhsst-1718.csr -key tjhsst-1617.key -new.
```

### Rotating the certificate

Once you've received a certificate from the CA, put it alongside the private key using a similar naming format, like `tjhsst-1718.crt`. Create a certificate bundle/chained certificate file according to the instructions given by the CA \(usually this looks something like `cat tjcsl_bundle.crt tjhsst-1718.crt > tjhsst-1718.chained.crt`\).

You can now update the web server's SSL configuration in `/etc/nginx/ssl.conf`, making sure to replace the values of `ssl_certificate`, `ssl_certificate_key`, as well as `ssl_trusted_certificate` if it's necessary.

### Restart the web server

You can now restart nginx with `/etc/init.d/nginx restart`. If all goes well, the new SSL certificate should be in place.

## Scripts

This section contains various other scripts to do useful things on [WWW](./).

### What to do if the webserver goes down

1. Log in to remote.tjhsst.edu \(or if you're already on the internal network, that's fine too\)
2. If you're on remote, `kinit username/root`
3. `ssh root@www`
4. `reload-webserver`

   This restarts nginx/Apache and ensures that the service manager is still in a consistent state. The website should work after this \(if not, try clearing cache/etc, it's possible a redirect to an error page might've been cached, although it shouldn't be\).

If this doesn't work, there are a few things you can try:

```text
pkill k5start
pkill -9 k5start
pkill apache2
pkill -9 apache2
pkill nginx
pkill -9 nginx
service apache2 zap
service nginx zap
service apache2 start
service nginx start
```

This will make sure k5start/nginx/apache have actually been stopped \(although possibly not cleanly\) before restarting them. If this doesn't work, it's probably an issue with Kerberos / AFS -- make sure `/etc/krb5.keytab.www-data` exists and has the correct keys \(`ktlist -K -k /etc/krb5.keytab.www-data`, you should see `www-data@CSL.TJHSST.EDU` listed at least once\).

If all of that doesn't work, it's most likely not a problem with the web server -- perhaps check AFS or Kerberos for issues that might be causing a web problem.

### Granting a user access to edit the website

You'll need to be an [AFS admin](../../technologies/storage/afs/administration.md), or ask someone who is, to simply run:

```text
pts adduser <USERNAME> web.admins
```

This will grant full access to `/afs/csl/web/www`, where the website files are located.

