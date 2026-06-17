# 🌐 Esercizio 02: Due Reti Un Router
## 🎯 Obiettivo:

Creare due reti LAN semplici collegate tramite Router,  ciascuna composta da 1 dispositivo finale collegato tramite uno Switch.
Lo scopo dell'esercizio è: 
- Configurare gli indirizzi IP dei dispositivi
- Configurare le interfacce del Router
- Verificare la comunicazione nella rete
- Utilizzare il comando 'ping' per il test

## 🖥️ Topologia di rete

Schema della Rete realizzata:

![Topologia](img/topologia.png)

## 🧰 Dispositivi utilizzati
|  Dispositivo  |  Quantità  |
|--|--|
| Cisco Catalyst 2960 | 2 |
| Cisco Router 2911 | 1 |
| PC | 2 |
| Cavi Copper Straight-Through | 4 |

## 📡Configurazione IP
| Dispositivo | Indirizzo IP | Subnet Mask | Gateway |
|---|---|---|---| 
| 💻 PC1 | 192.168.1.1 | 255.255.255.0 | 192.168.1.254 |
| 💻 PC2 | 192.168.2.1 | 255.255.255.0 | 192.168.2.254 |

## ⚙️ Configurazione Eseguita

###  💻 Computer

Configurati Manualmente:

- Indirizzo IP
- Subnet Mask
- Gateway

### 🔀 Switch

Nessuna configurazione è stata apportata.

### 🖧 Router

Configurazione delle due interfacce Gig0/0 e Gig0/1 tramite i seguenti comandi:
```
conf t
int Gig0/0
ip address 192.168.1.254 255.255.255.0
no shutdown
exit 

conf t
int Gig0/1
ip address 192.168.2.254 255.255.255.0
no shutdown
exit

write
```

## 🛠️ Test Effettuati

Verifica dell'effettivo collegamento dei PC agli Switch tramite il comando:

```
show interfaces status
```
con seguente Output:
```
SW1
Port    Name  Status     Vlan   Duplex    Speed   Type
Fa0/1         connected  1      a-full    a-100   10/100BaseTX
Gig0/1        connected  1      a-full    a-100   10/100/1000BaseTX

SW2
Port    Name  Status     Vlan   Duplex    Speed   Type
Fa0/1         connected  1      a-full    a-100   10/100BaseTX
Gig0/1        connected  1      a-full    a-100   10/100/1000BaseTX
```
Verifica della Switching Table dello Switch utilizzando il comando:

```
show mac address-table
```
con seguente Output:

```
SW1
Mac Address Table
-------------------------------------------
Vlan Mac Address    Type     Ports
---- -----------    -------- -----
1    00d0.ff60.6702 DYNAMIC  Gig0/1

SW2
Mac Address Table
-------------------------------------------
Vlan Mac Address    Type     Ports
---- -----------    -------- -----
1    00d0.ff60.6701 DYNAMIC  Gig0/1
```
Verifica della Configurazione del Router tramite il comando:
```
show ip interface brief
```
con seguente Output:

```
Interface            IP-Address      OK?   Method   Status   Protocol
GigabitEthernet0/0   192.168.2.254   YES   manual   up       up
GigabitEthernet0/1   192.168.1.254   YES   manual   up       up
```


Verifica della comunicazione tramite ping:  

✔ Test di connettività: tutti i ping riusciti tra i PC

📌 Risultato:  
Tutti i dispositivi comunicano correttamente all'interno della rete. Le due LAN comunicano correttamente tramite routing a livello 3 effettuato dal router.


# 🛠️ Problemi riscontrati

Nessun problema rilevato.



