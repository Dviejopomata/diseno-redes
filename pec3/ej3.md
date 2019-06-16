# Ejercicio 3 Troubleshooting

## Configurar los dispositivos

Habian algunas interfaces sin encender, las he ido encendiendo para que la red funcionase.

### PCA
Pertenece a la VLAN 100

- IP 192.168.100.5
- SUBNET MASK 255.255.255.0
- GATEWAY 192.168.100.1

Configuracion manual a partir de la interfaz.

### PCB
Pertenece a la VLAN 200

- IP 192.168.200.5
- SUBNET MASK 255.255.255.0
- GATEWAY 192.168.200.1

Configuracion manual a partir de la interfaz.

### SW-CORE

VLAN 100

Interfaz g1/0/23

- IP 192.168.100.1
- SUBNET MASK 255.255.255.0
- GATEWAY 10.0.0.2
Pertenecen PCA

VLAN 200

Interfaz g1/0/24

- IP 192.168.200.1
- SUBNET MASK 255.255.255.0
- GATEWAY 10.0.0.2

Pertenecen PCB

VLAN 400

Interfaz g1/0/1

- IP 10.0.0.1
- SUBNET MASK 255.255.255.252
- GATEWAY 10.0.0.2

Pertenece RT-BCN


```bash
vlan 100
name vlan_100

vlan 200
name vlan_200

vlan 400
name vlan_400

int GigabitEthernet1/0/23
switchport mode access
switchport access vlan 100
ip address 192.168.100.1 255.255.255.0
ip default-gateway 10.0.0.2

int GigabitEthernet1/0/24
switchport mode access
switchport access vlan 200
ip address 192.168.200.1 255.255.255.0
ip default-gateway 10.0.0.2

int GigabitEthernet1/0/1
switchport mode access
switchport access vlan 400
ip address 10.0.0.1 255.255.255.252
ip default-gateway 10.0.0.2

```

### S3
Configurar las interfaces en modo access
```bash

int fa0/18
switchport mode access
switchport access vlan 200

int fa0/5
switchport mode access
switchport access vlan 200

```

### S1
Configurar las interfaces en modo access
```bash

int fa0/6
switchport mode access
switchport access vlan 100

int fa0/5
switchport mode access
switchport access vlan 100
```


### RT-BCN

Gi0/0

- IP 10.0.0.2
- SUBNET MASK 255.255.255.252
- GATEWAY 172.16.0.2

S0/0/1

- IP 172.16.0.1
- SUBNET MASK 255.255.255.252
- GATEWAY 172.16.0.2

```bash
int g0/0
ip address 10.0.0.2 255.255.255.252 
ip default-gateway 172.16.0.2

int s0/0/1
ip address 172.16.0.1 255.255.255.252
ip default-gateway 172.16.0.2

```

### RT-LOND

S0/0/1

- IP 172.16.0.2
- SUBNET MASK 255.255.255.252
- GATEWAY 172.16.0.1

Gi0/0

- IP 192.168.250.1
- SUBNET MASK 255.255.255.0
- GATEWAY 172.16.0.1

```bash
ip default-gateway 172.16.0.1

int s0/0/1
ip address 172.16.0.2 255.255.255.252
ip default-gateway 172.16.0.1

int g0/0
ip address 192.168.250.1 255.255.255.0
ip default-gateway 172.16.0.1
```

### SW-LON

MGMT

- IP 192.168.250.10
- SUBNET MASK 255.255.255.0
- GATEWAY 192.168.250.1

```bash
int vlan 99
ip address 192.168.250.10 255.255.255.0
ip default-gateway 192.168.250.1
no shutdown

```

### WWW

NIC

- IP 192.168.250.100
- SUBNET MASK 255.255.255.0
- GATEWAY 192.168.250.1

Configuracion manual a partir de la interfaz.
