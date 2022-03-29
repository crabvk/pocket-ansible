# Pocket node setup/check automation with Ansible<!-- omit in toc -->

- [Prerequisites](#prerequisites)
  - [Basic Ansible terminology](#basic-ansible-terminology)
  - [Control node requirements](#control-node-requirements)
  - [Control node dependencies](#control-node-dependencies)
  - [Managed node dependencies](#managed-node-dependencies)
  - [Domain name and DNS record](#domain-name-and-dns-record)
  - [SSL certificate](#ssl-certificate)
- [These playbooks do](#these-playbooks-do)
  - [setup.yaml](#setupyaml)
  - [check.yaml](#checkyaml)
- [These playbooks do not](#these-playbooks-do-not)
- [Setting up your Pocket node](#setting-up-your-pocket-node)
- [Check the correctness of your setup](#check-the-correctness-of-your-setup)
- [Contributions](#contributions)

## Prerequisites

### Basic Ansible terminology

* Control node - the machine that runs [Ansible](https://docs.ansible.com/), e.g. your laptop or desktop computer
* Managed node - the target machine, e.g. your server
* Playbook - a blueprint of automation tasks ([more info](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook))
* Inventory - a file that defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate

### Control node requirements

* UNIX system, e.g. Arch Linux, Ubuntu, Debian, CentOS, Red Hat, macOS, any of the BSDs, etc.
* Windows is not supported, but you can use [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install)

### Control node dependencies

* Ansible 2.9 or later:

    - [ansible-core](https://archlinux.org/packages/community/any/ansible-core/) package on Arch Linux
    - [ansible](https://packages.ubuntu.com/focal/ansible) package on Ubuntu 20.04

* python-dnspython
* python-jmespath

### Managed node dependencies

* Python 3.5 or later

### Domain name and DNS record

Before continuing and running any of the playbooks you should have a domain name and DNS record type A pointing to your server's IPv4 address.
If you don't already have it, read [Your Pocket node domain name and DNS record](docs/dns-record.md).

### SSL certificate

If you don't already have an SSL certificate for your domain, here are instructions on how to [Obtain SSL certificate with certbot](docs/certbot-certificate.md).

## These playbooks do

### setup.yaml

* Install and configure the firewall (nftables)
* Check Pocket binary installed on the target system
* Check installed Pocket version is the latest
* Create Pocket user, group, and home directory
* Create Pocket systemd service
* Install and configure web server (Nginx)

### check.yaml

* Check your Pocket node is publicly available
* Check you have the correct mainnet genesis.json config file
* Check your Pocket node is fully synced
* Check your Pocket node relays requests to the chains successfully

## These playbooks do not

* Build Pocket and install the binary

    - Follow [instructions in the official guide](https://docs.pokt.network/home/paths/node-runner#software)
    - Copy Pocket binary to /usr/local/bin directory or if it's not in the PATH set `pocket_path` variable in *host_vars/&lt;host&gt;.yaml*

* Configure your SSH server

    - You should already have your SSH server configured with access by SSH key
    - On the control node add your SSH key to the ssh-agent, make sure you can connect to the server **without a password**
    - If using not default SSH port, set `sshd_port` variable in *host_vars/&lt;host&gt;.yaml*

## Setting up your Pocket node

Before continuing, you should have followed the steps in the [These playbooks do not](#these-playbooks-do-not) section above, got [your domain name with a DNS record](docs/dns-record.md), and have an [SSL certificate on the target node](docs/certbot-certificate.md).

1. Clone this repository on your control node:

```shell
git clone https://github.com/crabvk/pocket-ansible.git
cd pocket-ansible
```

2. Create file *hosts* and write a list of your servers (probably you have only one for now). Use the same `Host` names as in your *~/.ssh/config* file.

3. Copy *host_vars/host.example.yaml* to *host_vars/&lt;host&gt;.yaml*, read comments and set the variables accordingly.

4. Setup your Pocket node:

```shell
ansible-playbook -i hosts setup.yaml
```

5. Follow the steps in [Deploy Your Validator & Full Nodes](https://docs.pokt.network/home/paths/node-runner#deploy-your-validator-and-full-nodes).

> NOTE: To execute `pocket` commands open the shell as user pocket:

```shell
sudo -u pocket bash
```

> WARNING: Don't forget to check your Pocket node's config file at *~/.pocket/config/config.json*  
> It is auto-generated within the "Create an account" step.

## Check the correctness of your setup

After your Pocket node and all the chains are fully synced, start pocket in test mode:

```shell
pocket start --simulateRelay
```

and check the correctness of setup with:

```shell
ansible-playbook -i hosts check.yaml
```

## Contributions

Contributions are very welcome.
Feel free to create an issue if you found a bug, want to request a feature, or have a question.  
You can also contact me on [Telegram](https://t.me/crabvk) or [Discord](https://discordapp.com/users/801675841397325834).
