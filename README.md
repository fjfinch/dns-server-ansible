# dns-server-ansible
Ansible playbook to deploy and configure Pi-hole & Unbound Docker containers. Tested on Ubuntu ARM64 (Raspberry Pi).

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

Go to the `ansible/` folder and change the network variables in `main.yml`.

3 - Pull required roles:
```bash
ansible-galaxy collection install -r requirements.yml
```

4 - Execute the playbook:
```bash
ansible-playbook main.yml -K
```

When the playbook executes the task *netplan*, it will give the device a static IP. If you used SSH to connect, the session will automatically terminate.

Pihole container need some extra configuration:
```bash
sudo docker exec -it pihole pihole -a -p

(in GUI) add block lists
    https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt                  - Advertising
    https://v.firebog.net/hosts/AdguardDNS.txt                                                  - Advertising
    https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt     - Tracking & Telemetry
    https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts             - Tracking & Telemetry
    https://v.firebog.net/hosts/Easyprivacy.txt                                                 - Tracking & Telemetry

sudo docker exec -it pihole pihole updateGravity
```

## Update
```bash
sudo docker pull <IMAGE>
sudo docker-compose up -d
sudo docker system prune -a
```
