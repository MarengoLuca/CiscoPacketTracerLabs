# 🌐 Esercizio 07: Syslog Server
## 🎯 Obiettivo:

Impostare il servizio Syslog su un server
Lo scopo dell'esercizio è: 
- Configurare gli indirizzi IP dei dispositivi
- Configurare il servizio Syslog
- Verificare la tabella del Syslog

## 🖥️ Topologia di rete

Schema della Rete realizzata:

![Topologia](img/topologia.png)

## 🧰 Dispositivi utilizzati
|  Dispositivo  |  Quantità  |
|--|--|
| Cisco Router 2911 | 1 |
| Cisco Catalyst 2960-24TT | 1 |
| PC | 2 |
| Server-PT | 1 |
| Cavi Copper Straight-Through | 4 |

## 📡Configurazione IP
| Dispositivo | Indirizzo IP | Subnet Mask |  Default Gateway |
|---|---|---|---|
| 💻 PC1 | 192.168.0.11 | 255.255.255.0 | 192.168.0.254 |
| 💻 PC2 | 192.168.0.12 | 255.255.255.0 | 192.168.0.254 |
| 🔀 SW1 | 192.168.0.2 | 255.255.255.0 | 192.168.0.254 |
| 🌐 SERVER | 192.168.1.100 | 255.255.255.0 | 192.168.1.254 |

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
È stata configurata l'interfaccia virtuale VLAN 1 (SVI) per poter comunicare con il server Syslog, questo è necessario affinché lo switch possa generare traffico IP e comunicare verso il server. Senza un indirizzo IP sulla SVI, lo switch opererebbe esclusivamente a livello 2 e non potrebbe generare traffico IP verso altri dispositivi della rete.
La configurazione è stata effettuata tramite questo comando:
```
int vlan1
ip address 192.168.0.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.0.254
logging on ==> abilito il servizio di logging
logging 192.168.0.100 ==> specifico il server a cui mandare i logs
logging trap debugging ==> definisce il livello di gravità dei messaggi di sistema (da 0 a 7, dal più grave al meno grave)
service timestamps log datetime msec
```
**Analisi comando service timestamps log datetime msec:**

--> ```service timestamps``` attiva la marcatura temporale per i servizi di sistema
--> ```log``` specifica che questa regola si applica ai messaggi di log
--> ```datetime``` sostituisce il counter dei minuti/giorni dall'avvio con la data e l'ora reali
--> ```msec``` aggiunge il massimo livello di precisione, mostrando anche i milli-secondi


Questo perché lo switch diventa un client di rete che invia i logs al server.
Solitamente, non si usa mai la vlan1 come default, ma si crea una vlan dedicata per motivi di sicurezza.


### 🖧 Router

Configurazione  dell'interfaccia Gig0/0 tramite i seguenti comandi:

```
conf t
hostname R1
int Gig0/0
ip address 192.168.0.254 255.255.255.0
no shutdown
exit
logging on
logging 192.168.0.100
logging trap debugging
service timestamps log datetime msec
```

## 🛠️ Test Effettuati

Verifica dell'effettivo collegamento dei PC allo Switch tramite il comando:

```
show interfaces status
```
con seguente Output:
```
SW1
Port    Name  Status     Vlan   Duplex    Speed   Type
Fa0/1         connected  1      a-full    a-100   10/100BaseTX
Fa0/2         connected  1      a-full    a-100   10/100BaseTX
Gig0/1        connected  1      a-full    a-100   10/100/1000BaseTX
Gig0/2        connected  1      a-full    a-100   10/100/1000BaseTX
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

   1    0001.c762.8201    DYNAMIC     Gig0/2
   1    0001.c96e.41e6    DYNAMIC     Fa0/2
   1    0060.4786.51de    DYNAMIC     Fa0/1
   1    00d0.ff02.e57e    DYNAMIC     Gig0/1
```

Verifica degli indirizzi IP assegnati alle porte del router tramite il comando:
```
show ip interface brief
```

con seguente Output:

```
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/1     192.168.0.254   YES manual up                    up 
```

# 🛠️ Problemi riscontrati

