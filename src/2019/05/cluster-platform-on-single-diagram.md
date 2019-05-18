# Single Diagram Cluster Platform

## States

```mermaid
graph TB
    subgraph Stage 9
        svc1
        svc2
        svc3
    end
    subgraph Stage 8
        nats
        minio
        verdaccio
    end
    subgraph Stage 7
        kmcn[kube-master-components]
        kncn[kube-node-components]
        nested[... n * system from below]
        KubeVirt
    end
    subgraph Stage 6
        kmc[kube-master-components]
        knc[kube-node-components]
        containerd
        Kube-router
        rook
        k3s
    end
    subgraph Stage 5
        ZeroTier
    end
    subgraph Stage 4
        NetworkManager
        wpa_supplicant
        SWUpdate
        Dropbear
        busybox
        ca-certificates
        systemd
        linux[Linux,SELinux]
    end
    subgraph Stage 3
        iPXE
    end
    subgraph Stage 2
        uefi[UEFI/BIOS]
    end
    subgraph Stage 1
        pro[proprietary UEFI/proprietary BIOS/proprietary Bootloader]
        USB
        NET
        PWR
    end
    subgraph Stage 0
        usbprovider[USB Provider]
        pwrprovider[PWR Provider]
        netprovider[NET Provider]
    end
    svc1 --- nats
    svc2 --- nats
    svc3 --- nats
    svc1 --- verdaccio
    svc2 --- verdaccio
    svc3 --- verdaccio
    svc3 --- minio
    nats --- kmcn
    minio --- kmcn
    verdaccio --- kmcn
    kmcn --- kncn
    kncn --- nested
    nested --- KubeVirt
    KubeVirt --- kmc
    kmc --- knc
    knc --- containerd
    knc --- Kube-router
    knc --- rook
    containerd --- k3s
    Kube-router --- k3s
    rook --- k3s
    NetworkManager --- systemd
    wpa_supplicant --- systemd
    SWUpdate --- systemd
    Dropbear --- systemd
    ZeroTier --- systemd
    k3s --- systemd
    busybox --- systemd
    ca-certificates --- systemd
    systemd --- linux
    linux --- iPXE
    iPXE --- uefi
    uefi --- pro
    pro --- USB
    pro --- NET
    pro --- PWR
    USB --- usbprovider
    NET --- netprovider
    PWR --- pwrprovider
```

## Steps

> These are just some examples. You may of course use different means of distributing the states, skip steps, ...

```mermaid
graph TB
    stage0
    stage1
    stage2
    stage3
    stage4
    stage5
    stage6
    stage7
    stage8
    stage9
    stage0 -- "Manual[USB,NET,PWR distributables]" --> stage1
    stage1 -- "PXE/Stick[proprietary distributables]" --> stage2
    stage2 -- "PXE/Stick[EFI/BIOS distributables]" --> stage3
    stage3 -- "HTTP[iPXE distributables]" --> stage4
    stage4 -- "ssh[SSH distributables in local scope]" --> stage5
    stage5 -- "ssh[SSH distributables in global scope]" --> stage6
    stage6 -- "kubectl[Kubernetes distributables in layer 1]" --> stage7
    stage7 -- "kubectl[Kubernetes distributables in layer n]" --> stage8
    stage8 -- "skaffold[kubectl[Moleculer services]]" --> stage9
```

## Means

### Imperative

```mermaid
graph TB
    subgraph local
        subgraph local energy
            lsw[local solar worker]
            solar[Solar]
            lsw --- solar
        end
        subgraph local internet
            lbw[local batman-adv worker]
            lbw --- batman-adv
            isp[Internet Service Provider]
            lbw --- isp
        end
        subgraph local network
            ldw[local dhcp worker]
            dhcp[DHCP]
            ldw --- dhcp
        end
        subgraph local bridge
            lzw[local ztbridge worker]
            ztbridge[ZeroTier Bridge]
            lzw --- ztbridge
        end
        subgraph local domain
            ldnsw[local dns worker]
            dns[DNS]
            ldnsw --- dns
        end
        subgraph local network distributable
            lptw[local pxe+tftp worker]
            pxe[PXE]
            tftp[TFTP]
            lptw --- pxe
            lptw --- tftp
        end
        subgraph local disk distributable
            luw[local usb worker]
            usb[USB]
            luw --- usb
        end
        subgraph local node
            lrw[local registry worker]
            lrw --- registry
        end
        subgraph local node distributable
            lssw[local SSH worker]
            ssh[SSH]
            lssw --- ssh
        end
    end
    subgraph global
        subgraph global switch
            gzw[global ztcontroller worker]
            ztcontroller[ZeroTier Controller]
            gzw --- ztcontroller
        end
        subgraph global network
            gnw[global ztapi worker]
            ztapi[ZeroTier API]
            gnw --- ztapi
        end
        subgraph global domain
            gdw[global dns worker]
            dns2[DNS]
            gdw --- dns2
        end
        subgraph global node
            gnow[global registry worker]
            registry2[registry]
            gnow --- registry2
        end
        subgraph global node distributable
            gsshw[global SSH worker]
            ssh2[SSH]
            gsshw --- ssh2
        end
    end
    subgraph meta
        subgraph cluster
            gndm[global node distributable manager]
            gndm --- manager
        end
        subgraph namespace
            nw[namespace worker]
            manager2[manager]
            nw --- manager2
        end
    end
```

### Declarative

```mermaid
graph LR
    n1[Node 1]
    n2[Node 2]
    subgraph Energy Node Protocol
        Solar
        usbe[USB]
        QI
    end
    subgraph Network Node Protocol
        WLAN
        LAN
        WWAN
        batman["batman-adv(-bridge)"]
        zerotier["ZeroTier(-bridge)"]
    end
    subgraph Boot Node Protocol
        PXE
        TFTP
        USB
        IPFS
    end
    subgraph Cluster Node Protocol
        k3s
    end
    subgraph Namespace Node Protocol
        crd[Custom Resource Definition]
    end
    n1 --- Solar
    n1 --- usbe
    n1 --- QI
    Solar --- n2
    usbe --- n2
    QI --- n2
    n1 --- WLAN
    n1 --- LAN
    n1 --- WWAN
    n1 --- batman
    n1 --- zerotier
    WLAN --- n2
    LAN --- n2
    WWAN --- n2
    batman --- n2
    zerotier --- n2
    n1 --- PXE
    n1 --- TFTP
    n1 --- USB
    n1 --- IPFS
    PXE --- n2
    TFTP --- n2
    USB --- n2
    IPFS --- n2
    n1 --- k3s
    k3s --- n2
    n1 --- crd
    crd --- n2
```
