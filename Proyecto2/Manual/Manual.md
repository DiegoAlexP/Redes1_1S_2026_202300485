





## 1. Sede Occidente 

![alt text](image.png)

### Dispositivos:

- 8 computadoras (PC-PT)
- 5 Switches (2960-24TT)
- Router (2911)

**VLANs para este edificio**

| No 	| Nombre      	| VLAN 	|
|:--:	|-------------	|------	|
| 1  	| OPERACIONES       	| 15   	|
| 2  	| ADMINISTRACION    	| 25   	|
| 3  	| SEGURIDAD  	| 35   	|
| 4  	| INVENTARIO 	| 45   	|


**Subnnetting**

| VLAN | Área           | Hosts requeridos | Red               | Máscara         | Gateway        |
| ---: | -------------- | ---------------: | ----------------- | --------------- | -------------- |
|   45 | Inventario     |               55 | 192.168.85.0/26   | 255.255.255.192 | 192.168.85.1   |
|   15 | Operaciones    |               40 | 192.168.85.64/26  | 255.255.255.192 | 192.168.85.65  |
|   25 | Administración |               18 | 192.168.85.128/27 | 255.255.255.224 | 192.168.85.129 |
|   35 | Seguridad      |                8 | 192.168.85.160/28 | 255.255.255.240 | 192.168.85.161 |

**Configuracion router R-Occidente modo server**
```bash
enable
conf t
hostname R-Occidente

interface g0/0
 no shutdown

interface g0/0.15
 encapsulation dot1Q 15
 ip address 192.168.85.65 255.255.255.192

interface g0/0.25
 encapsulation dot1Q 25
 ip address 192.168.85.129 255.255.255.224

interface g0/0.35
 encapsulation dot1Q 35
 ip address 192.168.85.161 255.255.255.240

interface g0/0.45
 encapsulation dot1Q 45
 ip address 192.168.85.1 255.255.255.192

end
wr
```
![alt text](image-6.png)

**Configuracion switch SW-OCC-MAIN modo server**

```bash
enable
conf t
hostname SW-OCC-MAIN

vtp domain CONRED
vtp mode server
vtp password redes123

vlan 15
 name OPERACIONES
vlan 25
 name ADMINISTRACION
vlan 35
 name SEGURIDAD
vlan 45
 name INVENTARIO

interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/1 - 4
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

end
wr
```
![alt text](image-1.png)

**Configuracion switch SW-OCC-1 modo client**
```bash
enable
conf t
hostname SW-OCC-1 

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 15

end
wr
```
![alt text](image-2.png)

**Configuracion switch SW-OCC-2 modo client**
```bash
enable
conf t
hostname SW-OCC-2

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 25

end
wr
```
![alt text](image-3.png)

**Configuracion switch SW-OCC-3 modo client**

```bash
enable
conf t
hostname SW-OCC-3

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 35


end
wr
```

![alt text](image-4.png)

**Configuracion switch SW-OCC-4 modo client**
```bash
enable
conf t
hostname SW-OCC-4

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 45


end
wr

```

![alt text](image-5.png)

**Comprobaciones**

*show vlan brief*

![alt text](image-8.png)

*show interfaces trunk*

![alt text](image-9.png)

*show vtp status*

![alt text](image-10.png)

*show ip interface brief*

![alt text](image-11.png)


*PING*

![alt text](image-7.png)