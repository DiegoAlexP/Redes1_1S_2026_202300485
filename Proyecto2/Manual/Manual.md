





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




## 2. Sede Norte 

![alt text](image-12.png)

### Dispositivos:

- 8 computadoras (PC-PT)
- 4 Switches (2960-24TT)
- Router (2911)

**VLANs para este edificio**

| No 	| Nombre      	| VLAN 	|
|:--:	|-------------	|------	|
| 1  	| OPERACIONES       	| 15   	|
| 2  	| ADMINISTRACION    	| 25   	|
| 3  	| SEGURIDAD  	| 35   	|
| 4  	| INVENTARIO 	| 45   	|


**Subnnetting**

| VLAN | Área           | Hosts | Red              | Máscara         | Gateway       |
| ---: | -------------- | ----: | ---------------- | --------------- | ------------- |
|   15 | Operaciones    |    30 | 192.168.86.0/27  | 255.255.255.224 | 192.168.86.1  |
|   45 | Inventario     |    15 | 192.168.86.32/27 | 255.255.255.224 | 192.168.86.33 |
|   25 | Administración |    12 | 192.168.86.64/28 | 255.255.255.240 | 192.168.86.65 |
|   35 | Seguridad      |    10 | 192.168.86.80/28 | 255.255.255.240 | 192.168.86.81 |


**Configuracion router R-Occidente**

```bash
enable
conf t
hostname R-Norte

interface g0/0
 no shutdown

interface g0/0.15
 encapsulation dot1Q 15
 ip address 192.168.86.1 255.255.255.224

interface g0/0.25
 encapsulation dot1Q 25
 ip address 192.168.85.65 255.255.255.240

interface g0/0.35
 encapsulation dot1Q 35
 ip address 192.168.85.81 255.255.255.240

interface g0/0.45
 encapsulation dot1Q 45
 ip address 192.168.85.33 255.255.255.224

end
wr
```
![alt text](image-18.png)


**Configuracion switch SW-NOR-1 modo server**

```bash
enable
conf t
hostname SW-NOR-1

vtp domain CONRED
vtp mode server
vtp password redes123

spanning-tree mode rapid-pvst

spanning-tree vlan 15,25,35,45 root primary

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

interface range fa0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

end
wr
```

![alt text](image-13.png)


**Configuracion switch SW-NOR-2 modo client**

```bash
enable
conf t
hostname SW-NOR-2

vtp domain CONRED
vtp mode server
vtp password redes123

spanning-tree mode rapid-pvst

interface range fa0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

end
wr
```

![alt text](image-14.png)

**Configuracion switch SW-NOR-3 modo client**

```bash
enable
conf t
hostname SW-NOR-3

vtp domain CONRED
vtp mode server
vtp password redes123

spanning-tree mode rapid-pvst

interface range fa0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/4 - 5
 switchport mode Acces
 switchport trunk allowed vlan 15

interface range fa0/6 - 7
 switchport mode Acces
 switchport trunk allowed vlan 25

end
wr
```

![alt text](image-15.png)

**Configuracion switch SW-NOR-4 modo client**

```bash
enable
conf t
hostname SW-NOR-3

vtp domain CONRED
vtp mode server
vtp password redes123

spanning-tree mode rapid-pvst

interface range fa0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45

interface range fa0/4 - 5
 switchport mode Acces
 switchport trunk allowed vlan 35

interface range fa0/6 - 7
 switchport mode Acces
 switchport trunk allowed vlan 45

end
wr
```

![alt text](image-16.png)

**Comprobaciones**

*show vlan brief*

![alt text](image-19.png)

*show interfaces trunk*

![alt text](image-20.png)

*show vtp status*

![alt text](image-21.png)

*show ip interface brief*

![alt text](image-22.png)


*PING*

![alt text](image-23.png)

## 3. Sede Oriente 

![alt text](image-24.png)

### Dispositivos:

- 8 computadoras (PC-PT)
- 3 Switches (2960-24TT)
- 2 Routers (2911)

**VLANs para este edificio**

| No 	| Nombre      	| VLAN 	|
|:--:	|-------------	|------	|
| 1  	| ATENCION REGIONAL       	| 55   	|
| 2  	| ADMINISTRACION    	| 25   	|
| 3  	| SEGURIDAD  	| 35   	|
| 4  	| INVENTARIO 	| 45   	|


**Subnnetting**

| VLAN | Área              | Red              | Máscara         | Gateway HSRP  |
| ---: | ----------------- | ---------------- | --------------- | ------------- |
|   55 | Atención Regional | 192.168.87.0/27  | 255.255.255.224 | 192.168.87.1  |
|   45 | Inventario        | 192.168.87.32/27 | 255.255.255.224 | 192.168.87.33 |
|   25 | Administración    | 192.168.87.64/27 | 255.255.255.224 | 192.168.87.65 |
|   35 | Seguridad         | 192.168.87.96/28 | 255.255.255.240 | 192.168.87.97 |

**Configuracion router R-Occidente1**
```bash
enable
conf t
hostname R-Oriente1

interface g0/0
 no shutdown

interface g0/0.55
 encapsulation dot1Q 55
 ip address 192.168.87.2 255.255.255.224
 standby 55 ip 192.168.87.1
 standby 55 priority 110
 standby 55 preempt

interface g0/0.25
 encapsulation dot1Q 25
 ip address 192.168.87.66 255.255.255.224
 standby 25 ip 192.168.87.65
 standby 25 priority 110
 standby 25 preempt

interface g0/0.35
 encapsulation dot1Q 35
 ip address 192.168.87.98 255.255.255.240
 standby 35 ip 192.168.87.97
 standby 35 priority 90
 standby 35 preempt

interface g0/0.45
 encapsulation dot1Q 45
 ip address 192.168.87.34 255.255.255.224
 standby 45 ip 192.168.87.33
 standby 45 priority 90
 standby 45 preempt

end
wr
```
![alt text](image-25.png)

**Configuracion router R-Occidente2**
```bash
enable
conf t
hostname R-Oriente2

interface g0/0
 no shutdown

interface g0/0.55
 encapsulation dot1Q 55
 ip address 192.168.87.3 255.255.255.224
 standby 55 ip 192.168.87.1
 standby 55 priority 90
 standby 55 preempt

interface g0/0.25
 encapsulation dot1Q 25
 ip address 192.168.87.67 255.255.255.224
 standby 25 ip 192.168.87.65
 standby 25 priority 90
 standby 25 preempt

interface g0/0.35
 encapsulation dot1Q 35
 ip address 192.168.87.99 255.255.255.240
 standby 35 ip 192.168.87.97
 standby 35 priority 110
 standby 35 preempt

interface g0/0.45
 encapsulation dot1Q 45
 ip address 192.168.87.35 255.255.255.224
 standby 45 ip 192.168.87.33
 standby 45 priority 110
 standby 45 preempt

end
wr
```

![alt text](image-26.png)

**Configuracion switch SW-ORI-MAIN modo Server**

```bash
enable
conf t
hostname SW-ORI-MAIN

vtp domain CONRED
vtp mode server
vtp password redes123

spanning-tree mode rapid-pvst

vlan 55
 name ATENCION_REGIONAL
vlan 25
 name ADMINISTRACION
vlan 35
 name SEGURIDAD
vlan 45
 name INVENTARIO

interface range g0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 25,35,45,55

interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 25,35,45,55

end
wr

```

![alt text](image-27.png)

**Configuracion switch SW-ORI-1 modo cliente**

```bash
enable
conf t
hostname SW-ORI-1

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface range f0/1
 switchport mode trunk
 switchport trunk allowed vlan 25,35,45,55

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 55

interface range fa0/4 - 5
 switchport mode access
 switchport access vlan 25

end
wr

```
![alt text](image-28.png)

**Configuracion switch SW-ORI-2 modo cliente**

```bash
enable
conf t
hostname SW-ORI-2

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface range f0/1
 switchport mode trunk
 switchport trunk allowed vlan 25,35,45,55

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 35

interface range fa0/4 - 5
 switchport mode access
 switchport access vlan 45

end
wr

```

![alt text](image-29.png)




**Comprobaciones**

*show vlan brief*

![alt text](image-31.png)

*show interfaces trunk*

![alt text](image-32.png)

*show vtp status*

![alt text](image-33.png)

*show ip interface brief*

![alt text](image-34.png)

![alt text](image-35.png)

*PING*

![alt text](image-30.png)

## 4. Sede Central 

![alt text](image-36.png)

### Dispositivos:

- 10 computadoras (PC-PT)
- 5 Switches (2960-24TT)
- 1 Routers (2911)

**VLANs para este edificio**

| No 	| Nombre      	| VLAN 	|
|:--:	|-------------	|------	|
| 1  	| ATENCION REGIONAL       	| 55   	|
| 2  	| ADMINISTRACION    	| 25   	|
| 3  	| MONITOREO Y CONTROL  	| 65  	|
| 4  	| SOPORTE 	| 75   	|
| 5  	| SERVICIOS CRITICOS 	| 85   	|


**Subnnetting**

| VLAN | Área                | Hosts | Red               | Máscara         | Gateway        |
| ---: | ------------------- | ----: | ----------------- | --------------- | -------------- |
|   65 | Monitoreo y Control |    45 | 192.168.88.0/26   | 255.255.255.192 | 192.168.88.1   |
|   25 | Administración      |    20 | 192.168.88.64/27  | 255.255.255.224 | 192.168.88.65  |
|   85 | Servicios Críticos  |    16 | 192.168.88.96/27  | 255.255.255.224 | 192.168.88.97  |
|   75 | Soporte             |    10 | 192.168.88.128/28 | 255.255.255.240 | 192.168.88.129 |
|   35 | Seguridad           |     9 | 192.168.88.144/28 | 255.255.255.240 | 192.168.88.145 |


**Configuracion router R-Central**


```bash

enable
conf t
hostname R-Central

interface g0/0
 no shutdown

interface g0/0.25
 encapsulation dot1Q 25
 ip address 192.168.88.65 255.255.255.224

interface g0/0.35
 encapsulation dot1Q 35
 ip address 192.168.88.145 255.255.255.240

interface g0/0.65
 encapsulation dot1Q 65
 ip address 192.168.88.1 255.255.255.192

interface g0/0.75
 encapsulation dot1Q 75
 ip address 192.168.88.129 255.255.255.240

interface g0/0.85
 encapsulation dot1Q 85
 ip address 192.168.88.97 255.255.255.224

end
wr

```
![alt text](image-43.png)

**Configuracion switch SW-CEN-1 modo server**

```bash
enable
conf t
hostname SW-CEN-1

vtp domain CONRED
vtp mode server
vtp password redes123

vlan 25
 name ADMINISTRACION
vlan 35
 name SEGURIDAD
vlan 65
 name MONITOREO_CONTROL
vlan 75
 name SOPORTE
vlan 85
 name SERVICIOS_CRITICOS


spanning-tree mode rapid-pvst
spanning-tree vlan 25,35,65,75,85 root primary


interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85


interface range fa0/3 - 4 
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85


interface range fa0/1 - 2
channel-protocol pagp
channel-group 2 mode desirable
no shutdown
exit

interface port-channel 2
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85
no shutdown

end
wr


```

![alt text](image-37.png)

![alt text](image-38.png)

**Configuracion switch SW-CEN-2 modo cliente**

```bash
enable
conf t
hostname SW-CEN-2

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst
spanning-tree vlan 25,35,65,75,85 root secondary

interface range fa0/3 - 4 
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85


interface range fa0/1 - 2
channel-protocol pagp
channel-group 2 mode desirable
no shutdown
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 25,35,65,75,85
no shutdown

end
wr


```
![alt text](image-39.png)


**Configuracion switch SW-CEN-3 modo cliente**

```bash
enable
conf t
hostname SW-CEN-3

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface range fa0/1 
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 25
 spanning-tree portfast

end
wr


```

![alt text](image-40.png)

**Configuracion switch SW-CEN-4 modo cliente**

```bash
enable
conf t
hostname SW-CEN-4

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface range fa0/1 - 2 
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85

interface range fa0/3 - 4
 switchport mode access
 switchport access vlan 35
 spanning-tree portfast

interface range fa0/5 - 6
 switchport mode access
 switchport access vlan 65
 spanning-tree portfast

end
wr


```

![alt text](image-41.png)


**Configuracion switch SW-CEN-5 modo cliente**

```bash
enable
conf t
hostname SW-CEN-5

vtp domain CONRED
vtp mode client
vtp password redes123

spanning-tree mode rapid-pvst

interface range fa0/1 
 switchport mode trunk
 switchport trunk allowed vlan 25,35,65,75,85

interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 75
 spanning-tree portfast

interface range fa0/4 - 5
 switchport mode access
 switchport access vlan 85
 spanning-tree portfast

end
wr


```
![alt text](image-42.png)



**Comprobaciones**

*show vlan brief*

![alt text](image-45.png)

*show interfaces trunk*

![alt text](image-46.png)

*show vtp status*

![alt text](image-47.png)

*show etherchannel summary*

![alt text](image-48.png)

*show ip interface brief*

![alt text](image-49.png)

*PING*

![alt text](image-44.png)