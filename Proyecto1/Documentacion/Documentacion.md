









## Edificio A

### Dispositivos:

- 3 computadoras (PC-PT)
- 2 Switches (2960-24TT)
- Switch (Switch-PT)
- Acces Point (AC-PT)
- Smartphone (Smartphone-PT)
- 2 Laptops (Laptop-PT)

**VLANs para este edificio**

1. Administracion
2. Laboratorio
3. Docencia

**Asignacion IP**

* Administracion ```192.168.15.0/24```
* Laboratorio ```192.168.45.0/24```
* Docencia ```192.168.25.0/24```


**Configuracion switch SW-A1 modo server**


![alt text](image.png)

```bash
enable
configure terminal 
hostname SW-A1
vtp version 2
vtp domain C8_NetCore
vtp mode server
vtp password proyecto12026

vlan 15
name ADMIN

vlan 25
name DOCENTES

vlan 35
name BIBLIOTECA

vlan 45
name LABORATORIO

vlan 55
name VISITANTES
```

**Configuracion switch SW-A2 y A3 modo cliente**

Misma configuracion para ambos switches


![alt text](image-1.png)

```bash
enable
conf t

hostname SW-A2 

vtp domain C8_NetCore
vtp password proyecto12026
vtp mode client
vtp version 2
```

**Configuración de TRUNK**

*SW-A1 → SW-A2*

![alt text](image-2.png)

```bash
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-A1 → SW-A3*

![alt text](image-3.png)

```bash
interface fa1/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

**Configuración de puertos ACCESS**

*SW-A2*

![alt text](image-5.png)
```bash
# VLAN 15
interface fa0/4
switchport mode access
switchport access vlan 15

# VLAN 45

interface fa0/6
switchport mode access
switchport access vlan 45

interface fa0/5
switchport mode access
switchport access vlan 45


```

*SW-A3*

![alt text](image-6.png)

```bash
# VLAN 25
interface fa0/4
switchport mode access
switchport access vlan 25


```

**Configurar Spanning Tree Rapid-PVST**

**SW-A1 - SW-A2 - SW-A3**

![alt text](image-7.png)


```bash
enable
conf t
spanning-tree mode rapid-pvst

```

**Configurar PortChannel con PAgP**

**Configuración SW-A2 (PAgP DESIRABLE)**
![alt text](image-8.png)
```bash
enable
conf t

interface range fa0/1 , fa0/3
 channel-protocol pagp
 channel-group 1 mode desirable
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 11,21,31,41,51
 no shutdown
exit

wr
```

**Configuración SW-A3 (PAgP AUTO)**

```bash
enable
conf t

interface range fa0/1 , fa0/2
 channel-protocol pagp
 channel-group 1 mode auto
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 11,21,31,41,51
 no shutdown
exit

wr

```

**Comprobaciones**

**Ver las VLANs configuradas**

![alt text](image-9.png)

```bash
show vlan brief
```

**Ver los TRUNKS entre switches**

![alt text](image-10.png)

**SW-A1**
```bash
show interfaces trunk
```

**Ver VTP**

![alt text](image-11.png)

```bash
show vtp status
```

**Ver EtherChanne**
**SW-A2 Y SW-A3**

```bash
show etherchannel summary
```


![alt text](image-12.png)

**Ver Spanning Tree**

![alt text](image-13.png)

```bash
show spanning-tree
```

