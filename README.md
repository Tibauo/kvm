# KVM

Install & configure KVM to create my lab

## Requirements

```

```

## Var

```
dir_scripts: "/root/kvm"

qemu_pool:
  path: /home/pools
  name: lab_pool

image_path: /root/images

qemu_networks:
  - name: pxe
    gateway: 172.16.1.1
    netmask: 255.255.255.0
    first_ip: 172.16.1.2
    last_ip: 172.16.1.254
    path: "{{ dir_scripts }}/networks"
    address:
      - mac_address: 52:54:00:5A:C7:05
        ip: 172.16.1.10
  - name: management
    gateway: 172.17.1.1
    netmask: 255.255.255.0
    first_ip: 172.17.1.2
    last_ip: 172.17.1.254
    path: "{{ dir_scripts }}/networks"
    address:
      - mac_address: 52:54:00:70:99:2B
        ip: 172.17.1.10
      - mac_address: 52:54:00:C4:32:BF
        ip: 172.17.1.20
      - mac_address: 52:54:00:4F:51:4D
        ip: 172.17.1.30
      - mac_address: 52:54:00:29:A9:15
        ip: 172.17.1.40
      - mac_address: 52:54:00:3A:69:94
        ip: 172.17.1.50
qemu_vms:
  - name: pxe
    path_xml: "{{ dir_scripts }}/vms"
    memory: 4
    cpu: 4
    volume:
      - name: pxe
        size_gb: 40
        path_volume: /home/pools/pxe
        path_xml: "{{ dir_scripts }}/volumes"
        dev: vda
        device: disk
        bus: virtio
      - name: cdrom
        device: cdrom
        path_volume: "{{ image_path }}/rhel-8.6.iso"
        dev: sda
        path_xml: "{{ dir_scripts }}/volumes"
        bus: sata
        cdrom: yes
    network:
      - name: pxe
        mac_address: "52:54:00:5A:C7:05"
      - name: management
        mac_address: "52:54:00:70:99:2B"
```

## Playbook

```
- name: KVM
  hosts: kvm
  roles:
    - kvm
```
