# Setup

{% hint style="info" %}
You can also find setup instruction within the [Ceph documentation](https://docs.ceph.com)
{% endhint %}

{% hint style="warning" %}
For our G10 servers, do not use Ubuntu Server 16.04.  Instead, use Ubuntu Server 18.04.
{% endhint %}

## Initial Installation

### Prerequisites

Install ceph-deploy:

```bash
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-{ceph-stable-release}/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
sudo apt update
sudo apt install ceph-deploy
```

where `{ceph-stable-release}` is the codename for the release you want



