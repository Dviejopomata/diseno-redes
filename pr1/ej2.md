## Sedes

### USA

USA-WAN1

Gi0/1 10.0.0.1      255.255.255.252
Gi0/0 192.168.10.1  255.255.255.0

```
int gi0/1
no shut
ip address 10.0.0.1      255.255.255.252
int gi0/0
no shut
ip address 192.168.10.1  255.255.255.0

```

USA-WAN2

Se0/0/0/0
Fa0/0     192.168.10.2 255.255.255.0


```
int fa0/0
no shut
ip address 192.168.10.2  255.255.255.0

```

USA-CORE
VLAN 100 172.16.100.1 255.255.255.0 VLAN ADMINISTRACION
VLAN 150 172.16.150.1 255.255.255.0 VLAN DESIGN

```
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

ip route 0.0.0.0 0.0.0.0 

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

```

SW-GF
```
vlan 100
name ADMINISTRACION
vlan 150
name DESIGN

int fa0/1
switchport mode trunk

int fa0/2
switchport mode trunk

int fa0/3
switchport mode trunk

```

SW-FF
```bash
vlan 100
name ADMINISTRACION
vlan 150
name DESIGN
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


# SW-USERS2
int fa0/3
switchport mode trunk
int fa0/2
switchport mode access
switchport access vlan 100

# SW-USERS1
int fa0/4
switchport mode trunk
int fa0/2
switchport mode access
switchport access vlan 150

```

PC0
int fa0 172.16.100.2 255.255.255.0 172.16.100.1

PC1
int fa0 172.16.150.2 255.255.255.0 172.16.150.1


SW-USERS1
int fa0/2
switchport mode trunk

SW-USERS2
int fa0/2
switchport mode trunk

### DATACENTER

RT-DATACENTER

Fa0/1     10.0.0.2      255.255.255.252
Fa0/0     192.168.5.1   255.255.255.0
Lo10      47.3.5.10     255.255.255.255

```
int fa0/1 
ip address 10.0.0.2    255.255.255.252
int fa0/0 
ip address 192.168.5.1 255.255.255.0
int lo10  
ip address 47.3.5.10   255.255.255.255
```

WWW

Fa0/0     192.168.5.10  255.255.255.0

```
int fa0/0 192.168.5.10  255.255.255.0
```

### LONDON

RT-LONDON

Se0/0/0/0
Fa0/1.47(VLAN 47)    10.47.59.1    255.255.255.0     VLAN SERVERS
Fa0/1.48(VLAN 48)    10.48.10.1    255.255.255.0   VLAN  IT

```
int fa0/1.47
encapsulation dot1q 47
ip address 10.47.59.1 255.255.255.0

int fa0/1.48
encapsulation dot1q 48
ip address 10.48.10.1 255.255.255.0

```



FILE SERVER

Fa0/0             10.47.59.10   255.255.255.0

```
int fa0 10.47.59.10 255.255.255.0
```

DNS

Fa0               10.47.59.20   255.255.255.0
```
int fa0 10.47.59.20 255.255.255.0
```
FTP

Fa0               10.47.59.30   255.255.255.0
```
int fa0 10.47.59.30 255.255.255.0
```


## Paso 1 Configurar etherchannel

https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#4.2.1.2

USA-CORE -> SW-GF
g1/0/3 -> fa0/2 
g1/0/5 -> fa0/3

```bash

# USA-CORE
int g1/0/3
switchport mode trunk
switchport trunk encapsulation dot1q
int g1/0/5
switchport mode trunk
switchport trunk encapsulation dot1q

int range g1/0/3 - g1/0/5
channel-group 1 mode active
int port-channel 1
switchport mode trunk

# SW-GF
int fa0/2
switchport mode trunk
int fa0/3
switchport mode trunk

int range fa0/2 - fa0/3
channel-group 1 mode active
int port-channel 1
switchport mode trunk

```

USA-CORE ->  SW-FF
```bash
# USA-CORE
int g1/0/2
switchport mode trunk
switchport trunk encapsulation dot1q

int g1/0/4
switchport mode trunk
switchport trunk encapsulation dot1q

int range g1/0/2 - g1/0/4
channel-group 2 mode active
int port-channel 2
switchport mode trunk

# SW-FF
int fa0/1
int fa0/5
int range fa0/1 - fa0/5
channel-group 2 mode active
int port-channel 2
switchport mode trunk

```

## Paso 2

Configurar las IPS como en el cuadro


## Paso 3 Configurar HSRP (Hot Standby Router Protocol)

Descripcion: https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#4.3.2.1

Configuracion: https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#4.3.3.1

## Paso 4 Configurar OSPF
Area 0:
- USA-WAN2
- RT-DATACENTER
- RT-LONDON


descripcion: https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#8.1.1.5

configuracion: https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#8.2.2.3

## Paso 5 VLAN 25 en red de gestion

IP = 10.25.0.0/16


## Paso 6 NAT estático en RT-Datacenter

Hacer NAT en RT-Datacenter para que se pude acceder al servidor WWW

47.3.5.10 -> 192.168.5.10




