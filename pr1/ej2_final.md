## USA-CORE

```bash

ip routing

int g1/0/1
no switchport trunk encapsulation dot1q
no switchport mode trunk


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
no switchport trunk encapsulation dot1q
no switchport mode trunk


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

spanning-tree mode rapid-pvst

int fa0/1
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

int fa0/2
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



# USA-WAN2

```bash
int fa0/0
no shut
ip address 192.168.10.2  255.255.255.0
```

# USA-WAN1

```bash
int gi0/1
no shut
ip address 10.0.0.1      255.255.255.252
int gi0/0
no shut
ip address 192.168.10.1  255.255.255.0

```


## RT-DATACENTER
```bash
int fa0/1 
ip address 10.0.0.2    255.255.255.252
int fa0/0 
ip address 192.168.5.1 255.255.255.0
int lo10  
ip address 47.3.5.10   255.255.255.255

```

## RT-LONDON

```bash
int fa0/1.47
encapsulation dot1q 47
ip address 10.47.59.1 255.255.255.0

int fa0/1.48
encapsulation dot1q 48
ip address 10.48.10.1 255.255.255.0

```

## FILE-SERVER

```bash
int fa0 10.47.59.10 255.255.255.0
```
## DNS

```bash
int fa0 10.47.59.20 255.255.255.0
```
## FTP

```bash
int fa0 10.47.59.30 255.255.255.0
```

# Static routing between routers

```bash
ip route 192.168.10.0 255.255.255.0 192.168.

int g1/0/6
port link-type 
```

## Implementar HSRP
Vamos a implementar HSRP, con USA-WAN2 como prioridad

### USA-WAN1
```bash
int gi0/0
standby version 2
standby 1 ip 192.168.10.254
standby 1 priority 50
```
### USA-WAN2
```bash
int fa0/0
standby version 2
standby 1 ip 192.168.10.254
standby 1 priority 100
```



## Implementar OSPF

USA-WAN2

```bash
int lo0
ip address 172.16.0.1 255.255.255.255
router ospf 10
network 192.168.10.2 0.0.0.255 area 0
network 172.16.0.1 0.0.0.0 area 0
network 10.10.10.2 0.0.0.7 area 0

```
RT-DATACENTER
```bash
int lo0
ip address 172.16.1.1 255.255.255.255
router ospf 10
network 192.168.5.1 0.0.0.255 area 0
network 172.16.1.1 0.0.0.0 area 0
network 10.10.10.1 0.0.0.7 area 0

```
RT-LONDON
```bash
int lo0
ip address 172.16.2.1 255.255.255.255
router ospf 10
network 10.47.59.1 0.0.0.255 area 0
network 172.16.2.1 0.0.0.0 area 0
network 10.10.10.3 0.0.0.7 area 0

```