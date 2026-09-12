# Alpine Linux VM

## Installation

1. Download `virtual` version `alpine-virt-....-x86-64.iso` of the OS from [Source](https://alpinelinux.org/downloads)
2. Upload the ISO file back into Proxmox's local storage
3. Create a VM inside Proxmox
4. Start the VM and then run `setup-alpine`
5. unmount CD/DVD from VM and run `reboot`
6. run `apk update`
7. Connect to Proxmox's montoring using `QEMU Guest Agent`
8. run `apk add tailscale docker docker-cli-compose`

## Useful command

    ```bash
        rc-update add <service> default ## Add service to auto run on boot
        rc-service <service> start      ## Start that certain service right away
        rc-status                       ## List all OpenRC setup
    ```

# Reference

- [Step-by-step guide](https://vormox.com/blog/how-to-create-an-alpine-linux-vm-in-proxmox-ve-step-by-step-guide)
