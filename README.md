# dns-server-ansible
Ansible playbook to deploy and configure Pi-hole & Unbound containers. Tested on Ubuntu (x86 and ARM).

Note 1: Pi-hole requires information about the hosts network for it's configuration (device ip / gateway ip / subnet prefix / etc.). So make sure the host has network connectivity.

Note 2: You can consider using Pi-hole for the host system -> configure *127.0.0.1* to be the hosts nameserver. This however has one drawback: if the Pi-hole fails, the host does not have a working DNS setup and might prevent you from repairing the Pi-hole installation (if you don't have much networking experience).

## Install & setup
To use this repo, a couple of tools are required:

* git (to clone the repo)
* pipx (to install ansible)
* ansible (to configure the system)

1 - Oneliner to install all above:
```bash
sudo apt update && sudo apt install -y git pipx && pipx install ansible --include-deps && pipx ensurepath && exec $SHELL
```

2 - Clone this repository:
```bash
git clone https://github.com/fjfinch/dns-server-ansible.git
```

3 - Pull required roles:
```bash
ansible-galaxy collection install -r requirements.yml
```

4 - In `ansible/`, change the variables in `main.yml` & execute the playbook:
```bash
ansible-playbook main.yml -K
```

---

Pi-hole container need some extra configuration:
```bash
Change password:
(in CLI) sudo docker exec -it pihole pihole -a -p # default password = finch

Add block lists:
(in GIU) go to the 'Adlists' page
    https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt                  - Advertising
    https://v.firebog.net/hosts/AdguardDNS.txt                                                  - Advertising
    https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt     - Tracking & Telemetry
    https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts             - Tracking & Telemetry
    https://v.firebog.net/hosts/Easyprivacy.txt                                                 - Tracking & Telemetry

Update gravity:
(in GUI) in the tool tab, go to the 'Update Gravity' page
(in CLI) sudo docker exec -it pihole pihole updateGravity
```
