MLS_3
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

# SW0 Etherchannel
int range g1/0/2 - g1/0/3
channel-group 1 mode active
int port-channel 1
switchport mode trunk
switchport trunk encapsulation dot1q

# SW1 Etherchannel
int range g1/0/1 - g1/0/4
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


# SW0
```bash
vtp mode client
vtp domain UOC
vtp password cisco

int g0/1
switchport mode trunk
int g0/2
switchport mode trunk

# Etherchannel
int range g0/1 - g0/2
channel-group 1 mode active
int port-channel 1
switchport mode trunk


int fa0/24
switchport mode access
switchport access vlan 100
```

# SW1

```bash
vtp mode client
vtp domain UOC
vtp password cisco


int g0/1
switchport mode trunk
int g0/2
switchport mode trunk

# Etherchannel
int range g0/1 - g0/2
channel-group 2 mode active
int port-channel 2
switchport mode trunk


int fa0/24
switchport mode access
switchport access vlan 150
```

# PC0

```bash
172.16.100.10
255.255.255.0
172.16.100.1
```
# PC1

```bash
172.16.150.10
255.255.255.0
172.16.150.1
```


# USA-WAN2
int s1/0
ip address 10.10.10.2 255.255.255.248


# RT-LONDON

int s1/0
ip address 10.10.10.3 255.255.255.248
