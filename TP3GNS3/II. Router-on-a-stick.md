# TP : Configuration VLAN et tests de connectivité

## 🌞 Tests de connectivité pour les VLANs
### A. VLANs
**Depuis PC1 :**
```plaintext
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=2.778 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=2.868 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=3.087 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=2.719 ms
84 bytes from 10.3.1.2 icmp_seq=5 ttl=64 time=2.738 ms
```

---

### B. Routeur
**Tests de connectivité :**
```plaintext
R1#ping 10.3.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 44/45/52 ms

R1#ping 10.3.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 32/40/48 ms

R1#ping 10.3.1.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 36/50/56 ms
```

**Configuration IP de PC1 :**
```plaintext
PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         : 
MAC         : 00:50:79:66:68:00
LPORT       : 20018
RHOST:PORT  : 127.0.0.1:20019
MTU         : 1500

PC1> ping 10.3.2.1

84 bytes from 10.3.2.1 icmp_seq=1 ttl=63 time=15.255 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=63 time=15.914 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=63 time=20.622 ms
```

---

## 🌞 Vérification de l’accès internet
**Depuis le routeur R1 :**
```plaintext
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 84/90/96 ms
```

**Depuis PC1 :**
```plaintext
PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=59 time=25.879 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=59 time=31.444 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=59 time=31.921 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=59 time=25.661 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=59 time=25.468 ms
```

**Depuis PC2 :**
```plaintext
PC2> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=59 time=30.584 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=59 time=30.328 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=59 time=27.446 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=59 time=23.323 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=59 time=27.337 ms
```
