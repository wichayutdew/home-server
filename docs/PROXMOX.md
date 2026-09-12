# Proxmox

## Prerequisites

1. Bootable USB stick

- Download [Proxmox Virtual Environment](https://www.proxmox.com/en/downloads/proxmox-virtual-environment)
- Use Rufus to burn the ISO file to USB drive

## Steps

1. Boot the device with USB stick and follow GUI installation steps
### Networking guide: use Router's admin portal to help e.g. 192.168.1.1
- Hostname(FQDN): <can be anything but will be crucial once you have DNS host, use that one for ease of migration>
- IP Address (CIDR): <The IP address that proxmox will reserve to it's own, try to use one that is not within DHCP config range, usually 192.168.1.10>
- Gateway: <Router's gateway IP, usually the same one we use to access admin portal 192.168.1.1>
- DNS Server: <Router's DNS Server IP, usually the same one we use to access admin portal 192.168.1.1 or the last one in DHCP config range 192.168.1.254>

2. Run post install script [PVE Post install](https://community-scripts.org/scripts/post-pve-install)
- Disable `pve-enterprise`
- Enable and correct `ceph-enterprise` `ceph package`
- Add `pve test repository`
- Disable subscription nag
- Disable High availability (HA)
- not update/reboot

3. update packages `apt update` `pveupgrade` `apt autoremove`

4. Install tailscale
- `curl -fsSL https://tailscale.com/install.sh | sh` `tailscale up --ssh`
- `sudo tailscale serve --bg https+insecure://localhost:8006`


## Useful command
```bash
    ssh root@192.168.1.10 ## ssh into proxmox from other devices

```


## Back up

# Reference

- [Installation Guide](https://youtu.be/zngSuqCM4d8?si=9ehrTru5148Ki_Qu)
- [Connecting with tailscale](https://youtu.be/guHoZ68N3XM?si=Z-CqqEZalwuXs27l)
