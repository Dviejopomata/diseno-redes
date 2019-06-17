# M11 

## A13

VLAN 1 10.1.0.0/16 SERVER TIPO 1
VLAN 2 10.2.0.0/16 SERVER TIPO 2
VLAN 3 10.3.0.0/16 SERVER TIPO 3
VLAN 4 10.4.0.0/16 I+D Y M
VLAN 5 10.5.0.0/16 A CG



## SM01

```bash
vtp mode server
vtp domain UOC
vtp password cisco

vlan 10
name SERVER1
vlan 20
name SERVER2
vlan 30
name SERVER3
vlan 40
name ID_M
vlan 50
name A_CG



int fa0/2
switchport mode trunk

int fa0/3
switchport mode trunk

int fa0/4
switchport mode trunk

int fa0/1
switchport mode access
switchport access vlan 30

int fa0/5
switchport mode access
switchport access vlan 40


```

### Server tipo 3

```bash
10.3.0.10 255.255.0.0
```



## M11A11

### Switch

```bash
vtp mode client
vtp domain UOC
vtp password cisco


int fa0/2
switchport mode trunk

int fa0/3
switchport mode trunk

int fa0/1
switchport mode access
switchport access vlan 40



```


### I+D
```bash
10.4.0.10 255.255.0.0
```



## M11A13
### Switch
```bash
vtp mode client
vtp domain UOC
vtp password cisco


int fa0/3
switchport mode trunk


int fa0/2
switchport mode access
switchport access vlan 20

int fa0/1
switchport mode access
switchport access vlan 10

int fa0/4
switchport mode access
switchport access vlan 40

int fa0/5
switchport mode access
switchport access vlan 30

```

### Server tipo 1
```bash
10.1.0.10 255.255.0.0
```

### Server tipo 2
```bash
10.2.0.30 255.255.0.0
```


## M12A21

### Switch

```bash
vtp mode client
vtp domain UOC
vtp password cisco


int fa0/2
switchport mode trunk

```

### Server tipo 2
```bash
10.2.0.20 255.255.0.0
```


## M12A12
### Switch
```bash
vtp mode client
vtp domain UOC
vtp password cisco


int fa0/1
switchport mode trunk

```

### CG

```bash
10.5.0.10 255.255.0.0
```

### Server tipo 2

```bash
10.2.0.10 255.255.0.0
```


## Router M01

```bash
int g0/0
ip address 192.168.1.1 255.255.255.0

int g0/2/0
ip address 10.6.0.1 255.255.255.0

```

## Router Sala 01

```bash

int g0/3/0
ip address 10.6.0.2 255.255.255.252

int s0/2/0
ip address 10.10.10.1 255.255.255.252

```


