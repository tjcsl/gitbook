# Netbox

Netbox is an [open-source](https://github.com/netbox-community/netbox/) Django application that is designed to help manage and document networks and the devices on those networks. In the CSL we use it to document each machine and VM along with their network configurations.

## Installation 

Netbox consists of 4 parts:

1. PostgreSQL database
2. Redis
3. Netbox Django Application
4. HTTP Proxy and Daemon

Basic installation instructions are [here](https://netbox.readthedocs.io/en/stable/installation/).   
The `netbox` Ansible playbook properly installs and configures everything as well.

### Upgrading Netbox

Netbox graciously created an upgrade script that automatically upgrades the current installation.  
Just `ssh` into `netbox` and run the following:

```bash
/opt/netbox/upgrade.sh
```

Once the script finishes, make sure to restart the `supervisor`daemons:

```text
supervisorctl restart netbox
supervisorctl restart netbox-rqworker
```

## Using Netbox

You can access the netbox web page at [https://netbox.tjhsst.edu](https://netbox.tjhsst.edu), but you MUST be VPN'ed into the CSL in order to access the website.

### Creating User Accounts

In order to edit any content on netbox, you must have an user account on netbox. Unfortunately, netbox does not support generic oauth at the moment, so users are authenticated against the netbox's `postgres` database through the django authentication framework. You can create user accounts in two ways:

1. Through the admin interface at [https://netbox.tjhsst.edu/admin](https://netbox.tjhsst.edu/admin) , \(requires another sysadmin to log in with their admin account\)
2. Through `manage.py createsuperuser` directly on the `netbox` VM

### Interacting with Netbox

Using netbox is pretty straightforward; navigating through the web page is pretty intuitive. More specific documentation on netbox is available [here](https://netbox.readthedocs.io/en/stable/core-functionality/ipam/), but here are a few basics:

* The highest order of organization is a `region`, it represents the physical area where the network is \(TJHSST , CSL\). 
* Next there are `sites`, they represent the specific area in the `region` that divides the network \(Machine Room, Room 200, Room 200C, etc.\). 
* Each `site` can have `racks`, which represent a physical server rack \(only used in the Machine Room `site`\)
* `devices` are the next organizational unit. `devices` are NOT VMs. `devices` can belong to a `rack`\(in the case of servers\), or they can be independent \(in the case of workstations\).
* `devices` have `interfaces` which can be assigned an `ip`. 
  * Creating `interfaces` and `ips` is not obvious, though.
    1.  First you have to create a `device`
    2. then on that `device`'s overview page, click the "Add Components" dropdown and select "Interfaces".
    3.  Then fill out the information for that `interface`. 
    4. Finally, when the `interface` is created, scroll to the bottom of the `device`'s overview page and click the green "+" box and add an `ip` there.

It is **much** easier to mass create devices through netbox's API, available at [https://netbox.tjhsst.edu/api](https://netbox.tjhsst.edu/api).  
In order to use the API, however, you must first create an API token [here](https://netbox.tjhsst.edu/user/api-tokens/).  


Some \(messy\) scripts that were used to bulk create the `borg` devices and workstations are available at [https://gitlab.tjhsst.edu/sysadmins/docs/netbox-scripts.git](https://gitlab.tjhsst.edu/sysadmins/docs/netbox-scripts.git)

