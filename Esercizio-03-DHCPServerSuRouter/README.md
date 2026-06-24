# 🌐 Esercizio 03: DHCP server su router
## 🎯 Obiettivo:

Impostare un Router con funziona di DHCP server
Lo scopo dell'esercizio è: 
- Configurare gli indirizzi IP dei dispositivi
- Configurare l'interfaccia del Router
- Configurare il servizio DHCP
- Verificare la comunicazione nella rete
- Utilizzare il comando 'ping' per il test

## 🖥️ Topologia di rete

Schema della Rete realizzata:

![Topologia](img/topologia.png)

## 🧰 Dispositivi utilizzati
|  Dispositivo  |  Quantità  |
|--|--|
| Cisco Catalyst 2960 | 1 |
| Cisco Router 2911 | 1 |
| PC | 2 |
| Cavi Copper Straight-Through | 3 |

## 📡Configurazione IP
| Dispositivo | Indirizzo IP | Subnet Mask | Gateway | DNS Server |
|---|---|---|---| --- |
| 💻 PC1 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 | 8.8.8.8 |
| 💻 PC2 | 192.168.1.12 | 255.255.255.0 | 192.168.1.1 | 8.8.8.8 |

Gli indirizzi IP riportati sono stati assegnati automaticamente dal servizio DHCP configurato sul router.

## ⚙️ Configurazione Eseguita

###  💻 Computer

Indirizzi configurati tramite DCHP Server

### 🔀 Switch

Lo switch opera in modalità Layer 2 con configurazione di default (VLAN 1)

### 🖧 Router

Configurazione del interfaccia Gig0/0 tramite i seguenti comandi:
```
conf t
int Gig0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit 
```
Configurazione del Servizio DHCP tramite i seguenti comandi:

```
conf t
ip dhcp pool RETE-LAN
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
exit
ip dhcp excluded-address 192.168.1.1 192.168.1.10
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
Fa0/2         connected  1      a-full    a-100   10/100BaseTX
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
```
Verifica della Configurazione del Router tramite il comando:
```
show ip interface brief
```
con seguente Output:

```
Interface            IP-Address      OK?   Method   Status   Protocol
GigabitEthernet0/0   192.168.1.1   YES   manual   up       up
```
Verifica del Pool del DHCP tramite il comando:
```
show ip dhcp pool
```
con seguente Output:

```
Pool RETE-LAN :
Utilization mark (high/low) : 100 / 0
Subnet size (first/next) : 0 / 0
Total addresses : 254
Leased addresses : 2
Excluded addresses : 1
Pending event : none

1 subnet is currently in the pool
Current index     IP address range              Leased/Excluded/Total
192.168.1.1       192.168.1.1 - 192.168.1.254   2     / 1      / 254
```
Verifica degli indirizzi IP assegnati dal DHCP tramite il comando:
```
show ip dhcp binding
```
con seguente Output:
```
IP address     Client-ID/             Lease expiration   Type
               Hardware address
192.168.1.11   00E0.B0DA.289E         --                 Automatic
192.168.1.12   0001.9729.C77B         --                 Automatic
```

Verifica della comunicazione tramite ping:  

✔ Test di connettività: tutti i ping riusciti tra i PC

📌 Risultato:  
Tutti i dispositivi comunicano correttamente all’interno della stessa rete IP tramite switching Layer 2 e gateway unico sul router.


# 🛠️ Problemi riscontrati

Nessun problema rilevato.



