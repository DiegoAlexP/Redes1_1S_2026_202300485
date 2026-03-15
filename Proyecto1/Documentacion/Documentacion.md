


# UNIVERSIDAD DE SAN CARLOS DE GUATEMALA
## FACULTAD DE INGENIERÍA
### REDES DE COMPUTADORAS 1

**TUTOR ACADÉMICO:** César Fernando Sazo Quisquinay

---

**Diego Alexander Pablo Subuyuj**  
**CARNÉ:** 202300485  
**SECCIÓN:** N

**Guatemala, 19 de enero del 2026**


## PROYECTO 1
### NetCore Academy


## OBJETIVOS DEL SISTEMA

### GENERAL
Disenar, simular y documentar una red VLAN de cuatro edificios por medio de Cisco Packet Tracer, aplicando buenas practicas de segmentacion logica, administracion de switches y validacion de conectividad en un entorno controlado.

### ESPECÍFICOS
- Modelar una topologia funcional para un edificio de 4 edificios con al menos 50 dispositivos finales.
- Configurar VLANs para separar trafico por areas o funciones, mejorando orden, seguridad y administracion.
- Implementar VTP para centralizar la distribucion de informacion de VLAN entre switches del mismo dominio.
- Establecer enlaces troncales 802.1Q para transportar multiples VLANs entre equipos de conmutacion.
- Configurar puertos de acceso segun la necesidad de cada dispositivo final y su respectiva VLAN.
- Aplicar Spanning Tree (Rapid-PVST) para prevenir bucles de capa 2 y acelerar la convergencia.
- Utilizar EtherChannel para aumentar capacidad y redundancia en enlaces entre switches.
- Verificar conectividad y consistencia de configuracion mediante pruebas basicas de comunicacion y revision de tablas.

## INTRODUCCIÓN

El presente proyecto integra los fundamentos de conmutacion vistos en el curso de Redes de Computadoras 1, trasladandolos a un escenario practico de simulacion.

Desde una perspectiva tecnica, la propuesta se centra en separar dominios de broadcast por medio de VLANs, estandarizar la propagacion de dichas VLANs con VTP y asegurar una interconexion confiable con enlaces troncales. Adicionalmente, se incorporan mecanismos de resiliencia y eficiencia como Rapid-PVST y EtherChannel, que son esenciales para evitar fallas por bucles y para optimizar el aprovechamiento de los enlaces. La simulacion permite validar decisiones de diseno antes de una implementacion real, reduciendo riesgos y facilitando la comprension del comportamiento de la red. De esta forma, el proyecto no solo evidencia el dominio de comandos de configuracion, sino tambien la capacidad de planificar, justificar y documentar una solucion de red alineada con requerimientos academicos.

## JUSTIFICACION

La realizacion de esta practica fortalece la relacion entre teoria y aplicacion real en el area de redes. Implementar un escenario completo en Packet Tracer permite comprender con mayor claridad como se comportan los dispositivos de capa 2 ante diferentes politicas de segmentacion y transporte de trafico.

Asimismo, el proyecto aporta competencias clave para entornos profesionales: analisis de requerimientos, diseno de topologias, estandarizacion de configuraciones, deteccion temprana de errores y documentacion tecnica reproducible.

## ALCANCE DE LA PRÁCTICA

* Diseno logico y fisico de la topologia LAN para los tres niveles del edificio en Cisco Packet Tracer.
* Configuracion de switches con funciones de acceso, distribucion de VLANs y enlaces troncales.
* Aplicacion de practicas de redundancia y control de lazo de capa 2 con Rapid-PVST y EtherChannel.
* Asignacion de direccionamiento IP estatico sin duplicidad para los hosts definidos.
* Verificacion funcional de comunicacion entre equipos permitidos por la segmentacion implementada.
* Elaboracion de evidencia tecnica de configuraciones y resultados de la simulacion.

### LIMITACIONES

* El entorno es de simulacion, por lo que no se contemplan factores fisicos reales como interferencia, fallos electricos o limitaciones de cableado.
* No se incluye implementacion de servicios avanzados de capa 3 (enrutamiento dinamico, ACLs complejas o alta disponibilidad de gateway).
* Las pruebas se orientan al cumplimiento de la practica academica y no a una auditoria de seguridad de nivel empresarial.

## RECURSOS Y HERRAMIENTAS UTILIZADAS

* Cisco Packet Tracer
* Switches Cisco 2960
* VPCs (para equipos finales)
* GitHub (repositorio del curso)
* Editor Markdown

## MARCO TEORICO RESUMIDO

### VLAN
Una VLAN permite dividir una red fisica en varias redes logicas independientes. Esto reduce dominios de broadcast, mejora el orden del trafico y facilita aplicar politicas administrativas por area funcional.

### VTP
VTP (VLAN Trunking Protocol) simplifica la administracion de VLANs al propagar cambios dentro de un mismo dominio. En este proyecto se usa un switch en modo servidor y switches en modo cliente para mantener consistencia.

### Trunk
Un enlace trunk transporta multiples VLANs sobre un mismo enlace fisico entre switches. Esto permite interconectar segmentos de red sin requerir un enlace dedicado por cada VLAN.

### STP / Rapid-PVST
Spanning Tree evita bucles de capa 2 al bloquear caminos redundantes cuando es necesario. Rapid-PVST mejora los tiempos de convergencia y mantiene estabilidad ante cambios de topologia.

### EtherChannel
EtherChannel agrupa varios enlaces fisicos en un unico enlace logico. Sus ventajas principales son mayor ancho de banda agregado y redundancia frente a la falla de un enlace individual.


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
 channel-protocol LACP
 channel-group 1 mode activate
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45,55
 no shutdown
exit

wr
```

**Configuración SW-A3 (PAgP AUTO)**

```bash
enable
conf t

interface range fa0/1 , fa0/2
 channel-protocol LACP
 channel-group 1 mode activate
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45,55
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

**Configuración en SW-A1**

Para fibra optica entre edificios

![alt text](image-14.png)

```bash
enable
conf t

interface range fa4/1, fa5/1
channel-protocol pagp
channel-group 2 mode desirable
no shutdown
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr
```

## Edificio B

### Dispositivos:


- 4 computadoras (PC-PT)
- 2 Switches (2960-24TT)
- 2 Switch (Switch-PT)
- Hub (HUB.PT)
- 3 Laptops (Laptop-PT)

**VLANs para este edificio**

1. Administracion
2. Biblioteca
3. Docencia

**Asignacion IP**

* Administracion ```192.168.15.0/24```
* Biblioteca ```192.168.35.0/24```
* Docencia ```192.168.25.0/24```


**Configuracion switch SW-B1 modo cliente**

![alt text](image-15.png)

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

*SW-B1 → SW-B3*

![alt text](image-16.png)


```bash
interface g0/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-B1 → SW-B4*

![alt text](image-17.png)

```bash
interface g1/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

*SW-B3 → SW-B4*

![alt text](image-18.png)

```bash
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


**Configuración de puertos ACCESS**

*SW-B3*

![alt text](image-19.png)
```bash
# VLAN 35
interface fa0/2
switchport mode access
switchport access vlan 35

# VLAN 25

interface fa0/3
switchport mode access
switchport access vlan 25


```

*SW-B4*

![alt text](image-6.png)

```bash
# VLAN 15
interface fa0/2
switchport mode access
switchport access vlan 15

# VLAN 35
interface fa0/3
switchport mode access
switchport access vlan 35


```


**Configurar Spanning Tree Rapid-PVST**

**SW-A1 - SW-A2 - SW-A3**

![alt text](image-20.png)

![alt text](image-21.png)


```bash
enable
conf t
spanning-tree mode rapid-pvst

```

**Configuracion SW-B2**

![alt text](image-22.png)

```bash
enable
conf t

hostname SW-B2 

vtp domain C8_NetCore
vtp password proyecto12026
vtp mode client
vtp version 2

```

**Configuración en SW-B2**

Para fibra optica entre edificios

![alt text](image-23.png)

```bash
enable
conf t

interface range fa9/1, fa8/1
channel-protocol pagp
channel-group 2 mode desirable
no shutdown
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr
```


![alt text](image-24.png)

```bash
conf t
interface range fa5/1, fa4/1
channel-group 2 mode desirable
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr

```

**Configuración de puertos ACCESS**

**SW-B2**

```bash
# VLAN 35
interface fa0/3
switchport mode access
switchport access vlan 35
```



**Comprobaciones**

**Ver las VLANs configuradas**

![alt text](image-9.png)

```bash
show vlan brief
```

**Ver los TRUNKS entre switches**

![alt text](image-25.png)

**SW-B1**
```bash
show interfaces trunk
```

**Ver VTP**

![alt text](image-26.png)

```bash
show vtp status
```

**Ver EtherChanne**
**SW-A2 Y SW-A3**

```bash
show etherchannel summary
```


![alt text](image-27.png)

**Ver Spanning Tree**

![alt text](image-28.png)

```bash
show spanning-tree
```

## Edificio C

### Dispositivos:

- 5 computadoras (PC-PT)
- 2 Switches (2960-24TT)
- 2 Switches (Switch-PT)
- Hub (Hub-PT)

**VLANs para este edificio**

1. Administracion
2. Biblioteca
3. Docencia

**Asignacion IP**

* Administracion ```192.168.15.0/24```
* Biblioteca ```192.168.35.0/24```
* Docencia ```192.168.25.0/24```


**Configuracion switch SW-C1, C2, C3 Y C4 modo cliente**


Misma configuracion para los 3 switches


![alt text](image-29.png)

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

*SW-C4 → HUB*

![alt text](image-30.png)

```bash
interface g9/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-C3 → HUB*

![alt text](image-35.png)

```bash
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

*SW-C1 → HUB*

![alt text](image-33.png)

```bash
interface fa3/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-C2 → HUB*

![alt text](image-34.png)

```bash
interface fa0/3
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


**Configuración de puertos ACCESS**

*SW-C3*

![alt text](image-36.png)
```bash

# VLAN 35
interface fa0/2
switchport mode access
switchport access vlan 35

# VLAN 15

interface fa0/3
switchport mode access
switchport access vlan 15

```

*SW-C1*

![alt text](image-37.png)

```bash
# VLAN 15
interface fa1/1
switchport mode access
switchport access vlan 15


```

*SW-C2*

![alt text](image-38.png)

```bash
# VLAN 15
interface fa0/2
switchport mode access
switchport access vlan 15

# VLAN 15
interface fa0/1
switchport mode access
switchport access vlan 15


```

**Configurar Spanning Tree Rapid-PVST**

**SW-C1 - SW-C2 - SW-C3**

![alt text](image-39.png)


```bash
enable
conf t
spanning-tree mode rapid-pvst

```


**Configurar PortChannel con PAgP**

**Configuración SW-B4 (PAgP DESIRABLE)**
![alt text](image-31.png)
```bash
enable
conf t

interface range fa8/1 , fa7/1
 channel-protocol pagp
 channel-group 1 mode desirable
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45,55
 no shutdown
exit

wr
```

**Configuración SW-A3 (PAgP DESIRABLE)**

![alt text](image-32.png)

```bash
enable
conf t

interface range fa6/1 , fa3/1
 channel-protocol pagp
 channel-group 1 mode desirable
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 15,25,35,45,55
 no shutdown
exit

wr

```



**Comprobaciones**

**Ver las VLANs configuradas**

![alt text](image-40.png)

```bash
show vlan brief
```

**Ver los TRUNKS entre switches**

![alt text](image-41.png)

**SW-B1**
```bash
show interfaces trunk
```

**Ver VTP**

![alt text](image-42.png)

```bash
show vtp status
```

**Ver EtherChanne**
**SW-A2 Y SW-A3**

```bash
show etherchannel summary
```


![alt text](image-43.png)

**Ver Spanning Tree**

![alt text](image-44.png)

```bash
show spanning-tree
```


## Edificio D

### Dispositivos:

- 7 computadoras (PC-PT)
- 3 Switches (2960-24TT)
- 3 Switches (Switch-PT)
- Acces Point (AC-PT)
- Repetidor (Repeater-PT)
- Server (Server-PT)

**VLANs para este edificio**

1. Administracion
2. Laboratorio
3. Biblioteca
4. Visitantes

**Asignacion IP**

* Administracion ```192.168.15.0/24```
* Laboratorio ```192.168.45.0/24```
* Biblioteca ```192.168.35.0/24```
* Visitantes ```192.168.55.0/24```


**Configuracion switch SW-D1, D2, D3, D4 Y D5 modo cliente**


Misma configuracion para los 5 switches excepto 1.

![alt text](image-45.png)

```bash
enable
conf t

hostname SW-A2 

vtp domain C8_NetCore
vtp password proyecto12026
vtp mode client
vtp version 2
```

**Configuracion switch SW-E1 modo Transparente**

![alt text](image-58.png)

![alt text](image-59.png)

```bash
enable
conf t

hostname SW-E1
vtp domain C8_NetCore
vtp password proyecto12026
vtp mode transparent
vtp version 2

```

**Ethernet Channel**
**Configuración en SW-D5**

Para fibra optica entre edificios

![alt text](image-46.png)

```bash
enable
conf t

interface range fa4/1, fa5/1
channel-protocol pagp
channel-group 1 mode desirable
no shutdown
exit

interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr
```

SW 2

![alt text](image-47.png)

```bash
conf t
interface range fa7/1, fa8/1
channel-group 1 mode desirable
exit

interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr

```


**Configuración en SW-D1**

Para fibra optica entre edificios

![alt text](image-48.png)

```bash
enable
conf t

interface range fa5/1, fa6/1
channel-protocol pagp
channel-group 2 mode desirable
no shutdown
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr
```

SW 2

![alt text](image-49.png)

```bash
conf t
interface range fa4/1, fa5/1
channel-protocol pagp
channel-group 2 mode desirable
no shutdown
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
no shutdown
end
wr

```




**Configuración de TRUNK**

*SW-C4 → HUB*

![alt text](image-50.png)

```bash
interface g9/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-D1 → SW-D5*

![alt text](image-51.png)

```bash
interface fa4/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

*SW-D5 → SW-D2*

![alt text](image-52.png)

```bash
interface gig7/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-D2 → SW-D5*

![alt text](image-53.png)

```bash
interface gig6/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```
*SW-D2 → SW-D4*
![alt text](image-55.png)
```bash
interface fa1/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

*SW-D4 → SW-D2*

![alt text](image-54.png)

```bash
interface fa0/3
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

*SW-D2 → Repeater*

![alt text](image-56.png)

```bash
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```
*SW-D3 → Repeater*

![alt text](image-57.png)

```bash
interface fa0/4
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


*SW-D2 → SW-E1*

![alt text](image-60.png)

```bash
interface fa2/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```

*SW-D1 → SW-E1*

![alt text](image-61.png)

```bash
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 15,25,35,45,55
```


**Configurar Spanning Tree Rapid-PVST**

**SW-D1 - SW-D2 - SW-D3 - SW-D4 - SW-D5**

![alt text](image-62.png)


```bash
enable
conf t
spanning-tree mode rapid-pvst

```




**Configuración de puertos ACCESS**

*SW-D4*

![alt text](image-63.png)
```bash

# VLAN 35
interface fa0/1
switchport mode access
switchport access vlan 35

# VLAN 45

interface fa0/2
switchport mode access
switchport access vlan 45

```

*SW-D3*

![alt text](image-64.png)

```bash
# VLAN 15
interface range fa0/1, fa0/3
switchport mode access
switchport access vlan 15

# VLAN 25
interface fa0/2
switchport mode access
switchport access vlan 25



```

*SW-C2*

![alt text](image-38.png)

```bash
# VLAN 15
interface fa0/2
switchport mode access
switchport access vlan 15

# VLAN 15
interface fa0/1
switchport mode access
switchport access vlan 15


```




**Comprobaciones**

**Ver las VLANs configuradas**

![alt text](image-65.png)

```bash
show vlan brief
```

**Ver los TRUNKS entre switches**

![alt text](image-66.png)

**SW-B1**
```bash
show interfaces trunk
```

**Ver VTP**

![alt text](image-67.png)

```bash
show vtp status
```

**Ver EtherChanne**
**SW-B2 Y SW-D5**


![alt text](image-68.png)


```bash
show etherchannel summary
```




**Ver Spanning Tree**

![alt text](image-69.png)

```bash
show spanning-tree
```


### COMPROBACIONES GENERALES PINGS


**Ping Edificio A al B**


![alt text](image-70.png)



**Ping Edificio A al C**


![alt text](image-71.png)

**Ping Edificio A al D**

![alt text](image-72.png)

## Topologia Completa

![alt text](image-73.png)