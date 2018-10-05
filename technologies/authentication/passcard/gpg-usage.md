# GPG Usage

**GPG** is the underlying technology used to control access to secret material on the CSL [Passcard](./). For information on GPG read its [Wikipedia article](https://en.wikipedia.org/wiki/GNU_Privacy_Guard). The GnuPG community has excellent documentation on its [website](https://gnupg.org/documentation/howtos.html).

## High-level Overview

GPG works through a public-private key pair. Text and data can be encrypted with the public key component of the key pair \(which can be shared publicly with little restriction\). The encrypted data, however, can only be decrypted with the private key component of the key pair. GPG has other uses such as signing messages with private keys and authentication with a private key but those functionalities are not used by Passcard.

## Generating

GPG key pairs should be generated and stored on a computer you trust. Any individual with root access would be able to read the private part of your GPG key pair.

### Quick

The GnuPG community has more detailed documentation on this but, in essence, to generate your key pair, you need to run the command `gpg --gen-key`. You will be prompted to enter your name, email address, and private key passphrase.

### Manual

When generating the key, you may want to specify additional configuration options. Running `gpg --full-gen-key` It will prompt you for the type of key \(generally RSA is good\), a key size \(anything 2048 bits or longer is strong enough\), a validity time, your name, email address, a comment, and private key passphrase.

## Exporting

You can view all secret \(private\) keys you have stored on your computer with `gpg --list-secret-keys` and `gpg -K`. You can view all the public keys you have in your key ring with `gpg --list-keys` or `gpg -k`.

An example key can be found below:

```text
pub   rsa4096/0x67198197EBDE4957 2018-01-03 [SC]
      34248131BE91E92FEE033DC567198197EBDE4957
uid                   [ultimate] Theo Ouzhinski (Education) <2020fouzhins@tjhsst.edu>
sub   rsa4096/0x46FB05EE18EBA895 2018-01-03 [E]
```

In this example, `0x67198197EBDE4957` is the key ID. By default, most installations of GnuPG will default to the short format which in this case is `EBDE4957`.

GPG keys are shared publicly through GPG public key servers. Most of them automatically synchronize changes between each other. Popular key servers include `pgp.mit.edu`, `keys.gnupg.net`, and `keyserver.ubuntu.com`. A pool of many key servers can be referenced with `pool.sks-keyservers.net`.

{% hint style="info" %}
Once a key is on a public key server, it cannot be removed. IDs, signatures, and expiration dates can be added to keys on the key servers but not removed.
{% endhint %}

You can send your key \(or any other key in your keyring\) to a key server by running `gpg --keyserver <KEYSERVER URL> --send-keys <KEY ID>`.

You can export your public key in ASCII format \(to send in emails or to other people\) with `gpg --export --armor <SUBSTRING OF NAME/EMAIL OR KEY ID>`.

## Receiving

All public keys that are known to you are stored locally in your public key keyring. You can request these keys with `gpg --keyserver <KEYSERVER URL> --search-keys <SUBSTRING OF NAME/EMAIL>`. If you have a copy of someone's public key, you can import it with `gpg --import <FILE PATH>`.

{% hint style="info" %}
You should generally have all Sysadmin's public keys in your public keyring. This is useful for determining who is on a passcard \(with `./passcard.py who <SERVER>`\)
{% endhint %}

## Signing

{% hint style="info" %}
Do not sign someone's key unless you are sure that it is their key. Signing keys are at your discretion.
{% endhint %}

When you sign someone's public key, you are vouching for their identity and the key's validity. The initial purpose of signing public key's was to create a network of trust. The Sysadmins have their own network of trust. Each sysadmin should have a trust path to other sysadmins \(this may not always be the case but is general good practice\).

To sign someone's public key, run `gpg -u <SUBSTRING OF YOUR NAME/EMAIL OR YOUR KEY ID> --sign-key <SUBSTRING OF OTHER'S NAME/EMAIL>`. You will be prompted to confirm your signature. You can then send the signed key to a key server with `--send-keys` described above.

