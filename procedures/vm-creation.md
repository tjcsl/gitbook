# Virtual Machine Creation

1. Ensure that you have root access to a VM server\(link\).
2. Log in to the VM server as root
3. Ensure that the VM server is able to connect to Ceph by running `ceph --user libvirt -s`
4. Create a new RBD image with the command `rbd --user libvirt create --pool virtual-machines --size <SIZE> <IMGNAME>`
5. Copy the XML from an existing VM with the command `virsh dumpxml <existing> > existing.xml` and then edit it to point to the new VM image.  You also need to change the UUID to something different so that libvirt doesn't get mad that two VMs exist with the same UUID.
6. Move the changed XML file to `/qemuconfig/<vm server name>/<newvm>.xml`
7. `/qemuconfig` is a git repository, so add and commit the change once you are done
8. Run `virsh define /qemuconfig/<vm server name>/<newvm>.xml` to add the newly created VM to libvirt
9. If all went well, running `virsh start <newvm>` will start it.  You can then use the normal VNC/netboot process to get an OS installed on the vm.
10. If there was something wrong with the configuration, **DON'T EDIT THE XML FILE DIRECTLY**.  Instead, use `virsh edit <newvm>` until it works, then add and commit the file again.

