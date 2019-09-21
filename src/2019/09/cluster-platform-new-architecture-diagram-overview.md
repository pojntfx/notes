# Cluster Platform New Architecture Overview

Note: No S3 buckets, databases, network interfaces etc. are shown because they are to be set by the client as a parameter or an environment variable!

## Builders

```mermaid
graph LR
    subgraph Builders
        client

        subgraph Interfaces
            ipxe-builder-interface[ipxes]
            grub-builder-interface[grubs]
            syslinux-builder-interface[syslinuxes]
            key-builder-interface[key]
        end

        subgraph Managers
            ipxe-builder-manager
            grub-builder-manager
            syslinux-builder-manager
        end

        subgraph Workers
            ipxe-builder-worker
            grub-builder-worker
            syslinux-builder-worker
            key-builder-worker
        end

        client --- ipxe-builder-interface
        client --- grub-builder-interface
        client --- syslinux-builder-interface
        client --- key-builder-interface

        ipxe-builder-interface --- ipxe-builder-manager
        ipxe-builder-manager --- ipxe-builder-worker

        grub-builder-interface --- grub-builder-manager
        grub-builder-manager --- grub-builder-worker

        syslinux-builder-interface --- syslinux-builder-manager
        syslinux-builder-manager --- syslinux-builder-worker

        key-builder-interface --- key-builder-worker
    end
```

## Packagers

```mermaid
graph LR
    subgraph Packagers
        client

        subgraph Interfaces
            iso-packager-interface[isos]
            img-packager-interface[imgs]
            pxe-packager-interface[pxes]
        end

        subgraph Managers
            iso-packager-manager
            img-packager-manager
            pxe-packager-manager
        end

        subgraph Workers
            iso-packager-worker
            img-packager-worker
            pxe-packager-worker
        end

        client --- iso-packager-interface
        client --- img-packager-interface
        client --- pxe-packager-interface

        iso-packager-interface --- iso-packager-manager
        iso-packager-manager --- iso-packager-worker

        img-packager-interface --- img-packager-manager
        img-packager-manager --- img-packager-worker

        pxe-packager-interface --- pxe-packager-manager
        pxe-packager-manager --- pxe-packager-worker
    end
```

## Deployers

```mermaid
graph LR
    subgraph Deployers
        client

        subgraph Interfaces
            dhcp-deployer-interface[dhcp-deployment]
            dns-deployer-interface[dns-deployment]
            usb-deployer-interface[usb-deployments]
            sd-deployer-interface[sd-deployments]
            tftp-deployer-interface[tftp-deployment]
            ssh-deployer-interface[ssh-deployments]
            pnc-deployer-interface[pnc-deployments]
            wlc-deployer-interface[wlc-deployments]
        end

        subgraph Managers
            usb-deployer-manager
            sd-deployer-manager
            ssh-deployer-manager
            pnc-deployer-manager
            wlc-deployer-manager
        end

        subgraph Workers
            dhcp-deployer-worker
            dns-deployer-worker
            tftp-deployer-worker
            usb-deployer-worker
            sd-deployer-worker
            ssh-deployer-worker
            pnc-deployer-worker
            wlc-deployer-worker
        end

        client --- dhcp-deployer-interface
        client --- dns-deployer-interface
        client --- usb-deployer-interface
        client --- sd-deployer-interface
        client --- tftp-deployer-interface
        client --- ssh-deployer-interface
        client --- pnc-deployer-interface
        client --- wlc-deployer-interface

        dhcp-deployer-interface --- dhcp-deployer-worker

        dns-deployer-interface --- dns-deployer-worker

        tftp-deployer-interface --- tftp-deployer-worker

        usb-deployer-interface --- usb-deployer-manager
        usb-deployer-manager --- usb-deployer-worker

        sd-deployer-interface --- sd-deployer-manager
        sd-deployer-manager --- sd-deployer-worker

        ssh-deployer-interface --- ssh-deployer-manager
        ssh-deployer-manager --- ssh-deployer-worker

        pnc-deployer-interface --- pnc-deployer-manager
        pnc-deployer-manager --- pnc-deployer-worker

        wlc-deployer-interface --- wlc-deployer-manager
        wlc-deployer-manager --- wlc-deployer-worker
    end
```

## Registries

```mermaid
graph LR
    subgraph Registries
        client

        subgraph Interfaces
            script-registry-interface[scripts]
            key-registry-interface[keys]
        end

        subgraph Managers
            script-registry-manager
            key-registry-manager
        end

        client --- script-registry-interface
        client --- key-registry-interface

        script-registry-interface --- script-registry-manager
        key-registry-interface --- key-registry-manager
    end
```
