# 🌐 Esercizio 08: Password e Banner di sicurezza
## 🎯 Obiettivo:

Impostare il su switch e router password e banner.
Lo scopo dell'esercizio è: 
- Configurare gli indirizzi IP dei dispositivi
- Configurare una sicurezza base su switch e router
- Verificare la sicurezza configurata

## 🖥️ Topologia di rete

Schema della Rete realizzata:

![Topologia](img/topologia.png)

## 🧰 Dispositivi utilizzati
|  Dispositivo  |  Quantità  |
|--|--|
| Cisco Router 2911 | 1 |
| Cisco Catalyst 2960-24TT | 1 |
| PC | 1 |
| Cavi Copper Straight-Through | 3 |

## 📡Configurazione IP
| Dispositivo | Indirizzo IP | Subnet Mask |  Default Gateway |
|---|---|---|---|
| 💻 PC1 | 192.168.0.11 | 255.255.255.0 | 192.168.0.254 |
| 🔀 SW1 | 192.168.0.2 | 255.255.255.0 | 192.168.0.254 |

## ⚙️ Configurazione Eseguita

###  💻 Computer

Configurati manualmente:

- Indirizzo IP
- Subnet Mask
- Default Gateway

### 🔀 Switch

Lo switch opera in modalità Layer 2 con configurazione di default (VLAN 1)
E' stato modificato l'hostname con il seguente comando:
```
hostname SW1
```
È stata configurata l'interfaccia virtuale VLAN 1 (SVI) per poterci collegare tramite protocollo SSH. Senza un indirizzo IP sulla SVI, lo switch opererebbe esclusivamente a livello 2 e non potrebbe generare traffico IP verso altri dispositivi della rete.
La configurazione è stata effettuata tramite questo comando:
```
int vlan1
ip address 192.168.0.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.0.254
```
Configurazione degli accessi e banner tramite:

```
conf t
enable secret Cisco123!
service password-encryption
banner motd #ATTENZIONE! Accessi consentito solo al personale autorizzato.#
line console 0
password Console123!
login
exit
ip domain-name test.local
ip ssh version 2
username admin secret SshPass123!
crypto key generate rsa [2048]
line vty 0 4
transport input ssh
login local
```

Solitamente, non si usa mai la vlan1 come default, ma si crea una vlan dedicata per motivi di sicurezza.

Spiegazione comandi:

--> ```service password-encryption``` permette di cifrare le password nel file di configurazione
--> ```banner motd``` permette di inserire un banner che viene visualizzato durante la connessione ad un device

📌 Differenza tra ```enable password``` e ```enable secret```

Utilizzando ```enable password```, le password non vengono cifrate. Diversamente usando ```enable secret``` vengono cifrate.


### 🖧 Router

Configurazione  dell'interfaccia Gig0/0 tramite i seguenti comandi:

```
conf t
hostname R1
int Gig0/0
ip address 192.168.0.254 255.255.255.0
no shutdown
exit
```

Configurazione degli accessi e banner tramite:

```
conf t
enable secret Cisco123!
service password-encryption
banner motd #ATTENZIONE! Accessi consentito solo al personale autorizzato.#
line console 0
password Console123!
login
exit
ip domain-name test.local
ip ssh version 2
username admin secret SshPass123!
crypto key generate rsa [2048]
line vty 0 4
transport input ssh
login local
```

## 🛠️ Test Effettuati

Verifica dell'effettivo collegamento del PC allo Switch tramite il comando:

```
show interfaces status
```
con seguente Output:
```
SW1
Port    Name  Status     Vlan   Duplex    Speed   Type
Fa0/1         connected  1      a-full    a-100   10/100BaseTX
Gig0/1        connected  1      a-full    a-100   10/100/100BaseTX
```

Verifica della Switching Table dello Switch utilizzando il comando:

```
show mac address-table
```
con seguente Output:

```
SW1
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0060.5ca3.a901    DYNAMIC     Gig0/1
```

Verifica degli indirizzi IP assegnati alle porte del router tramite il comando:
```
show ip interface brief
```

con seguente Output:

```
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.0.254   YES manual up                    up 
```

Verifica del funzionamento dell'accesso console verso lo switch e il router:

```
ATTENZIONE! Accesso consentito solo al personale autorizzato.

User Access Verification

Password: 
```

Dopo l'inserimento della password per l'accesso alla console, digitando ```enable```, verrà richiesta la password per l'innalzamento dei privilegi:
```
SW1>enable
Password: 
SW1#
```

Possiamo notare come viene visualizzato anche il messaggio mtod.

Analogalmente avviene la stessa cosa dal router:

```
ATTENZIONE! Accesso consentito solo al personale autorizzato.

User Access Verification

Password: 
```

Dopo l'inserimento della password per l'accesso alla console, digitando ```enable```, verrà richiesta la password per l'innalzamento dei privilegi:
```
R1>enable
Password: 
R1#
```

Provando invece il collegamento tramite SSH:

```
C:\>ssh -l admin 192.168.0.2

Password: 

ATTENZIONE! Accesso consentito solo al personale autorizzato.

SW1>
```

```
C:\>ssh -l admin 192.168.0.254

Password: 

ATTENZIONE! Accesso consentito solo al personale autorizzato.

R1>
```