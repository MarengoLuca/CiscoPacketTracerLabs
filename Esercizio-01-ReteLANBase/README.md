# 🌐 Esercizio 01: Rete LAN base con Switch
## 🎯 Obiettivo:

Creare una rete LAN semplice composta da 4 dispositivi collegati tramite uno Switch.
Lo scopo dell'esercizio è: 
- Configurare gli indirizzi IP dei dispositivi
- Verificare la comunicazione nella rete
- Utilizzare il comando 'ping' per il test

## 🖥️ Topologia di rete

Schema della Rete realizzata:

![Topologia](img/topologia.png)

## 🧰 Dispositivi utilizzati
|  Dispositivo  |  Quantità  |
|--|--|
| Cisco Catalyst 2960 | 1 |
| PC | 4 |
| Cavi Copper Straight-Through | 4 |

## 📡Configurazione IP
| Dispositivo | Indirizzo IP | Subnet Mask |  
|---|---|---|  
| 💻 PC0 | 192.168.0.100 | 255.255.255.0 |  
| 💻 PC1 | 192.168.0.101 | 255.255.255.0 |  
| 💻 PC2 | 192.168.0.102 | 255.255.255.0 |
| 💻 PC3 | 192.168.0.103 | 255.255.255.0 |

## ⚙️ Configurazione Eseguita

###  💻 Computer

Configurati Manualmente:

- Indirizzo IP
- Subnet Mask

### 🔀 Switch

Nessuna configurazione è stata apportata.

## 🛠️ Test Effettuati

Verifica dell'effettivo collegamento dei PC allo Switch tramite il comando:

```
show interfaces status
```
con seguente Output:
```
Port Name Status     Vlan  Duplex   Speed   Type
Fa0/1     connected  1     a-full   a-100   10/100BaseTX
Fa0/2     connected  1     a-full   a-100   10/100BaseTX
Fa0/3     connected  1     a-full   a-100   10/100BaseTX
Fa0/4     connected  1     a-full   a-100   10/100BaseTX
Fa0/5     notconnect 1     auto     auto    10/100BaseTX
Fa0/6     notconnect 1     auto     auto    10/100BaseTX
Fa0/7     notconnect 1     auto     auto    10/100BaseTX
Fa0/8     notconnect 1     auto     auto    10/100BaseTX
```
Verifica della Switching Table dello Switch utilizzando il comando:

```
show mac address-table
```
con seguente Output:

```
Mac Address Table
-------------------------------------------
Vlan Mac Address    Type     Ports
---- -----------    -------- -----
1    0001.42e2.119a DYNAMIC  Fa0/2
1    0007.ec04.8b32 DYNAMIC  Fa0/3
1    0009.7ccd.653b DYNAMIC  Fa0/1
1    000b.be8d.1d85 DYNAMIC  Fa0/4
```

Verifica della comunicazione tramite ping:  

✔ Test di connettività: tutti i ping riusciti tra i PC

📌 Risultato:  
Tutti i dispositivi comunicano correttamente all'interno della rete. La rete LAN funziona correttamente in configurazione flat con subnet 192.168.0.0/24. Tutti i nodi comunicano tramite switching Layer 2 senza necessità routing. 


# 🛠️ Problemi riscontrati

Nessun problema rilevato.



