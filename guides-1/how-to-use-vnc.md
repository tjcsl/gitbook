# How to use VNC

This is useful for setting up/debugging [Virtual Machines] when they don't have ssh.

You'll first want to install a VNC client on your computer. I recommend TigerVNC. To install TigerVNC on Ubuntu, run the following command:

```
sudo apt install tigervnc
```

Then, ssh into the machine running the virtual machine and run the following command as root:

```
virsh vncdisplay <vm_name>
```

You will get an output similar to `127.0.0.1:1` add 5900 to the last number to get the port the VNC server is listening on. In this case, the port would be 5901.

Then, use ssh tunneling to forward that port to your local computer. If you are not on the CSL network, you will need to use [Openvpn] or tunnel twice through one of that [RAS]es. For example, if the virtual machine was on [Antipodes]:

```
[you@yourcomputer:~]$ ssh -L 5901:localhost:5901 ras1.tjhsst.edu
[2019yyou@ras1:~]$ ssh -L 5901:localhost:5901 antipodes.csl.tjhsst.edu
```

Finally, open up your VNC client and connect to `127.0.0.1:5901` (customize port to fit your case). That's it!

Look at your VNC's manual to learn how to use it better. Mostly, though, keyboard and mouse input should pass right through as long as the VNC window is focused.

If you press Ctrl+Alt+Delete while connected via tty, you will reboot the machine.

[Virtual Machines]: ../technologies/virtualization-stack/README.md
[Openvpn]: ../technologies/networking/openvpn.md
[RAS]: ../services/remote-access/README.md
[Antipodes]: ../machines/vm-servers/antipodes.md
