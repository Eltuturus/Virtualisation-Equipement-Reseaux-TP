# TP : Connectivité entre LAN et accès internet

## 🌞 Vérification de la connectivité entre deux réseaux locaux
### `ping` depuis un client du `LAN1` vers un client du `LAN2`

**Configuration du client `PC1` :**
```plaintext
PC1> show ip
Nom : PC1[1]
Adresse IP/Masque : 10.3.1.1/24
Passerelle : 10.3.1.254
Adresse MAC : 00:50:79:66:68:00
Port local : 20026
Hôte distant : 127.0.0.1:20027
MTU : 1500
```

**Test de connectivité :**
```plaintext
PC1> ping 10.3.2.2
84 bytes from 10.3.2.2 icmp_seq=1 ttl=62 time=41.389 ms
84 bytes from 10.3.2.2 icmp_seq=2 ttl=62 time=38.424 ms
84 bytes from 10.3.2.2 icmp_seq=3 ttl=62 time=33.709 ms
84 bytes from 10.3.2.2 icmp_seq=4 ttl=62 time=29.687 ms
84 bytes from 10.3.2.2 icmp_seq=5 ttl=62 time=23.024 ms
```

---

## 🌞 Analyse avec Wireshark
Effectuez une capture réseau intitulée **`ping_partie1`** pour visualiser l’échange ICMP entre les deux LAN.  
**Lien vers la capture :** [Echange ICMP entre deux LAN](ping_entre_deux_lan.pcapng)

---

## 🌞 Récupération des adresses MAC des routeurs
**Commandes exécutées sur le routeur R2 :**
```plaintext
R2#show arp
Protocol  Address       Age (min)  Hardware Addr   Type  Interface
Internet  10.3.12.1     24         c401.05da.0001  ARPA  FastEthernet0/0
Internet  10.3.12.2      -         c402.060a.0000  ARPA  FastEthernet0/0
Internet  10.3.2.2       0         0050.7966.6802  ARPA  FastEthernet1/0
Internet  10.3.2.1      17         0050.7966.6803  ARPA  FastEthernet1/0
Internet  10.3.2.254     -         c402.0600.0010  ARPA  FastEthernet1/0
```

---

## 🌞 Vérification de l’accès internet depuis `R1`
**Commandes exécutées sur le routeur R1 :**
```plaintext
R1#ping 8.8.8.8
Type escape sequence to abort.
Envoi de 5 paquets ICMP de 100 octets vers 8.8.8.8, délai d’expiration de 2 secondes :
Taux de réussite : 100 % (5/5), temps aller-retour min/moy/max = 60/79/96 ms
```

---

## 🌞 Test d’accès internet depuis le `LAN1`
**Depuis PC1 :**
```plaintext
PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=59 time=39.400 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=59 time=44.366 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=59 time=39.093 ms
```

---

## 🌞 Test d’accès internet depuis le `LAN2`
**Depuis PC3 :**
```plaintext
PC3> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=57 time=47.942 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=57 time=45.632 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=57 time=52.843 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=57 time=62.019 ms
