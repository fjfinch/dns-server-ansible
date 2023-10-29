# dns-server-ansible
Ansible playbook to deploy and configure Pi-hole & Unbound containers. Tested on Ubuntu (x86 and ARM).

Note: Make sure your device already obtained an IP address through DHCP. Internet access is required to install tools, and the network variables are used for the Pi-hole container and the netplan task.

## Install & setup
To use Ansible, a couple of tools are required:

* git (to clone this repo)
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

4 - Change the variables in `main.yml` & execute the playbook:
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
