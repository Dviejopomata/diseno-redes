## USA-CORE

```bash

ip routing

int g1/0/1
switchport trunk encapsulation dot1q
switchport mode trunk

int g1/0/2
switchport trunk encapsulation dot1q
switchport mode trunk

int g1/0/3
switchport trunk encapsulation dot1q
switchport mode trunk

int g1/0/4
switchport trunk encapsulation dot1q
switchport mode trunk

int g1/0/5
switchport trunk encapsulation dot1q
switchport mode trunk

int g1/0/6
switchport trunk encapsulation dot1q
switchport mode trunk

# SW-GF Etherchannel
int range g1/0/3,g1/0/5
channel-group 1 mode active
int port-channel 1
switchport mode trunk
switchport trunk encapsulation dot1q

# SW-FF Etherchannel
int range g1/0/2,g1/0/4
channel-group 2 mode active
int port-channel 2
switchport mode trunk
switchport trunk encapsulation dot1q

vlan 100
name ADMINISTRACION
vlan 150
name DESIGN

int vlan 100
ip address 172.16.100.1 255.255.255.0
no sh

int vlan 150
ip address 172.16.150.1 255.255.255.0
no sh


vtp mode server
vtp domain UOC
vtp password cisco

```


## SW-GF

```bash
vtp mode client
vtp domain UOC
vtp password cisco



int fa0/1
switchport mode trunk
int fa0/2
switchport mode trunk
int fa0/3
switchport mode trunk

# Etherchannel
int range fa0/2,fa0/3
channel-group 1 mode active
int port-channel 1
switchport mode trunk

spanning-tree vlan 100,150 root secondary

```

## SW-FF

```bash
vtp mode client
vtp domain UOC
vtp password cisco

int fa0/1
switchport mode trunk
int fa0/2
switchport mode trunk

int fa0/3
switchport mode trunk

int fa0/4
switchport mode trunk

int fa0/5
switchport mode trunk

# Etherchannel
int range fa0/1,fa0/5
channel-group 2 mode active
int port-channel 2
switchport mode trunk

spanning-tree vlan 100,150 root primary

```
## SW-USERS1

```bash
vtp mode client
vtp domain UOC
vtp password cisco

int fa0/1
switchport mode trunk

int fa0/2
switchport mode access
switchport access vlan 150

```

## SW-USERS2

```bash
vtp mode client
vtp domain UOC
vtp password cisco

int fa0/1
switchport mode trunk

int fa0/2
switchport mode access
switchport access vlan 100

```

## PC0
172.16.100.10
255.255.255.0
172.16.100.1

## PC1
172.16.150.10
255.255.255.0
172.16.150.1
