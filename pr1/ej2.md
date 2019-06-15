## Sedes

### USA

USA-WAN1

Gi0/1 10.0.0.1      255.255.255.252
Gi0/0 192.168.10.1  255.255.255.0

USA-WAN2

Se0/0/0/0
Fa0/0     192.168.10.2 255.255.255.0

USA-CORE 172.16.100.1 255.255.255.0 VLAN ADMINISTRACION
USA-CORE 172.16.150.1 255.255.255.0 VLAN DESIGN


### DATACENTER

RT-DATACENTER

Fa0/1     10.0.0.2      255.255.255.252
Fa0/0     192.168.5.1   255.255.255.0
Lo10      47.3.5.10     255.255.255.255


WWW
Fa0/0     192.168.5.10  255.255.255.0


### LONDON

RT-LONDON

Se0/0/0/0
Fa0/1.47(VLAN 47)    10.47.59.1    255.255.255.0     VLAN SERVERS
Fa0/1.48(VLAN 48)     10.48.10.1    255.255.255.0     IT

FILE SERVER

Fa0/0             10.47.59.10   255.255.255.0

DNS

Fa0               10.47.59.20   255.255.255.0

FTP

Fa0               10.47.59.30   255.255.255.0



## Paso 1 Configurar etherchannel

https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#4.2.1.2

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

Area 1:

descripcion: https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#8.1.1.5

configuracion: https://static-course-assets.s3.amazonaws.com/ScaN6/es/index.html#8.2.2.3

## Paso 5 VLAN 25 en red de gestion

IP = 10.25.0.0/16


## Paso 6 NAT estático en RT-Datacenter

Hacer NAT en RT-Datacenter para que se pude acceder al servidor WWW

47.3.5.10 -> 192.168.5.10

