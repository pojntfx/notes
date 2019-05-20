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

> This is just an example. You may of course use different means of distributing the states, skip steps, ...

```mermaid
graph TB
    state0[State 0]
    subgraph Step 0
        humanmanager[Human manager]
        humanworker[Human worker]
        hand[Hand]
        handdistributable[Hand distributable with USB,NET,PWR]
        humanmanager --> humanworker
        humanworker --> hand
        hand --> handdistributable
    end
    handdistributable --> state0
    state1[State 1]
    state0 --> state1
    subgraph Step 1
        dhcpmanager[DHCP manager]
        dhcpworker[DHCP worker]
        dhcp[DHCP]
        dnsmanager[DNS manager]
        dnsworker[DNS worker]
        dns[DNS]
        pxemanager[PXE manager]
        pxeworker[PXE worker]
        pxe[PXE]
        tftpmanager[TFTP manager]
        tftpworker[TFTP worker]
        tftp[TFTP]
        proprietarylocalnetworkdistributable[Proprietary network distributable in local scope with BIOS/UEFI]
        humanmanager1[Human manager]
        humanworker1[Human worker]
        stick[Stick]
        proprietarymediadistributable[Proprietary media distributable]
        dhcpmanager --> dhcpworker
        dhcpworker --> dhcp
        dnsmanager --> dnsworker
        dnsworker --> dns
        dhcp --> dns
        pxemanager --> pxeworker
        pxeworker --> pxe
        dns --> pxe
        tftpmanager --> tftpworker
        tftpworker --> tftp
        pxe --> tftp
        tftp --> proprietarylocalnetworkdistributable
        humanmanager1 --> humanworker1
        humanworker1 --> stick
        stick --> proprietarymediadistributable
    end
    proprietarylocalnetworkdistributable --> state1
    proprietarymediadistributable --> state1
    state2[State 2]
    state1 --> state2
    subgraph Step 2
        tftp --> uefibioslocalnetworkdistributable
        uefibioslocalnetworkdistributable[UEFI/BIOS network distributable in local scope with iPXE]
        uefibiosmediadistributable[UEFI/BIOS media distributable]
        stick --> uefibiosmediadistributable
    end
    uefibioslocalnetworkdistributable --> state2
    uefibiosmediadistributable --> state2
    state3[State 3]
    state2 --> state3
    subgraph Step 3
        scriptrepo[script repository]
        ipxescript[iPXE script]
        http31[HTTP]
        kernelrepo[Kernel repository]
        kernel[Kernel]
        http32[HTTP]
        initramfsrepo[Inital RAM filesystem repo]
        initramfs[Inital RAM filesystem]
        http33[HTTP]
        osrepomirrorrepo[OS repo mirror repository]
        osrepomirror[OS repo mirror]
        http34[HTTP]
        kickstart[Kickstart]
        http35[HTTP]
        prescript[pre-reboot script]
        http36[HTTP]
        postscript[post-reboot script]
        http37[HTTP]
        sshkeymanager[SSH key manager]
        sshkeyworker[SSH key worker]
        http38[HTTP]
        sshkey[SSH key]
        http39[HTTP]
        globalnetworkdistributable[network distributable in global scope with generic GNU/Linux distro]
        scriptrepo --> ipxescript
        ipxescript --> http31
        http31 --> globalnetworkdistributable
        kernelrepo --> kernel
        kernel --> http32
        http32 --> globalnetworkdistributable
        initramfsrepo --> initramfs
        initramfs --> http33
        http33 --> globalnetworkdistributable
        osrepomirrorrepo --> osrepomirror
        osrepomirror --> http34
        http34 --> globalnetworkdistributable
        scriptrepo --> kickstart
        kickstart --> http35
        http35 --> globalnetworkdistributable
        scriptrepo --> prescript
        prescript --> http36
        http36 --> kickstart
        scriptrepo --> http37
        http37 --> postscript
        postscript --> http38
        http38 --> kickstart
        sshkeymanager --> sshkeyworker
        sshkeyworker --> sshkey
        sshkey --> http39
        http39 --> postscript
    end
    globalnetworkdistributable --> state3
    state4[State 4]
    state3 --> state4
    subgraph Step 4
        sshdistributablemanager[Remote shell distributable manager]
        localsshdistributableworker[Remote shell distributable worker in local scope]
        ssh4[SSH]
        localregistry[local registry]
        sshdistributable4[remote shell distributable with ZeroTier VPN]
        sshdistributablemanager --> localsshdistributableworker
        localsshdistributableworker --> ssh4
        ssh4 --> sshdistributable4
        localregistry --> sshdistributable4
    end
    sshdistributable4 --> state4
    state5[State 5]
    state4 --> state5
    subgraph Step 5
        globalsshdistributableworker[Remote shell distributable worker in global scope]
        ssh5[SSH]
        globalregistry[global registry]
        sshdistributable5[remote shell distributable with k3s]
        sshdistributablemanager --> globalsshdistributableworker
        globalsshdistributableworker --> ssh5
        ssh5 --> sshdistributable5
        globalregistry --> sshdistributable5
    end
    sshdistributable5 --> state5
    state6[State 6]
    state5 --> state6
    subgraph Step 6
        clusterdistributablemanager[cluster distributable manager]
        clusterdistributableworker[cluster distributable worker]
        kubectl[kubectl]
        clusterdistributablelayer1[cluster distributable in layer 1 with KubeVirt]
        clusterdistributablemanager --> clusterdistributableworker
        clusterdistributableworker --> kubectl
        kubectl --> clusterdistributablelayer1
    end
    clusterdistributablelayer1 --> state6
    state7[State 7]
    state6 --> state7
    subgraph Step 7
        kubectl --> clusterdistributablelayern
        clusterdistributablelayern[cluster distributable in layer n with dependencies]
    end
    clusterdistributablelayern --> state7
    state8[State 8]
    state7 --> state8
    subgraph Step 8
        skaffold[Skaffold]
        kubectl8[kubectl]
        servicemeshdistributable[service mesh distributable with services]
        skaffold --> kubectl8
        kubectl8 --> servicemeshdistributable
    end
    servicemeshdistributable --> state8
    state9[State 9]
    state8 --> state9
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
