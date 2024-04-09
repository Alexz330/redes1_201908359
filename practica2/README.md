# Practica 2 Alexis Marco Tulio López Cacoj 
# Carnet 201908359

x = 9
 
| Dispositivo| Interfaz| IP| Máscara de red subred|
| :------------ | :-----------: | ------------: |  ------------: |
| R1   |s1/0| 10.0.0.1 |/30 |
|   | e0/0    | 192.168.1.2 |/29|
|   | e0/1 | 192.168.2.2|/29|s
|R2|e0/0| 192.168.1.1|/29|
||e0/1| 192.168.0.2|/24|
|R3|e0/0| 192.168.2.1|/29|
||e0/1| 192.168.0.3|/24|
|R2-R3|Virtual| 192.168.0.1|/24|
|VPC11|eth0| 192.168.0.4|/24|
|R4|s0/0| 10.0.0.1|/30|
||e0/0| 192.178.1.1|/29|
||e0/1| 192.178.2.1|/29|
|R5|e0/0|192.178.1.2|/29|
||e0/1|192.178.0.2|/24|
|R6|e0/0|192.178.2.2|/29|
||e0/1|192.178.0.3|/24|
|R5-R6|Virtual| 192.178.0.1|/24|
|VPC12|eth0| 192.178.0.4|/24|


## Configuracion de Router 1, 2, 5

### Router 1

```
enable
conf t
no ip domain-lookup
hostname R1
```
```
int se0/0
ip add 10.0.0.1 255.255.255.252
no shut
exit
```
```
int f0/0
ip add 192.168.1.2 255.255.255.248
no shut
```
```
int f0/1
ip add 192.168.2.2 255.255.255.248
no shut

do w
```
### Router 2
```
enable
conf t
no ip-domain-lookup
hostname R2
```
```
int f0/1
ip add 192.168.0.2 255.255.255.0
standby 11 ip 192.168.0.1
standby 11 priority 150
standby 11 preempt
no shut
do w
```
```
int f0/0
ip add 192.168.1.1 255.255.255.248
no shut
do w
```

### Router 5
```
enable
conf t
no ip-domain-lookup
hostname R5
```
```
int f0/1
ip add 192.178.0.2 255.255.255.0
standby 12 ip 192.178.0.1
standby 12 priority 150
standby 12 preempt
no shut
exit
```
```
int f0/0
ip add 192.178.1.2 255.255.255.248
no shut
do w
```
### Configuiracion de VPC11
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/8ad6089d-1cb5-47dd-b2bb-e62fa99aa32d)

### Creación de rutas estaticas
```
######################### R1 ip estatica ####################

enable
conf t
ip route 192.168.0.0 255.255.255.0 192.168.1.1
ip route 192.168.0.0 255.255.255.0 192.168.2.1
ip route 192.168.1.0 255.255.255.248 192.168.1.1
ip route 192.168.2.0 255.255.255.248 192.168.2.1
ip route 10.0.0.0 255.255.255.252 10.0.0.2
ip route 192.178.1.0 255.255.255.248 10.0.0.2
ip route 192.178.2.0 255.255.255.248 10.0.0.2
ip route 192.178.0.0 255.255.255.0 10.0.0.2
do w
```
```
######################### R2 ip estatica ####################
enable
conf t
ip route 192.168.1.0 255.255.255.248 192.168.1.2
ip route 192.168.2.0 255.255.255.248 192.168.1.2
ip route 10.0.0.0 255.255.255.252 192.168.1.2
ip route 192.178.1.0 255.255.255.248 192.168.1.2
ip route 192.178.2.0 255.255.255.248 192.168.1.2
ip route 192.178.0.0 255.255.255.0 192.168.1.2

do w
```
```
######################### R3 ip estatica ####################


enable
conf t
ip route 192.168.2.0 255.255.255.248 192.168.2.2
ip route 192.168.1.0 255.255.255.248 192.168.2.2
ip route 10.0.0.0 255.255.255.252 192.168.2.2
ip route 192.178.1.0 255.255.255.248 192.168.2.2
ip route 192.178.2.0 255.255.255.248 192.168.2.2
ip route 192.178.0.0 255.255.255.0 192.168.2.2
do w
```
```
######################### R4 ip estatica ####################

enable
conf t

ip route 192.178.0.0 255.255.255.0 192.178.2.2
ip route 192.178.0.0 255.255.255.0 192.178.1.2
ip route 192.178.1.0 255.255.255.248 192.178.1.2
ip route 192.178.2.0 255.255.255.248 192.178.2.2
ip route 10.0.0.0 255.255.255.252 10.0.0.1
ip route 192.168.0.0 255.255.255.0 10.0.0.1
ip route 192.168.1.0 255.255.255.248 10.0.0.1
ip route 192.168.2.0 255.255.255.248 10.0.0.1
do w
```
```
######################### R5 ip estatica ####################
enable
conf t
ip route 192.178.1.0 255.255.255.248 192.178.1.1
ip route 192.178.2.0 255.255.255.248 192.178.1.1
ip route 192.168.0.0 255.255.255.0 192.178.1.1
ip route 192.168.1.0 255.255.255.248 192.178.1.1
ip route 192.168.2.0 255.255.255.248 192.178.1.1
ip route 10.0.0.0 255.255.255.252 192.178.1.1
do w
```
```
######################### R6 ip estatica ####################

enable
conf t

ip route 192.178.2.0 255.255.255.248 192.178.2.1
ip route 192.178.1.0 255.255.255.248 192.178.2.1
ip route 10.0.0.0 255.255.255.252 192.178.2.1
ip route 192.168.0.0 255.255.255.0 192.178.2.1
ip route 192.168.1.0 255.255.255.248 192.178.2.1
ip route 192.168.2.0 255.255.255.248 192.178.2.1
do w
```

### PortChannel con PAGP y LACP,
```
######################### Switch 0#############################
enable
conf t
int range fa0/2-3
channel-protocol pagp
channel-group 1 mode desirable
do w

```
```
######################### Switch 1#############################
enable
conf t
int range fa0/1-2
channel-protocol pagp
channel-group 1 mode desirable
do w
```
```
######################### Switch 2#############################
enable
conf t
int range fa0/4-5
channel-protocol lacp
channel-group 1 mode active
do w

```
```
######################### Switch 3#############################
enable
conf t
int range fa0/4-5
channel-protocol lacp
channel-group 1 mode active
do w
```

### configuración de VPC 
```
ip:192.168.0.4 maks:255.255.255.0 gateway:192.168.0.1
ip:192.178.0.4 maks:255.255.255.0 gateway:192.178.0.1
```

## Validacion switches

### SW0
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/7e74f824-58c9-4252-9df4-61e16312c338)
### SW1
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/51e12a59-f214-4288-82a3-23179d4f35f2)
### SW2
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/2c3ba7bb-78df-4707-acd8-b05ace54df16)
### SW3
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/dc67e794-7090-42c6-8fe1-abc855440910)

## Validacion de routes

### R3
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/71995d8d-0f66-4963-af94-a4625f41ae8f)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/531be8e3-1150-4471-9c81-d483e7d8f670)

### R2
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/9ae935c1-6594-49d2-8189-e24cccad5194)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/c4e50570-f347-4ef7-a466-58e92e64e387)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/291791eb-1912-4efb-bb1f-08a8607db9d4)

### R1
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/d70fd867-7e23-4fc8-b450-85c364c8f821)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/95169291-0194-47c3-807b-09f17a9a4a61)

### R4
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/3cd4f0f0-6f88-4d02-808f-bfab5f6c383e)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/9aa54253-7b54-46b7-9328-f4b659b02764)

### R5
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/ee9fb001-4eff-473e-8de6-c83873468fce)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/5aff741b-f54e-4894-ba65-6877f2f8e6f3)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/240e3c44-8327-4ccf-96c4-a510b148653a)
### R6
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/44bea2d0-599b-4a83-a018-87ac7c102bbc)
![image](https://github.com/Alexz330/redes1_201908359/assets/72354711/45c40104-b7a7-4180-adf5-68537a3de749)









