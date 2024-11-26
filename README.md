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
sudo apt update && sudo apt install -y git pipx && pipx install ansible --include-deps && . ~/.profile
```

2 - Clone this repository:
```bash
git clone https://github.com/fjfinch/dns-server-ansible.git
```

3 - Within `ansible/` - pull the required roles:
```bash
ansible-galaxy collection install -r requirements.yml
```

4 - Within `ansible/` - change the variables in `main.yml` & execute the playbook:
```bash
ansible-playbook main.yml -K
```

---

Pi-hole container need some extra configuration:
```bash
Add blocklists. Go to the 'Lists' page, and paste the URLs below all in once in the 'Address' box:
  https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt
  https://v.firebog.net/hosts/AdguardDNS.txt
  https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt
  https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts
  https://v.firebog.net/hosts/Easyprivacy.txt

The lists are, in order:
  Advertising
  Advertising
  Tracking & Telemetry
  Tracking & Telemetry
  Tracking & Telemetry

Update gravity. In the 'Tools' tab, go to the 'Update Gravity' page. Or run 'sudo docker exec -it pihole pihole updateGravity' in the CLI.
```
