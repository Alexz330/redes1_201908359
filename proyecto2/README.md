# Proyecto 2

_Este es un proyecto universitario del curso de Redes de Computadoras 1, en donde se demuestra los temas aprendidos en las actividades anteriores._

## 🚀 Comenzando

### 📋 Requerimientos

* Cisco Packet Tracer - 8.2.1.0118

### Indice

* [Documentación](#documentacion)
    * [Topologia](#topologia)
    * [Calculo del VLSM](#calculo-vlsm)
    * [VLSM Obtenidos](#vlsm-obtenidos)
    * [FLSM Obtenidos](#flsm-obtenidos)
    * [Configuración de la Sede Jutiapa](#configuracion-jutiapa)
        * [SW2](#sw2)
        * [SW3](#sw3)
        * [ESW1](#esw1)
        * [J1](#j1)
        * [J2](#j2)
    * [Configuración de la Sede Escuintla](#configuracion-escuintla)
        * [SW0](#sw0)
    * [Configuración del Core](#configuracion-core)
        * [CENTRAL](#router-central)
        * [ESCUINTLA](#router-escuintla)
        * [JUTIAPA](#router-jutiapa)
        * [PETEN](#router-peten)
    * [Configuración de VPCs](#configuracion-vpcs)
        * [VPC0](#vpc0)
        * [VPC1](#vpc1)
        * [VPC2](#vpc2)
        * [VPC3](#vpc3)
        * [VPC4](#vpc4)
        * [VPC5](#vpc5)
        * [VPC6](#vpc6)
        * [VPC7](#vpc7)

## 🎁 Otros

[Enunciado]([REDES1]Proyecto2.pdf)

## 📖 Documentacion <div id='documentacion'></div>

### 🔎 Topologia <div id="topologia"></div>

![alt text](image.png)

### 🔢 Calculo del VLSM <div id="calculo-vlsm"></div>

Para ilustrar el proceso de cálculo del VLSM, se utilizará como ejemplo la sede de **Jutiapa** durante toda la explicación.

#### Paso 1: Ordenamiento

Las VLANs se organizan de acuerdo con la cantidad de equipos necesarios para cada una, ordenándolas de mayor a menor.

|VLAN|ID VLAN|Equipos|
|:----|:----|:----|
|Ventas|32|25|
|Informatica|42|12|
|RRHH|12|10|
|Contabilidad|22|4|

#### Paso 2: Asignacion de mascara y wildcard

Es importante tener en cuenta que al definir la mascara de una red es necesario determinar la cantidad de hosts que deseamos tener en la red, para esto debemos considerar dos aspectos clave:

* La Parte de Red.
* La Parte de Host.

Es a través de la *Parte de Host* que se establece la cantidad de direcciones IP disponibles en la red. Por lo tanto, resulta crucial realizar los cálculos correspondientes para determinar adecuadamente la cantidad de hosts que la red podrá alojar.

A modo de ejemplo, se utilizará la VLAN 'Ventas'.

1. **Calcular la cantidad de equipos para la _Parte de Host_**

    Para determinar la cantidad de equipos, empleamos la fórmula $2^n$. A continuación, evaluamos varias opciones:

    * Cuando $n=4$, resulta en $2^4 = 16$ equipos.
    * Cuando $n=5$, resulta en $2^5 = 32$ equipos.
    * Cuando $n=6$, resulta en $2^6 = 64$ equipos.

    Después de obtener los resultados, determinamos que la opción que mejor se ajusta a la cantidad de equipos requeridos es cuando $n=5$.

    **N** representa la cantidad de bits que se utilizan para identificar la _Parte de Host_.

2. **Determinación de la cantidad de bits para la _Parte de Red_**

    Una vez calculado **N**, procedemos a determinar la cantidad de bits utilizados para la _Parte de Red_. Este cálculo se realiza de la siguiente manera:

    $32 - 5 = 27$ bits

    Por lo tanto, se concluye que la cantidad de bits para la _Parte de Red_ es de 27 bits, lo que se expresa en notación CIDR como /27.

    > Nota: Se resta con 32 debido a que estamos evaluando una dirección IPv4, la cual consta de 32 bits.

3. **Calculo de la mascara en decimal**

    Dado que la dirección IP consta de 32 bits, estos se representan de la siguiente manera:

    _Parte de Red_ + _Parte de Host_

    En este caso, la _Parte de Red_, que consta de 27 bits, se representa con unos (1s), mientras que la _Parte de Host_, con 5 bits, se representa con ceros (0s). En binario y decimal, esto se ve así:

    $11111111.11111111.11111111.11100000 = 255.255.255.224$

    Entonces la mascara es **255.255.255.224**

4. **Calculo del Wildcard**

    Para realizar este cálculo, se debe utilizar exclusivamente la cantidad de equipos determinada en el **paso 1**. En este ejemplo, con 32 equipos numerados del 0 al 31, el wildcard correspondiente es 0.0.0.31.

El calculo anteriormente visto, fue aplicado en cada VLAN, por lo cual, se obtuvo la siguiente tabla.

|VLAN|ID VLAN|Equipos|Mascara|Wildcard|
|:----|:----|:----|:----|:----|
|Ventas|32|25|255.255.255.224|0.0.0.31|
|Informatica|42|12|255.255.255.240|0.0.0.15|
|RRHH|12|10|255.255.255.240|0.0.0.15|
|Contabilidad|22|4|255.255.255.248|0.0.0.7|

#### Paso 3: Asignación de ID Red, Primera IP, Ultima IP e IP Broadcast

Donde:

* XX = 29
* Red Interna (Sede Jutiapa) = 192.168.XX.0/24 = 192.168.29.0/24

1. **Asignación de ID Red**

    Se asigna la red interna inicial.

    En este caso se define: 192.168.29.0

2. **Asignación de Primera IP**

    Sería la IP siguiente en la secuencia.

    En este caso se define: 192.168.29.1

3. **Asignación de Ultima IP**

    Sería la IP del broadcast menos 1

    En este caso se define: 192.168.29.30

4. **Asignación del Broadcast**

    La dirección de broadcast es la última dirección IP asignable. Esta se determina por la cantidad de Host que hay disponibles

    > Nota: Una forma de visualizar la cantidad de Host que hay, es por medio del wildcard

    En este caso se define: 192.168.29.31

> Nota: Para la próxima VLAN, al asignar la ID de Red, es importante definirla en relación con la secuencia que se sigue en la asignación del broadcast.

La tabla obtenida con respecto a las asignaciones son las siguientes:

|VLAN|ID VLAN|Equipos|Mascara|Wildcard|ID Red|Primera IP|Ultima IP|IP Broadcast|
|:----|:----|:----|:----|:----|:----|:----|:----|:----|
|Ventas|32|25|255.255.255.224|0.0.0.31|192.168.29.0|192.168.29.1|192.168.29.30|192.168.29.31|
|Informatica|42|12|255.255.255.240|0.0.0.15|192.168.29.32|192.168.29.33|192.168.29.46|192.168.29.47|
|RRHH|12|10|255.255.255.240|0.0.0.15|192.168.29.48|192.168.29.49|192.168.29.62|192.168.29.63|
|Contabilidad|22|4|255.255.255.248|0.0.0.7|192.168.29.64|192.168.29.65|192.168.29.70|192.168.29.71|

### 📄 VLSM Obtenidos <div id="vlsm-obtenidos"></div>

#### Para la sede de Jutiapa

* ID Red = 192.168.XX.0/24
* XX = 29

|VLAN|ID VLAN|Equipos|Mascara|Wildcard|ID Red|Primera IP|Ultima IP|IP Broadcast|
|:----|:----|:----|:----|:----|:----|:----|:----|:----|
|Ventas|32|25|255.255.255.224|0.0.0.31|192.168.29.0|192.168.29.1|192.168.29.30|192.168.29.31|
|Informatica|42|12|255.255.255.240|0.0.0.15|192.168.29.32|192.168.0.33|192.168.29.46|192.168.29.47|
|RRHH|12|10|255.255.255.240|0.0.0.15|192.168.29.48|192.168.29.49|192.168.29.62|192.168.29.63|
|Contabilidad|22|4|255.255.255.248|0.0.0.7|192.168.29.64|192.168.29.65|192.168.29.70|192.168.29.71|

#### Para la sede de Escuintla

* ID Red = 192.148.XX.0/24
* XX = 29

|VLAN|ID VLAN|Equipos|Mascara|Wildcard|ID Red|Primera IP|Ultima IP|IP Broadcast|
|:----|:----|:----|:----|:----|:----|:----|:----|:----|
|Ventas|3Y|20|255.255.255.224|0.0.0.31|192.148.29.0|192.148.29.1|192.148.29.30|192.148.29.31|
|RRHH|1Y|5|255.255.255.248|0.0.0.7|192.148.29.32|192.148.29.33|192.148.29.38|192.148.29.39|


#### Para la sede de Quiche

* ID Red = 192.178.XX.0/24
* XX = 29

| VLAN         | ID VLAN | Equipos | Mascara         | Wildcard | ID Red         | Primera IP     | Ultima IP      | IP Broadcast   |
|--------------|---------|---------|-----------------|----------|----------------|----------------|----------------|----------------|
| Ventas       | 3Y      | 36      | 255.255.255.192 | 0.0.0.63 | 192.178.29.0   | 192.178.29.1   | 192.178.29.62  | 192.178.29.63  |
| Informatica  | 4Y      | 21      | 255.255.255.224 | 0.0.0.31 | 192.178.29.64  | 192.178.29.65  | 192.178.29.94  | 192.178.29.95  |
| RRHH         | 1Y      | 12      | 255.255.255.240 | 0.0.0.15 | 192.178.29.96  | 192.178.29.97  | 192.178.29.110 | 192.178.29.111 |
| Contabilidad | 2Y      | 10      | 255.255.255.240 | 0.0.0.15 | 192.178.29.112 | 192.178.29.113 | 192.178.29.126 | 192.178.29.127 |

#### Para la sede de Peten

* ID Red = 192.158.XX.0/24
* XX = 29

| VLAN        | ID VLAN | Equipos | Mascara         | Wildcard | ID Red        | Primera IP    | Ultima IP     | IP Broadcast  |
|-------------|---------|---------|-----------------|----------|---------------|---------------|---------------|---------------|
| Ventas      | 3Y      | 30      | 255.255.255.224 | 0.0.0.31 | 192.158.29.0  | 192.158.29.1  | 192.158.29.30 | 192.158.29.31 |
| Informatica | 4Y      | 15      | 255.255.255.224 | 0.0.0.31 | 192.158.29.32 | 192.158.29.33 | 192.158.29.62 | 192.158.29.63 |
| RRHH        | 1Y      | 10      | 255.255.255.240 | 0.0.0.15 | 192.158.29.64 | 192.158.29.65 | 192.158.29.78 | 192.158.29.79 |

### 📄 FLSM Obtenidos <div id="flsm-obtenidos"></div>

Para el calculo del FLSM se han considerado 14 host por cada router.

#### Para el Router Central

|Interfaz|IP Red|Mascara|
|:----|:----|:----|
|f0/0|10.0.0.1|255.255.255.240|
|f1/0|10.0.0.17|255.255.255.240|
|f6/0|10.0.0.33|255.255.255.240|
|f7/0|10.0.0.49|255.255.255.240|

#### Para el Router Escuintla

|Interfaz|IP Red|Mascara|
|:----|:----|:----|
|f0/0|10.0.0.2|255.255.255.240|
|f1/0|10.0.0.65|255.255.255.240|
|f6/0|10.0.0.81|255.255.255.240|
|f7/0|10.0.0.97|255.255.255.240|

#### Para el Router Jutiapa

|Interfaz|IP Red|Mascara|
|:----|:----|:----|
|f0/0|10.0.0.113|255.255.255.240|
|f1/0|10.0.0.66|255.255.255.240|
|f6/0|10.0.0.129|255.255.255.240|
|f7/0|10.0.0.34|255.255.255.240|

#### Para el Router Peten

|Interfaz|IP Red|Mascara|
|:----|:----|:----|
|f0/0|10.0.0.114|255.255.255.240|
|f1/0|10.0.0.145|255.255.255.240|
|f6/0|10.0.0.82|255.255.255.240|
|f7/0|10.0.0.50|255.255.255.240|

#### Para el Router Quiche

|Interfaz|IP Red|Mascara|
|:----|:----|:----|
|f0/0|10.0.0.146|255.255.255.240|
|f1/0|10.0.0.50|255.255.255.240|
|f6/0|10.0.0.130|255.255.255.240|
|f7/0|10.0.0.198|255.255.255.240|

### 🔩 Configuración de la Sede Jutiapa<div id="configuracion-jutiapa"></div>

#### Para el SW2 <div id="sw2"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname SW2
    do w
    ```

* Configuración del modo truncal

    ```console
    int range f0/4-6
    switchport mode trunk
    exit
    do w
    ```

* Configuración del protocolo

    ```console
    vtp mode client
    vtp domain P32
    vtp password usac
    do w
    ```

* Configuración del modo de acceso

    La asignación de VLANs se dio arbitrariamente

    * Para PC2

        ```console
        int f0/1
        switchport mode access
        switchport access vlan 12
        do w
        ```

    * Para PC3

        ```console
        int f0/2
        switchport mode access
        switchport access vlan 22
        do w
        ```

    * Para PC4

        ```console
        int f0/3
        switchport mode access
        switchport access vlan 32
        exit
        do w
        ```

* Configuracion del STP (RPVST)

    ```console
    spanning-tree mode rapid-pvst
    exit
    ```

#### Para el SW3 <div id="sw3"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname SW3
    do w
    ```

* Configuración del modo truncal

    ```console
    int range f0/4-6
    switchport mode trunk
    exit
    do w
    ```

* Configuración del protocolo

    ```console
    vtp mode client
    vtp domain P32
    vtp password usac
    do w
    ```

* Configuración del modo de acceso

    La asignación de VLANs se dio arbitrariamente

    * Para PC5

        ```console
        int f0/1
        switchport mode access
        switchport access vlan 42
        do w
        ```

    * Para PC6

        ```console
        int f0/2
        switchport mode access
        switchport access vlan 12
        do w
        ```

    * Para PC7

        ```console
        int f0/3
        switchport mode access
        switchport access vlan 22
        exit
        do w
        ```

* Configuracion del STP (RPVST)

    ```console
    spanning-tree mode rapid-pvst
    exit
    ```

#### Para el ESW1 <div id="esw1"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname ESW1
    do w
    ```

* Configuración del modo truncal

    ```console
    int range f0/1-4
    switchport trunk encapsulation dot1q
    switchport mode trunk
    no shutdown
    exit
    do w
    ```

* Configuración del protocolo

    ```console
    vtp version 2
    vtp mode server
    vtp domain P32
    vtp password usac
    do w
    ```

* Creación de VLANs

    ```console
    vlan 12
    name RRHH
    vlan 22
    name CONTABILIDAD
    vlan 32
    name VENTAS
    vlan 42
    name INFORMATICA
    exit
    do w
    ```

* Configuracion de las VLANs con STP

    ```console
    spanning-tree vlan 1 root primary
    spanning-tree vlan 12 root primary
    spanning-tree vlan 22 root primary
    spanning-tree vlan 32 root primary
    spanning-tree vlan 42 root primary
    ```

* Configuración de las VLANs como Switch Virtual Interface (SVI)

    ```console
    int vlan 35
    ip address 192.168.29.1 255.255.255.224
    no shutdown
    exit

    int vlan 45
    ip address 192.168.29.33 255.255.255.240
    no shutdown
    exit

    int vlan 15
    ip address 192.168.29.49 255.255.255.240
    no shutdown
    exit

    int vlan 25
    ip address 192.168.29.65 255.255.255.248
    no shutdown
    exit
    ```

* Configuracion del STP (RPVST)

    ```console
    spanning-tree mode rapid-pvst
    exit
    ```

#### Para el J1 <div id="j1"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname J1
    do w
    ```

* Configuración HSRP

    ```console
    int f1/0
    standby 10 ip 192.167.29.1
    standby 10 priority 150
    standby 10 preempt
    no shutdown
    do w
    exit
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 11.0.0.1 255.255.255.0
    no shutdown
    do w
    ```

#### Para el J2 <div id="j2"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname J2
    do w
    ```

* Configuración HSRP

    ```console
    int f1/0
    standby 10 ip 192.167.29.1
    no shutdown
    do w
    exit
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 12.0.0.1 255.255.255.0
    no shutdown
    do w
    ```

### 🔩 Configuración de la Sede Escuintla<div id="configuracion-escuintla"></div>

#### Para el SW0 <div id="sw0"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname SW0
    do w
    ```

* Configuración del modo truncal

    ```console
    int f0/1
    switchport mode trunk
    exit
    do w
    ```

* Creación de VLANs

    ```console
    vlan 12
    name RRHH
    vlan 32
    name VENTAS
    exit
    do w
    ```

* Configuración del modo de acceso

    > La asignación de VLANs se dio arbitrariamente

    * Para PC0

        ```console
        int f0/2
        switchport mode access
        switchport access vlan 12
        do w
        exit
        ```

    * Para PC1

        ```console
        int f0/3
        switchport mode access
        switchport access vlan 32
        do w
        exit
        ```

* Configuracion del STP (RPVST)

    ```console
    spanning-tree mode rapid-pvst
    exit
    ```

### 🔩 Configuración del Core<div id="configuracion-core"></div>

#### Para el Router CENTRAL <div id="router-central"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname CENTRAL
    do w
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 10.0.0.1 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f1/0
    ip address 10.0.0.17 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f6/0
    ip address 10.0.0.33 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f7/0
    ip address 10.0.0.49 255.255.255.240
    no shutdown
    exit
    do w
    ```

#### Para el Router ESCUINTLA <div id="router-escuintla"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname ESCUINTLA
    do w
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 10.0.0.2 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f1/0
    ip address 10.0.0.65 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f6/0
    ip address 10.0.0.81 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f7/0
    ip address 10.0.0.97 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f8/0
    no shutdown
    exit
    do w
    ```

#### Para el Router JUTIAPA <div id="router-jutiapa"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname JUTIAPA
    do w
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 10.0.0.113 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f1/0
    ip address 10.0.0.66 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f6/0
    ip address 10.0.0.129 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f7/0
    ip address 10.0.0.34 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f8/0
    ip address 11.0.0.2 255.255.255.0
    no shutdown
    exit
    do w
    ```

    ```console
    int f9/0
    ip address 12.0.0.2 255.255.255.0
    no shutdown
    exit
    do w
    ```

#### Para el Router PETEN <div id="router-peten"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname PETEN
    do w
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 10.0.0.114 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f1/0
    ip address 10.0.0.145 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f6/0
    ip address 10.0.0.82 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f7/0
    ip address 10.0.0.50 255.255.255.240
    no shutdown
    exit
    do w
    ```

#### Para el Router QUICHE <div id="router-quiche"></div>

* Configuración inicial

    ```console
    enable
    conf t
    no ip domain-lookup
    hostname QUICHE
    do w
    ```

* Configuración de la IP y mascara en cada puerto correspondiente

    ```console
    int f0/0
    ip address 10.0.0.146 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f1/0
    ip address 10.0.0.50 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f6/0
    ip address 10.0.0.130 255.255.255.240
    no shutdown
    exit
    do w
    ```

    ```console
    int f7/0
    ip address 10.0.0.198 255.255.255.240
    no shutdown
    exit
    do w
    ```

### 💻 Configuración de VPCs <div id="configuracion-vpcs"></div>

#### Para el VPC0 (VLAN 12)<div id="vpc0"></div>

* **IPv4 Address**: 192.148.29.34
* **Subnet Mask**: 255.255.255.248
* **Default Gateway**: 192.148.29.33
* **DNS Server**: 0.0.0.0

#### Para el VPC1 (VLAN 32) <div id="vpc1"></div>

* **IPv4 Address**: 192.148.29.2
* **Subnet Mask**: 255.255.255.224
* **Default Gateway**: 192.148.29.1
* **DNS Server**: 0.0.0.0

#### Para el VPC2 (VLAN 12) <div id="vpc2"></div>

* **IPv4 Address**: 192.168.29.50
* **Subnet Mask**: 255.255.255.240
* **Default Gateway**: 192.168.29.49
* **DNS Server**: 0.0.0.0

#### Para el VPC3 (VLAN 22) <div id="vpc3"></div>

* **IPv4 Address**: 192.168.29.66
* **Subnet Mask**: 255.255.255.248
* **Default Gateway**: 192.168.29.65
* **DNS Server**: 0.0.0.0

#### Para el VPC4 (VLAN 32) <div id="vpc4"></div>

* **IPv4 Address**: 192.168.29.2
* **Subnet Mask**: 255.255.255.224
* **Default Gateway**: 192.168.29.1
* **DNS Server**: 0.0.0.0

#### Para el VPC5 (VLAN 42) <div id="vpc5"></div>

* **IPv4 Address**: 192.168.29.34
* **Subnet Mask**: 255.255.255.240
* **Default Gateway**: 192.168.29.33
* **DNS Server**: 0.0.0.0

#### Para el VPC6 (VLAN 12) <div id="vpc6"></div>

* **IPv4 Address**: 192.168.29.51
* **Subnet Mask**: 255.255.255.240
* **Default Gateway**: 192.168.29.49
* **DNS Server**: 0.0.0.0

#### Para el VPC7 (VLAN 22) <div id="vpc7"></div>

* **IPv4 Address**: 192.168.29.67
* **Subnet Mask**: 255.255.255.248
* **Default Gateway**: 192.168.29.65
* **DNS Server**: 0.0.0.0


### PROTOCOLOS DE ENRUTAMIENTO DINAMICO 🕸👀

#### QUICHE EIGRP 
```
router eigrp 1
```
```
network 192.178.29.0 0.0.0.63
network 192.178.29.64 0.0.0.31
network 192.178.29.96 0.0.0.15
network 192.178.29.112 0.0.0.15
```
```
network 10.0.0.146 0.0.0.15
network 10.0.0.50 0.0.0.15
network 10.0.0.130 0.0.0.15
network 10.0.0.198 0.0.0.15
```
```
no auto-summary
```

#### PETEN EIGRP 
```
router eigrp 1
```
```
network 192.158.29.0
network 192.158.29.32
network 192.158.29.64
```
```
network 10.0.0.114 0.0.0.15
network 10.0.0.145 0.0.0.15
network 10.0.0.82 0.0.0.15
network 10.0.0.50 0.0.0.15

```
```
no auto-summary
```

### ESCUINTLA OSPF
```
router ospf 2

network 192.148.29.0 0.0.0.31 area 1
network 192.148.29.32 0.0.0.7 area 1

network 10.0.0.2 0.0.0.15 area 1
network 	10.0.0.65 0.0.0.15 area 1
network 	10.0.0.81 0.0.0.15 area 1
network 10.0.0.97 0.0.0.15 area 1

do show ip protocol
do show ip route
```

### JUTIAPA OSPF
```
router ospf 2

network 192.168.29.0 0.0.0.31 area 1
network 192.168.29.32 0.0.0.15 area 1
network 192.168.29.48 0.0.0.15 area 1
network 192.168.29.64 0.0.0.7 area 1

network 10.0.0.113 0.0.0.15 area 1
network 	10.0.0.66 0.0.0.15 area 1
network 	10.0.0.129 0.0.0.15 area 1
network 10.0.0.34 0.0.0.15 area 1

do show ip protocol
do show ip route
```

### CENTRAL OSPF
```
router ospf 2

network 10.0.0.1 0.0.0.15 area 1
network 	10.0.0.17 0.0.0.15 area 1
network 	10.0.0.33 0.0.0.15 area 1
network 10.0.0.49 0.0.0.15 area 1

do show ip protocol
do show ip route
```
