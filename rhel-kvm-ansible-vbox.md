I recently shifted my homelab hypervisor from ESXi to KVM running on RHEL8.1. The core Ansible server is running on a VM running Virtualbox. This is so I can test my scripts in an isolated environment.

Versions of the specific software at the time of writing (4/22/2020) are:
- RHEL: 8.1
- Ansible 2.9.5
- Virtualbox: 6.1.6r137129
- Vagrant: 2.2.7

The specific install steps are as follows, assuming a RHEL8.1 host with libvirt:

**Run on the Ansible server VM**
`systemctl enable --now cockpit.socket`

`subscription-manager register --activation <name> --org <ID>`

`insights-client --register`

`sudo insights-client --register`

`sudo dnf -y install ansible`

**Install Virtualbox and dependencies**
`sudo dnf -y install binutils kernel-devel kernel-headers libgomp make patch gcc glibc-headers glibc-devel dkms`
`wget https://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo`
`sudo mv virtualbox.repo /etc/yum.repos.d/`
`wget -q https://www.virtualbox.org/download/oracle_vbox.asc`
`sudo rpm --import oracle_vbox.asc`
`sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`
`sudo dnf -y install binutils kernel-devel kernel-headers libgomp make patch gcc glibc-headers glibc-devel dkms`
`sudo dnf install -y VirtualBox-6.1`
`sudo usermod -aG vboxusers $USER`
`sudo /usr/lib/virtualbox/vboxdrv.sh setup`
`sudo dnf install sudo /usr/lib/virtualbox/vboxdrv.sh setup`
`sudo dnf install kernel-devel kernel-devel-4.18.0-147.el8.x86_64`
`sudo /usr/lib/virtualbox/vboxdrv.sh setup`

**Install Vagrant**
`wget https://releases.hashicorp.com/vagrant/2.2.7/vagrant_2.2.7_x86_64.rpm`
`sudo dnf -y install vagrant_2.2.7_x86_64.rpm`

**Run on the RHEL hypervisor itself - enable CPU passthrough mode to start vbox**
`virsh # list`
`virsh # stop ansible`
`virsh # edit ansible`
Replace CPU portion in the config with:
	`<cpu mode='host-passthrough' check='partial'>`
`virsh # start ansible`
