# dns-server-ansible
Ansible playbook for deploying and configuring a Pi-hole (& Unbound) container. Tested on Ubuntu (x86 and ARM).

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

# PIPX from APT is often outdated. PIPX from PIP:
sudo apt update &&\
sudo apt install -y git python3-pip python3-venv &&\
pip install --break-system-packages --user pipx &&\
python3 -m pipx ensurepath &&\
. ~/.profile &&\
pipx install ansible --include-deps
```

2 - Clone this repository:
```bash
git clone https://github.com/fjfinch/dns-server-ansible.git
```

3 - Pull the required roles:
```bash
ansible-galaxy collection install -r requirements.yml
```

4 - Execute the playbook:
> Note: change the *password* variable in `main.yml`
```bash
ansible-playbook main.yml -K
```

---

Extra blocklists for Pi-hole are optional. Go to the 'Lists' page, and paste the URLs below all in once in the 'Address' box:
```bash
https://v.firebog.net/hosts/AdguardDNS.txt
https://v.firebog.net/hosts/Easyprivacy.txt
https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/light.txt
https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/tif.txt
https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt
https://urlhaus.abuse.ch/downloads/hostfile/

The lists are, in order:
Advertising
Tracking & Telemetry
All
Malicious
Advertising
Malicious
```

Update gravity. In the 'Tools' tab, go to the 'Update Gravity' page. Or run 'sudo docker exec -it pihole pihole updateGravity' in the CLI.
