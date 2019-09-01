# SSH Passwordless Login

**SSH Passwordless Login** is set up on all CSL [workstations](../../../services/workstations.md) and the [remote access servers](../../../services/remote-access/) in order to seamlessly SSH into different machines without having to type in you password every time. This is achieved using GSSAPI and [Kerberos](../kerberos.md).

## Common Problems

Here are some common problems, and how to fix them.

### Disconnecting: Protocol error: didn't expect packet type 34

This is usually the result of an error in GSSAPI. Run ssh again with the "-v" option. If the message

```text
debug1: Delegating credentials
debug1: Received Error
debug1: GSSAPI Error:
Miscellaneous failure
Wrong principal in request
```

appears, then it means you are probably trying to connect to a service IP address. "Wrong principal in request" means \(I think\) that the server tried to contact a Kerberos KDC from a different IP than the IP you tried to access. GSSAPI does not seem to work with service IPs.

### No AFS tokens, but successful login

If you have Kerberos tickets \(run `klist` to see\) but no AFS tokens, then sshd might be running with UsePrivilegeSeparation turned on. Set it to "no" in `/etc/ssh/sshd_config`, and restart the ssh server.

### Prompted for a password

For some reason GSSAPI is either not being used or is failing. Run `ssh` with the `-vvv` option, and look around for output like this \(right after the "banner" displays\):

```text
debug3: preferred gssapi-with-mic,publickey,gssapi,keyboard-interactive,password
debug3: authmethod_lookup gssapi-with-mic
debug3: remaining preferred: publickey,gssapi,keyboard-interactive,password
debug3: authmethod_is_enabled gssapi-with-mic
debug1: Next authentication method: gssapi-with-mic
```

This represents that ssh is trying to use the `gssapi-with-mic` authentication method. If you don't see anything like this, check the local ssh\_config and the client's personal `.ssh/config` to see if they are changing their `PreferredAuthentications` to something without GSSAPI.

If it is trying to use GSSAPI, then look for other error messages. You can also try looking for errors on the KDC to see if there's an error with the tickets \(such as if they are not forwardable for some reason\). Also check to see if the user's tickets are forwardable by running `klist -f`  and look for the `F` flag by the user's tickets.

One known error is if the user logs into the system with capital letters, they will be able to log in, but GSSAPI will fail.

### GSSAPI not offered

If you see something similar to:

```text
debug1: Authentications that can continue: publickey,keyboard-interactive
```

then the server may not be configured to use GSSAPI. Check that `/etc/ssh/sshd_config` has the following:

```text
GSSAPIAuthentication yes
GSSAPICleanupCredentials yes
```

The following line may cause sshd to fail and fall back to older configuration, as the option is not recognized by newer ssh servers:

```text
GSSAPIStrictAcceptorCheck no
```

If this is happening, make sure to remove this line.

