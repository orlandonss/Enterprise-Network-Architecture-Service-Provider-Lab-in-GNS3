# Attention

 it may have some mistakes, so take only as a model to make the topology / the project for TL,
 as u can see all the commands are simillar, just
 the vlans and the ip address changes!!!

 btw i forgot to do some commands, so if u need help call or text me!!!

# TOPOLOGIA DE TECNOLOGIAS DE LIGACAO


## IPTERMS
```bash
mkdir -p ~/.ssh

printf "Host 194.65.* 192.168.0.* 10.0.0.*\n    KexAlgorithms +diffie-hellman-group14-sha1,diffie-hellman-group1-sha1\n    HostKeyAlgorithms +ssh-rsa\n    Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc\n    GSSAPIAuthentication no\n    StrictHostKeyChecking no\n" > ~/.ssh/config

chmod 600 ~/.ssh/config

```

## Global (All Devices)

```bash
write erase
reload
en
conf t
banner #Cuidado acesso restrito!!!! Apenas a pessoas autorizadas#
hostname <nameof the equipment>
line console 0
logging synchronous
exec-timeout 0


service password-encryption

enable secret cisco

ip domain-name manager.com

username admin secret cisco

ip ssh version 2

crypto key generate rsa modulus 1024


line console 0
logging synchronous
exec-timeout 0 
exit

line vty 0 
transport input ssh
login local
exec-timeout 0 

do wr
```
## Global (all interfaces that has terminal connected expet the pvlans)

```bash
int e<number/number>//host interfaces
spanning-tree portfast
spanning-tree bpduguard enable

switchport port-security
switchport port-security maximum 2
switchport port-security violation restrict
switchport port-security mac-address sticky

int e<number/number> //trunk interfaces
spanning-tree guard loop

```
## Global (all routers / switchesL3)

```bash
Kacper if u are here, ask my help !!!
this authentication need to be applied in the 
interfaces from the virtual lan, subinterfaces from routers and "no switchports" in the switches.


ip ospf message-digest-key 1 md5 cisco
ip ospf authentication message-digest

```

## ROUTER ISP

```bash

interface e0/0
ip address dhcp
ip nat outside
no shut

interface e0/2
ip nat inside
ip address 10.30.97.238 255.255.255.252
bandwidth 100
no shut


interface e0/1
ip nat inside
bandwidth 1000  
ip address 10.30.97.242 255.255.255.252
no shut


access-list 10 permit any
ip nat inside source list 10 int e0/0 overload


interface e0/3
ip nat inside
ip address 2.2.2.1 255.255.255.0
description Link para VPS de Testes 2.2.2.2
no shut

router ospf 1
 network 2.2.2.0 0.0.0.255 area 0
 network 10.30.97.236 0.0.0.3 area 0
 network 10.30.97.240 0.0.0.3 area 0
 default-information originate


```

# CAMPUS DE ENGENHARIA (CE)

## Switch & Router Configuration

### SW1-ce

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 10
 switchport mode access

 interface Ethernet1/0
 no shut
 switchport access vlan 10
 switchport mode access

interface Ethernet0/2
 no shut
 switchport access vlan 11
 switchport mode access

interface Ethernet0/3
 no shut
 switchport access vlan 12
 switchport mode access
```
<>

### SW2-ce

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk


interface Ethernet0/1
 no shut
 switchport access vlan 13
 switchport mode access

```

### SW3-ce

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/0 
 no shut
 switchport access vlan 14
 switchport mode access
```

### SW4-ce

```bash

interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 15
 switchport mode access

```

### R1-ce

```bash
interface e0/1
 no shut

interface e0/1.10
 encapsulation dot1q 10
 ip address 194.65.18.254 255.255.255.0

interface e0/1.11
 encapsulation dot1q 11
 ip address  194.65.23.126 255.255.255.224

interface e0/1.12
 encapsulation dot1q 12
 ip address 194.65.23.158 255.255.255.224

interface e0/2
 no shut  

interface e0/2.13
 encapsulation dot1q 13
 ip address 194.65.25.30 255.255.255.240

interface e0/0
ip addres 192.168.0.14 255.255.255.252
no shut

router ospf 1
 network 192.168.0.12 0.0.0.3 area 0
 network 194.65.18.0 0.0.0.255 area 0
 network 194.65.23.96 0.0.0.31 area 0
 network 194.65.23.128 0.0.0.31 area 0
 network 194.65.25.16 0.0.0.15 area 0
```

### R2-ce

```bash
interface e0/1
 no shut

interface e0/1.14
 encapsulation dot1q 14
 ip address 194.65.25.46 255.255.255.240

interface e0/0
ip addres 192.168.0.18 255.255.255.252
no shut

router ospf 1
 network 192.168.0.16 0.0.0.3 area 0
 network 194.65.25.32 0.0.0.15 area 0

```

### R3-ce

```bash
interface e0/1
 no shut

interface e0/1.15
 encapsulation dot1q 15
 ip address 194.65.25.230 255.255.255.248

    interface e0/0
    ip addres 192.168.0.22 255.255.255.252
    no shut
    router ospf 1
 network 192.168.0.20 0.0.0.3 area 0
 network 194.65.24.224 0.0.0.7 area 0
```

### R-CE

```bash
   en  
    conf t
    interface e0/0
    ip addres 192.168.0.13 255.255.255.252
    no shut

    interface e0/1
    ip addres 192.168.0.17 255.255.255.252
    no shut
  
    interface e0/2
    ip addres 192.168.0.21 255.255.255.252
    no shut

 router ospf 1
 network 192.168.0.12 0.0.0.3 area 0
 network 192.168.0.16 0.0.0.3 area 0
 network 192.168.0.20 0.0.0.3 area 0
 network 10.0.0.4 0.0.0.3 area 0

 int s2/0
   encapsulation frame-relay 
   no shut

   interface s2/0.301 point-to-point
  description SEDE-CE
  ip address 10.0.0.6 255.255.255.252
  frame-relay interface-dlci 301

```

# CAMPUS CIENCIA E SAUDE (CCS)

## Switches/Routers

### SW1-ccs

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 20
 switchport mode access

interface Ethernet0/2
 switchport access vlan 21
 switchport mode access

interface Ethernet0/3
 switchport access vlan 22
 switchport mode access
```

### SW2-ccs

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 23
 switchport mode access

```

### SW3-ccs

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 24
 switchport mode access
```

### SW4-ccs

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 25
 switchport mode access

 interface Ethernet0/2
 switchport access vlan 25
 switchport mode access

```

### R1-ccs

```bash

interface e0/1
 no shut

interface e0/1.20
 encapsulation dot1q 20 
 ip address 194.65.23.62 255.255.255.224

interface e0/1.21
 encapsulation dot1q 21
 ip address 194.65.23.94 255.255.255.224

interface e0/1.12
 encapsulation dot1q 22
 ip address 194.65.24.254 255.255.255.240

interface e0/2
 no shut  

interface e0/2.23
 encapsulation dot1q 23
 ip address 194.65.25.14 255.255.255.240

router ospf 1
network 194.65.23.32 0.0.0.31 area 0
network 194.65.23.64 0.0.0.31 area 0
network 194.65.24.240 0.0.0.15 area 0
network 194.65.25.0 0.0.0.15 area 0 
network 192.168.0.0 0.0.0.3 area 0 

```

### R2-css

```bash
interface e0/1
 no shut 

interface e0/1.24
 encapsulation dot1q 24
 ip address 194.65.25.214 255.255.255.248

 router ospf 1
network 194.65.25.208 0.0.0.7 area 0
 network 192.168.0.4 0.0.0.3 area 0
```

### R3-ccs

```bash

interface e0/1
 no shut

interface e0/1.25
 encapsulation dot1q 25
 ip address 194.65.25.222 255.255.255.248


 router ospf 1
 network 194.65.25.216 0.0.0.7 area 0
 network 192.168.0.8 0.0.0.3 area 0
```

### R-CCS

```bash
    en  
    conf t
    interface e0/0
    ip addres 192.168.0.1 255.255.255.252
    no shut

    interface e0/1
    ip addres 192.168.0.5 255.255.255.252
    no shut
  
    interface e0/2
    ip addres 192.168.0.9 255.255.255.252
    no shut

     router ospf 1
     network 192.168.0.0 0.0.0.3 area 0
     network 192.168.0.4 0.0.0.3 area 0
     network 192.168.0.8 0.0.0.3 area 0
     network 10.0.0.0 0.0.0.3 area 0

    int s2/0
    encapsulation frame-relay 
    no shut

  interface s2/0.201 point-to-point
  description SEDE-CCS
  ip address 10.0.0.2 255.255.255.252
  frame-relay interface-dlci 201

```

# ESCOLA DE CIENCIAS MATEMATICA APLICADA (ECMA)

## Switches/Routers

### SW1-ecma

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 30
 switchport mode access

interface Ethernet0/2
 switchport access vlan 31
 switchport mode access

interface Ethernet0/3
 switchport access vlan 32
 switchport mode access
```

### SW2-ecma

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 33
 switchport mode access

```

### SW3-ecma

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 34
 switchport mode access
```

### SW4-ecma

```bash
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 switchport access vlan 35
 switchport mode access

```

### R1-ecma

```bash

interface e0/1
no shut

interface e0/1.30
encapsulation dot1q 30
ip address 194.65.25.94 255.255.255.240

interface e0/1.31
encapsulation dot1q 31
ip address 194.65.25.110 255.255.255.240

interface e0/1.32
encapsulation dot1q 32
ip address 194.65.25.126 255.255.255.240

interface e0/2
no shut  

interface e0/2.33
encapsulation dot1q 33 
ip address  194.65.26.6 255.255.255.248

interface e0/0
ip address  192.168.0.50 255.255.255.252
no shut

router ospf 1
 network 192.168.0.48 0.0.0.3 area 0
 network 194.65.25.80 0.0.0.15 area 0
 network 194.65.25.96 0.0.0.15 area 0
 network 194.65.25.112 0.0.0.15 area 0
 network 194.65.26.0 0.0.0.7 area 0

```

### R2-ecma

```bash
interface e0/1
 no shut 

interface e0/1.34  
 encapsulation dot1q 34 
 ip address 194.65.26.34 255.255.255.252

 interface e0/0
ip address  192.168.0.54 255.255.255.252
no shut

router ospf 1
 network 192.168.0.52 0.0.0.3 area 0
 network 194.65.26.32 0.0.0.3 area 0
```

### R3-ecma

```bash

interface e0/1
 no shut

interface e0/1.35
 encapsulation dot1q 35 
 ip address 194.65.26.38 255.255.255.252

  interface e0/0
ip address  192.168.0.58 255.255.255.252
no shut

router ospf 1
 network 192.168.0.56 0.0.0.3 area 0
 network 194.65.26.36 0.0.0.3 area 0
```

### R-ECMA

```bash
    en  
    conf t
 
    interface e0/0
    ip addres 192.168.0.49 255.255.255.252
    no shut

    interface e0/1
    ip addres 192.168.0.53 255.255.255.252
    no shut
  
    interface e0/2
    ip addres 192.168.0.57 255.255.255.252
    no shut

   interface e0/3
    ip address 192.168.0.130 255.255.255.252
    no shut
   

   router ospf 1
   network 192.168.0.48 0.0.0.3 area 0
   network 192.168.0.52 0.0.0.3 area 0  
   network 192.168.0.56 0.0.0.3 area 0  
   network 192.168.0.128 0.0.0.3 area 0
   network 10.0.0.28 0.0.0.3 area 0
    network 10.0.0.36 0.0.0.3 area 0


   interface s2/0
   encapsulation frame-relay
   no shut

   interface Serial2/1
   no ip address
   encapsulation frame-relay
   serial restart-delay 0

interface Serial2/2
 no ip address
 encapsulation frame-relay
 serial restart-delay 0

interface Serial2/1.201 point-to-point
 frame-relay interface-dlci 201 ppp Virtual-Template1

   interface s2/0.301 point-to-point
   description ecma-SEDEf1
   ip address 10.0.0.34 255.255.255.252
   frame-relay interface-dlci 301

   interface Serial2/2.301 point-to-point
 frame-relay interface-dlci 301 ppp Virtual-Template1

   interface Virtual-Template1
 description Link Multilink para R-RE3
 ip address 10.0.0.37 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 ppp authentication chap
 ppp chap hostname ECMA
 ppp chap password 7 cisco
 ppp multilink






```

### ECMA-L3
```bash

hostname ECMA-L3
ip routing
vlan 89
!
! 1. Porta para o Router (Uplink - TRUNK)
interface Ethernet0/0
 description Uplink_para_Router
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 1,89
 no ip address
 no shutdown
!
! 2. Porta para o PC (Acesso)
interface Ethernet0/2
 description PC_ECMA
 switchport mode access
 switchport access vlan 89
 no shutdown
!
! 3. Gateway da Rede 89 (SVI - IP .21)
interface Vlan 89
 ip address 194.65.26.21 255.255.255.248
 no shutdown
!
end
write

```

# CENTRO DE INVESTIGAÇÃO E DESENVOLVIMENTO (CID)

## Switches/Routers

### SW1-CID

```bash
interface Ethernet0/0
 no shut
 switchport trunk allowed vlan 61-63
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 61
 switchport mode access
 interface Ethernet1/0
 no shut
 switchport access vlan 61
 switchport mode access

interface Ethernet0/2
 no shut
 switchport access vlan 62
 switchport mode access

interface Ethernet0/3
 no shut
 switchport access vlan 63
 switchport mode access
```

### SW2-CID

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 64
 switchport mode access

```

### SW3-CID

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 65
 switchport mode access

```

### SW4-CID

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 66
 switchport mode access 

```

### R1-CID

```bash
interface e0/0
no shut
ip address
ip address 192.168.0.62 255.255.255.252
do wr

interface e0/1
 no shut

interface e0/1.61
 encapsulation dot1q 61
 ip address 194.65.22.126 255.255.255.128

interface e0/1.62
 encapsulation dot1q 62
 ip address  194.65.22.190 255.255.255.192

interface e0/1.63
 encapsulation dot1q 63
 ip address 194.65.23.190 255.255.255.224

interface e0/2
 no shut  

interface e0/2.64
 encapsulation dot1q 64
 ip address 194.65.23.222 255.255.255.224

 router ospf 1
 network 192.168.0.60 0.0.0.3 area 0
 network 194.65.22.0 0.0.0.127 area 0
 network 194.65.22.128 0.0.0.63 area 0
 network 194.65.23.160 0.0.0.31 area 0
 network 194.65.23.192 0.0.0.31 area 0

```

### R2-CID

```bash
interface e0/1
 no shut

interface e0/1.65
 encapsulation dot1q 65
 ip address 194.65.23.254 255.255.255.224

interface e0/0
ip addres 192.168.0.66 255.255.255.252
no shut

router ospf 1
 network 192.168.0.64 0.0.0.3 area 0
 network 194.65.23.224 0.0.0.31 area 0
```

### R3-CID

```bash
interface e0/1
 no shut

interface e0/1.66
 encapsulation dot1q 66
 ip address 194.65.23.30 255.255.255.224

interface e0/0
ip addres 192.168.0.70 255.255.255.252
no shut

router ospf 1
 network 192.168.0.68 0.0.0.3 area 0
 network 194.65.24.0 0.0.0.31 area 0

```

### R-CID

```bash
    en  
    conf t
    interface e0/3
    ip address 10.30.97.237 255.255.255.252
    bandwidth 100
    no shut

  
    interface e0/0
    ip address 192.168.0.61 255.255.255.252
    no shut

    interface e0/1
    ip address 192.168.0.65 255.255.255.252
    no shut
  
    interface e0/2
    ip address 192.168.0.69 255.255.255.252
    no shut

    router ospf 1
    network 192.168.0.60 0.0.0.3 area 0
    network 192.168.0.64 0.0.0.3 area 0
    network 192.168.0.68 0.0.0.3 area 0
    network 10.0.0.16 0.0.0.3 area 0
    network 10.0.0.20 0.0.0.3 area 0
    default-information originate

  int s2/1
  no shut
  encapsulation frame-relay

  int s2/2
  no shut
  encapsulation frame-relay

  interface s2/1.301 point-to-point
  description CID-SEDE
  ip address 10.0.0.18 255.255.255.252
  frame-relay interface-dlci 301

interface s2/2.201 point-to-point
description CID-SEDEF1
ip address 10.0.0.22 255.255.255.252
frame-relay interface-dlci 201
  

```

# CENTRO DE MANUTENÇÃO E LOGÍSTICA (CML)

## Switches/routers

### SW1-CML

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 41
 switchport mode access

 interface Ethernet1/0
 no shut
 switchport access vlan 42
 switchport mode access

interface Ethernet0/2
 no shut
 switchport access vlan 42
 switchport mode access

interface Ethernet0/3
 no shut
 switchport access vlan 43
 switchport mode access
```

### SW2-CML

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 44
 switchport mode access

```

### SW3-CML

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 45
 switchport mode access

```

### SW4-CML

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 46
 switchport mode access

```

### R1-CML

```bash
interface e0/0
no shut
ip address 192.168.0.74 255.255.255.252


interface e0/1
 no shut

interface e0/1.41
 encapsulation dot1q 41 
 ip address 194.65.24.62 255.255.255.224

interface e0/1.42
 encapsulation dot1q 42
 ip address 194.65.25.62 255.255.255.240

interface e0/1.43
 encapsulation dot1q 43
 ip address 194.65.25.78 255.255.255.240

interface e0/2
 no shut  

interface e0/2.44
 encapsulation dot1q 44
 ip address 194.65.25.238 255.255.255.248

 
 router ospf 1
 network 192.168.0.72 0.0.0.3 area 0
 network 194.65.24.32 0.0.0.31 area 0
 network 194.65.25.48 0.0.0.15 area 0
 network 194.65.25.64  0.0.0.15 area 0
 network 194.65.25.232 0.0.0.7 area 0

```

### R2-CML

```bash
interface e0/0
ip addres 192.168.0.78 255.255.255.252
no shut

interface e0/1
 no shut

interface e0/1.45
 encapsulation dot1q 45
 ip address 194.65.25.246 255.255.255.248

router ospf 1
network 192.168.0.76 0.0.0.3 area 0
network 194.65.25.240 0.0.0.7 area 0

```

### R3-CML

```bash
interface e0/0
ip addres 192.168.0.82 255.255.255.252
no shut

interface e0/1
 no shut

interface e0/1.46
 encapsulation dot1q 46
 ip address 194.65.25.254 255.255.255.248

router ospf 1
network 192.168.0.80 0.0.0.3 area 0
network 194.65.25.248 0.0.0.7 area 0


```

### R-CML

```bash
   en  
    conf t
  
    interface e0/0
    ip address 192.168.0.73 255.255.255.252
    no shut

    interface e0/1
    ip address 192.168.0.77 255.255.255.252
    no shut
  
    interface e0/2
    ip address 192.168.0.81 255.255.255.252
    no shut

   router ospf 1
   network 192.168.0.76 0.0.0.3 area 0
   network 192.168.0.72 0.0.0.3 area 0
   network 192.168.0.80 0.0.0.3 area 0
   network 10.0.0.18 0.0.0.3 area 0

   int s2/0
   encapsulation frame-relay 
   no shut

  interface s2/0.201 point-to-point
  description CML-SEDE
  ip address 10.0.0.14 255.255.255.252
  frame-relay interface-dlci 201


```

# BIBLIOTECA CENTRO DE RECURSOS DIGITAIS (BRCD)

## Switches/routers

### SW1-BCRD

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 51
 switchport mode access

interface Ethernet0/2
 no shut
 switchport access vlan 52
 switchport mode access

interface Ethernet0/3
 no shut
 switchport access vlan 53
 switchport mode access
```

### SW2-BCRD

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 54
 switchport mode access

 interface Ethernet0/2
 no shut
 switchport access vlan 54
 switchport mode access

```

### SW3-BCRD

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 55
 switchport mode access

```

### SW4-BCRD

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 56
 switchport mode access

```

### R1-BCRD

```bash
interface e0/0
no shut
ip address 192.168.0.26 255.255.255.252


interface e0/1
 no shut

interface e0/1.51
 encapsulation dot1q 51
 ip address 194.65.16.254 255.255.255.0

interface e0/1.52
 encapsulation dot1q 52
 ip address 194.65.17.254 255.255.255.0

interface e0/1.53
 encapsulation dot1q 53
 ip address 194.65.23.30 255.255.255.224

interface e0/2
 no shut  

interface e0/2.54
 encapsulation dot1q 54
 ip address 194.65.24.206 255.255.255.240

 router ospf 1
 network 192.168.0.24 0.0.0.3 area 0
 network 194.65.16.0 0.0.0.255 area 0
 network 194.65.17.0 0.0.0.255 area 0
 network 194.65.23.0 0.0.0.31 area 0
 network 194.65.24.192 0.0.0.15 area 0

```

### R2-BCRD

```bash
interface e0/0
ip address 192.168.0.30 255.255.255.252
no shut

interface e0/1
 no shut

interface e0/1.55
 encapsulation dot1q 55
 ip address 194.65.24.222 255.255.255.240

router ospf 1
 network 192.168.0.28 0.0.0.3 area 0
 network 194.65.24.208 0.0.0.15 area 0
```

### R3-BCRD

```bash
interface e0/0
ip address 192.168.0.34 255.255.255.252
no shut
ip ospf message-digest-key 1 md5 cisco
ip ospf authentication message-digest

interface e0/1
 no shut

interface e0/1.56
 encapsulation dot1q 56
 ip address 194.65.24.238 255.255.255.240
 ip ospf message-digest-key 1 md5 cisco
 ip ospf authentication message-digest

 
R-BRCD3(config)# interface Ethernet0/2
R-BRCD3(config-if)# no shutdown
R-BRCD3(config-if)# exit

! Configure the Sub-interface for VLAN 120
R-BRCD3(config)# interface Ethernet0/2.120
R-BRCD3(config-subif)# encapsulation dot1Q 120
R-BRCD3(config-subif)# ip address 192.168.0.142 255.255.255.252

! Configure OSPF Authentication on the Interface
R-BRCD3(config-subif)# ip ospf message-digest-key 1 md5 cisco
R-BRCD3(config-subif)# ip ospf authentication message-digest
R-BRCD3(config-subif)# exit

 router ospf 1
 network 192.168.0.32 0.0.0.3 area 0
 network 194.65.24.224 0.0.0.15 area 0
 network 192.168.0.140 0.0.0.3 area 0
```

### R-BCRD

```bash
    en  
    conf t
  
    interface e0/0
    ip address 192.168.0.25 255.255.255.252
    no shut

    interface e0/1
    ip address 192.168.0.29 255.255.255.252
    no shut
  
    interface e0/2
    ip address 192.168.0.33 255.255.255.252
    no shut

router ospf 1
 network 10.0.0.8 0.0.0.3 area 0
 network 192.168.0.24 0.0.0.3 area 0
 network 192.168.0.28 0.0.0.3 area 0
 network 192.168.0.32 0.0.0.3 area 0

  int s2/0
  no shut
  encapsulation frame-relay

  int s2/1
  no shut
  encapsulation frame-relay

  interface s2/1.401 point-to-point
  description SEDE-BCRD
  ip address 10.0.0.10 255.255.255.252
  frame-relay interface-dlci 401



```

# RESIDENCIA DE ESTUDANTES (RE)

## Switches/routers

### SW1-RE

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 71
 switchport mode access

interface Ethernet0/2
 no shut
 switchport access vlan 72
 switchport mode access

interface Ethernet0/3
 no shut
 switchport access vlan 73
 switchport mode access
```

### SW2-RE

```bash

interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 74
 switchport mode access

```

### SW3-RE

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 75
 switchport mode access

```

### SW4-RE

```bash
interface Ethernet0/0
 no shut
 switchport trunk encapsulation dot1q
 switchport mode trunk

interface Ethernet0/1
 no shut
 switchport access vlan 76
 switchport mode access

```

### R1-RE

```bash
interface e0/0
no shut
ip address 192.168.0.38 255.255.255.252


interface e0/1
 no shut

interface e0/1.71
 encapsulation dot1q 71 
 ip address 194.65.24.126 255.255.255.224

interface e0/1.72
 encapsulation dot1q 72
 ip address 194.65.24.158 255.255.255.224

interface e0/1.73
 encapsulation dot1q 73 
 ip address 194.65.24.190 255.255.255.224

interface e0/2
 no shut  

interface e0/2.74
 encapsulation dot1q 74 
 ip address 194.65.25.174 255.255.255.240

 router ospf 1
 network 192.168.0.36 0.0.0.3 area 0
 network 194.65.24.96 0.0.0.31 area 0
 network 194.65.24.128 0.0.0.31 area 0
 network 194.65.24.160 0.0.0.31 area 0
 network 194.65.25.160 0.0.0.15 area 0

```

### R2-RE

```bash
interface e0/0
ip address 192.168.0.42 255.255.255.252
no shut

interface e0/1
 no shut

interface e0/1.75
 encapsulation dot1q 75
 ip address 194.65.25.190 255.255.255.240

 router ospf 1
 network 192.168.0.40 0.0.0.3 area 0
 network 194.65.25.176 0.0.0.15 area 0


```

### R3-RE

```bash
interface e0/0
ip address 192.168.0.46 255.255.255.252
no shut

interface e0/1
 no shut

interface e0/1.76
 encapsulation dot1q 76
 ip address 194.65.25.206 255.255.255.240

interface Serial2/0
 no ip address
 encapsulation frame-relay
 serial restart-delay 

interface Serial2/0.101 point-to-point
 frame-relay interface-dlci 101 ppp Virtual-Template1

interface Serial2/0.103 point-to-point
 frame-relay interface-dlci 103 ppp Virtual-Template1

interface Virtual-Template1
 description Link Multilink para ECMA
 ip address 10.0.0.38 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 7 121A0C041104
 ppp authentication chap
 ppp chap hostname R-RE3
 ppp chap password 7 01100F175804
 ppp multilink





 router ospf 1
 network 192.168.0.44 0.0.0.3 area 0
 network 194.65.25.192 0.0.0.15 area 0
  network 10.0.0.36 0.0.0.3 area 0
 
 
```

### R-RE

```bash
    en  
    conf t
  
    interface e0/0
    ip address 192.168.0.37 255.255.255.252
    no shut

    interface e0/1
    ip address 192.168.0.41 255.255.255.252
    no shut
  
    interface e0/2
    ip address 192.168.0.45 255.255.255.252
    no shut

    router ospf 1
  
    network 192.168.0.36 0.0.0.3 area 0
    network 192.168.0.40 0.0.0.3 area 0
    network 192.168.0.44 0.0.0.3 area 0
    network 10.0.0.24 0.0.0.3 area 0

  int s2/0
  no shut
  encapsulation frame-relay

  interface s2/0.301 point-to-point
description RE-SEDEF1
ip address 10.0.0.26 255.255.255.252
frame-relay interface-dlci 301


```

# SEDE

## Switches/routers

### SEDE-SW1

```bash

int e0/0
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
no shut
switchport access vlan 82
switchport mode access

int e1/0
no shut
switchport access vlan 82
switchport mode access


```

### SEDE-SW2

```bash
int e0/0
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
no shut
switchport access vlan 83
switchport mode access

int e1/0
no shut
switchport access vlan 84
switchport mode access


```

### SEDE-SW3

```bash

int e0/0
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
no shut
switchport access vlan 85
switchport mode access


```

### SEDE-SW4

```bash
int e0/0
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
no shut
switchport access vlan 86
switchport mode access

int e1/0
no shut
switchport access vlan 87
switchport mode access


```
### SEDE-SW5

```bash
int e0/0
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
no shut
switchport access vlan 88
switchport mode access

```

### SEDE-SW6

```bash
int e0/0
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
no shut
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
no shut
switchport access vlan 89
switchport mode access

int e1/0
no shut
switchport access vlan 90
switchport mode access

```

### SEDE1-L3

```bash

ip routing

vlan 82
name vlan82
exit

vlan 83
name vlan83
exit

vlan 84
name vlan84
exit

interface Ethernet0/0
 no switchport
 ip address 192.168.0.89 255.255.255.252
 no shut

interface Ethernet0/1
 no switchport
 ip address 192.168.0.101 255.255.255.252
 no shut

interface e0/2
no switchport
ip address 192.168.0.125 255.255.255.252
no shut

interface Vlan82
 description gateway vlan82
 ip address 194.65.20.254 255.255.255.0
  no shut

interface Vlan83
 description gateway vlan83
 ip address 194.65.21.254 255.255.255.0
 no shut

interface Vlan84
 description gateway vlan84
 ip address 194.65.22.254 255.255.255.192
 no shut

router ospf 1
network 192.168.0.88 0.0.0.3 area 0
network 192.168.0.100 0.0.0.3 area 0
network 192.168.0.124 0.0.0.3 area 0
network 194.65.20.0 0.0.0.255 area 0
network 194.65.21.0 0.0.0.255 area 0
network 194.65.22.192 0.0.0.63 area 0

```

### SEDE2-L3

```bash
ip routing 

vlan 85
name vlan85
exit

vlan 86
name vlan86
exit

vlan 87
name vlan87
exit

interface Ethernet0/0
 no switchport
 ip address 192.168.0.94 255.255.255.252

interface Ethernet0/1
 no switchport
 ip address 192.168.0.105 255.255.255.252

interface Ethernet0/2
 no switchport
 ip address 192.168.0.102 255.255.255.252

interface Vlan85
 description gateway vlan85
 ip address 194.65.24.94 255.255.255.224
 no shut

interface Vlan86
 description gateway vlan86
 ip address 194.65.25.142 255.255.255.240
  no shut

interface Vlan87
 description gateway vlan87
 ip address 194.65.25.158 255.255.255.240
 no shut



```

### SEDE3-L3

```bash
ip routing 

vlan 88
name vlan88
exit

vlan 90
name vlan90
exit

interface Ethernet0/0
 no switchport
 ip address 192.168.0.98 255.255.255.252

 interface Ethernet0/1
 no switchport
 ip address 192.168.0.138 255.255.255.252

interface Ethernet0/2
 no switchport
 ip address 192.168.0.106 255.255.255.252


interface Vlan88
 description gateway vlan88
 ip address 194.65.26.14 255.255.255.248
 no shut


interface Vlan90
 description gateway vlan90
 ip address 194.65.26.30 255.255.255.248
 no shut

router ospf 1
 network 192.168.0.96 0.0.0.3 area 0
 network 192.168.0.104 0.0.0.3 area 0
 network 192.168.0.136 0.0.0.3 area 0
 network 194.65.26.8 0.0.0.7 area 0
 network 194.65.26.16 0.0.0.7 area 0
 

```
### SEDE4-L3

```bash

ip routing 

vtp mode transparent

vlan 100
 name PVLAN_PRIMARY
  private-vlan primary
  private-vlan association 101-102

vlan 101
 name PVLAN_ISOLATED
  private-vlan isolated

vlan 102
 name PVLAN_COMMUNITY
  private-vlan community

interface Ethernet0/0
 switchport private-vlan mapping 100 101-102
 switchport mode private-vlan promiscuous

interface e0/1
no switchport
ip address 192.168.0.126 255.255.255.252 
no shut

interface Ethernet0/2
 switchport private-vlan host-association 100 101
 switchport mode private-vlan host

interface Ethernet1/0
 switchport private-vlan host-association 100 102
 switchport mode private-vlan host


int e0/3
switchport trunk encapsulation dot1q
switchport mode trunk
no shut

interface Vlan100
 ip address 194.65.19.254 255.255.255.0
 private-vlan mapping 101-102
 no shut

 router ospf 1
 network 192.168.0.124 0.0.0.3 area 0
 network 194.65.19.0 0.0.0.255 area 0


```

### SEDE5-L3

```bash
interface Ethernet0/0
 no switchport
 ip address 192.168.0.110 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 7 030752180500
!
interface Ethernet0/1
 no switchport
 ip address 192.168.0.137 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 7 1511021F0725
!
interface Ethernet0/2
 no switchport
 ip address 192.168.0.134 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 7 01100F175804
!         

hostname SEDE5-L3
ip routing
vlan 89
!
! 1. Porta para o Router (Uplink - TRUNK)
interface Ethernet1/0
 description Uplink_para_Router
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 1,89
 duplex full
 speed 100
 no ip address
 no shutdown
!
! 2. Porta para o PC (Acesso)
interface Ethernet0/3
 description PC_SEDE
 switchport mode access
 switchport access vlan 89
 no shutdown
!
! 3. Gateway da Rede 89 (SVI - IP .22)
interface Vlan 89
 ip address 194.65.26.22 255.255.255.248
 no shutdown
!
end
write

router ospf 1
 network 192.168.0.108 0.0.0.3 area 0
 network 192.168.0.132 0.0.0.3 area 0
 network 192.168.0.136 0.0.0.3 area 0
 network 194.65.26.16 0.0.0.7 area 0

```

### R-SEDE

```bash

 interface Ethernet0/0
 ip address 192.168.0.90 255.255.255.252
no shut
interface Ethernet0/1
 ip address 192.168.0.93 255.255.255.252
no shut
interface Ethernet0/2
 ip address 192.168.0.97 255.255.255.252
no shut
interface Ethernet0/3
 ip address 192.168.0.109 255.255.255.252
no shut
interface Ethernet1/0
 bandwidth 1000
 ip address 10.30.97.241 255.255.255.252
no shut
interface Ethernet1/1
 ip address 192.168.0.86 255.255.255.252
no shut

router ospf 1
 network 10.30.97.240 0.0.0.3 area 0
 network 192.168.0.84 0.0.0.3 area 0
 network 192.168.0.88 0.0.0.3 area 0
 network 192.168.0.92 0.0.0.3 area 0
 network 192.168.0.96 0.0.0.3 area 0
 network 192.168.0.108 0.0.0.3 area 0

```
# ROUTER INTELIGAÇÕES ENTRE FILIAIS:

### SEDE-FILIAL1

```bash

router ospf 1
network  192.168.0.112 0.0.0.3 area 0
network  192.168.0.132 0.0.0.3 area 0
network  10.0.0.24 0.0.0.3 area 0
network  10.0.0.20 0.0.0.3 area 0
network  10.0.0.28 0.0.0.3 area 0
network  10.0.0.32 0.0.0.3 area 0

int s2/1
encapsulation frame-relay 
no shut

int s2/2
encapsulation frame-relay 
no shut

interface s2/2.102 point-to-point
description SEDEf1-CID
ip address 10.0.0.21 255.255.255.252
frame-relay interface-dlci 102

interface s2/2.103 point-to-point
description SEDE-RE
ip address 10.0.0.25 255.255.255.252
frame-relay interface-dlci 103


interface s2/1.102 point-to-point
description SEDEf1-ecma
ip address 10.0.0.29 255.255.255.252
frame-relay interface-dlci 102

interface s2/1.103 point-to-point
description SEDEf1-brcd
ip address 10.0.0.33 255.255.255.252
frame-relay interface-dlci 103

interface e0/1
ip address 192.168.0.113 255.255.255.252
no shut

interface e0/2
ip address 192.168.0.133 255.255.255.252
no shut

```

### SEDE-FILIAL2

```bash
SEDE-FILIAL2(config)# interface Ethernet0/1
SEDE-FILIAL2(config-if)# no shutdown
SEDE-FILIAL2(config-if)# exit

! Configure the Sub-interface for VLAN 120
SEDE-FILIAL2(config)# interface Ethernet0/1.120
SEDE-FILIAL2(config-subif)# encapsulation dot1Q 120
SEDE-FILIAL2(config-subif)# ip address 192.168.0.141 255.255.255.252

! Configure OSPF Authentication on the Interface
SEDE-FILIAL2(config-subif)# ip ospf message-digest-key 1 md5 cisco
SEDE-FILIAL2(config-subif)# ip ospf authentication message-digest
SEDE-FILIAL2(config-subif)# exit

! Configure OSPF Routing Process
SEDE-FILIAL2(config)# router ospf 1
SEDE-FILIAL2(config-router)# network 192.168.0.140 0.0.0.3 area 0


```

### SEDE-FILIAL3

```bash
int e0/0
ip addess 192.168.0.85 255.255.255.252
no shut

router ospf 1
network 10.0.0.0 0.0.0.3 area 0
network 10.0.0.4 0.0.0.3 area 0
network 10.0.0.8 0.0.0.3 area 0
network 10.0.0.12 0.0.0.3 area 0
network 10.0.0.16 0.0.0.3 area 0
network 192.168.0.84 0.0.0.3 area 0

int s2/0
encapsulation frame-relay 
no shut

int s2/1
encapsulation frame-relay 
no shut


interface s2/0.102 point-to-point
description SEDE-CML
ip address 10.0.0.13 255.255.255.252
frame-relay interface-dlci 102

interface s2/0.103 point-to-point
description SEDE-CID
ip address 10.0.0.17 255.255.255.252
frame-relay interface-dlci 103

interface s2/1.102 point-to-point
description SEDE-CCS
ip address 10.0.0.1 255.255.255.252
frame-relay interface-dlci 102

interface s2/1.103 point-to-point
description SEDE-CE
ip address 10.0.0.5 255.255.255.252
frame-relay interface-dlci 103

interface s2/1.104 point-to-point
description SEDE-BRCD
ip address 10.0.0.9 255.255.255.252
frame-relay interface-dlci 104


```
### SEDE-FILIAL4

```bash

hostname SEDE-FILIAL4
ip cef
no ip domain lookup

! 1. Identidade
interface Loopback0
 ip address 4.4.4.4 255.255.255.255
!
! 2. Ligação ao Backbone (mpls1)
interface Ethernet0/0
 description LINK_TO_MPLS1
 ip address 192.168.0.113 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 mpls ip
 no shutdown
!
! 3. Ligação ao Switch (Cliente)
interface Ethernet0/1
 description LINK_TO_SWITCH_SEDE
 no ip address
 duplex full
 speed 100
 no shutdown
!
! 4. O Túnel L2VPN (VLAN 89)
interface Ethernet0/1.89
 encapsulation dot1Q 89
 xconnect 7.7.7.7 89 encapsulation mpls
  ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 no shutdown
!
! 5. OSPF
router ospf 1
 network 4.4.4.4 0.0.0.0 area 0
 network 192.168.0.112 0.0.0.3 area 0
 mpls ldp router-id Loopback0 force
!
end
write


```

# MPLS

## Routers

### MPLS1

```bash

hostname mpls1
ip cef
no ip domain lookup

! 1. Identidade
interface Loopback0
 ip address 5.5.5.5 255.255.255.255
!
! 2. Interfaces Core
interface Ethernet0/0
 description LINK_TO_SEDE
 ip address 192.168.0.114 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 mpls ip
 no shutdown
!
interface Ethernet0/1
 description LINK_TO_MPLS2
 ip address 192.168.0.117 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 mpls ip
 no shutdown
!
! 3. OSPF
router ospf 1
 network 5.5.5.5 0.0.0.0 area 0
 network 192.168.0.112 0.0.0.3 area 0
 network 192.168.0.116 0.0.0.3 area 0
 mpls ldp router-id Loopback0 force
!
end
write

```

### MPLS2

```bash
hostname mpls2
ip cef
no ip domain lookup

! 1. Identidade
interface Loopback0
 ip address 6.6.6.6 255.255.255.255
!
! 2. Interfaces Core
interface Ethernet0/0
 description LINK_TO_MPLS1
 ip address 192.168.0.118 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 mpls ip
 no shutdown
!
interface Ethernet0/1
 description LINK_TO_MPLS3
 ip address 192.168.0.121 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 mpls ip
 no shutdown
!
! 3. OSPF
router ospf 1
 network 6.6.6.6 0.0.0.0 area 0
 network 192.168.0.116 0.0.0.3 area 0
 network 192.168.0.120 0.0.0.3 area 0
 mpls ldp router-id Loopback0 force
!
end
write

```

### MPLS3
```bash
hostname mpls3
ip cef
no ip domain lookup

! 1. Identidade
interface Loopback0
 ip address 7.7.7.7 255.255.255.255
!
! 2. Ligação ao Backbone (mpls2)
interface Ethernet0/1
 description LINK_TO_MPLS2
 ip address 192.168.0.122 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 mpls ip
 no shutdown
!
! 3. Ligação ao Switch (Cliente)
interface Ethernet0/0
 description LINK_TO_SWITCH_ECMA
 no ip address
 no shutdown
!
! 4. O Túnel L2VPN (VLAN 89)
interface Ethernet0/0.89
 encapsulation dot1Q 89
 xconnect 4.4.4.4 89 encapsulation mpls
  ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 no shutdown
!
! 5. OSPF
router ospf 1
 network 7.7.7.7 0.0.0.0 area 0
 network 192.168.0.120 0.0.0.3 area 0
 mpls ldp router-id Loopback0 force
!
end
write
```

# Qin-Q

### SW1-Qinq

```bash

SW1-Qinq(config)# vlan 130
SW1-Qinq(config-vlan)# name SP-VLAN
SW1-Qinq(config-vlan)# exit

! Configure the link to the Core (SW3)
SW1-Qinq(config)# interface Ethernet0/1
SW1-Qinq(config-if)# switchport trunk encapsulation dot1q
SW1-Qinq(config-if)# switchport mode trunk
SW1-Qinq(config-if)# exit

! Configure the link to the Customer (SEDE-FILIAL4) for Q-in-Q
SW1-Qinq(config)# interface Ethernet0/0
SW1-Qinq(config-if)# switchport access vlan 130
SW1-Qinq(config-if)# switchport mode dot1q-tunnel
SW1-Qinq(config-if)# no cdp enable
SW1-Qinq(config-if)# no shutdown


```

### SW2-Qinq
```bash

SW2-QUINQ(config)# vlan 130
SW2-QUINQ(config-vlan)# name SP-VLAN
SW2-QUINQ(config-vlan)# exit

! Configure the link to the Core (SW3)
SW2-QUINQ(config)# interface Ethernet0/0
SW2-QUINQ(config-if)# switchport trunk encapsulation dot1q
SW2-QUINQ(config-if)# switchport mode trunk
SW2-QUINQ(config-if)# exit

! Configure the link to the Customer (R-BRCD3) for Q-in-Q
SW2-QUINQ(config)# interface Ethernet0/1
SW2-QUINQ(config-if)# switchport access vlan 130
SW2-QUINQ(config-if)# switchport mode dot1q-tunnel
SW2-QUINQ(config-if)# no cdp enable
SW2-QUINQ(config-if)# no shutdown


```

### SW3-Qinq

```bash

SW3-Qinq(config)# vlan 130
SW3-Qinq(config-vlan)# name SP-VLAN
SW3-Qinq(config-vlan)# exit

! Configure trunks to SW1 and SW2
SW3-Qinq(config)# interface range Ethernet0/0 - 1
SW3-Qinq(config-if-range)# switchport trunk encapsulation dot1q
SW3-Qinq(config-if-range)# switchport mode trunk
SW3-Qinq(config-if-range)# no shutdown
```
