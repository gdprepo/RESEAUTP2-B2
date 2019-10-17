
# TP2 - Reseau

## 1

```
                            +-----+        +-------+        +-----+
                            | PC1 +--------+  SW1  +--------+ PC2 |
                            +-----+        +-------+        +-----+


                                    VM	 |   IP
                                ---------|-------------------
                                 PC1	 |   10.2.1.1/24
                                 PC2	 |   10.2.1.2/24

```

* Ping pour prouver que les deux pcs peuvent communiquer entre eux

```
PC1> ping 10.2.1.2
84 bytes from 10.2.1.2 icmp_seq=1 ttl=64 time=0.202 ms
84 bytes from 10.2.1.2 icmp_seq=2 ttl=64 time=0.371 ms
84 bytes from 10.2.1.2 icmp_seq=3 ttl=64 time=0.359 ms
84 bytes from 10.2.1.2 icmp_seq=4 ttl=64 time=0.295 ms

```


## 2




```
                                           +-------+
                                           |  PC2  |
                                           +-------+
                                               |
                                           +-------+
                                           +  SW2  +
                                           +-------+
                                         /          \
                                        /            \
                        +-----+        +-------+    +-------+       +-----+
                        | PC1 +--------+  SW1  +----+  SW3  +-------+ PC3 |
                        +-----+        +-------+    +-------+       +-----+

```

* Ping pour faire commiquer les troic pcs.

```

PC1> ping 10.2.2.2
84 bytes from 10.2.2.2 icmp_seq=1 ttl=64 time=1.154 ms

PC2-2> ping 10.2.2.3
84 bytes from 10.2.2.3 icmp_seq=1 ttl=64 time=0.824 ms
84 bytes from 10.2.2.3 icmp_seq=2 ttl=64 time=0.632 ms
84 bytes from 10.2.2.3 icmp_seq=3 ttl=64 time=0.897 ms

PC2-3> ping 10.2.2.1
84 bytes from 10.2.2.1 icmp_seq=1 ttl=64 time=0.737 ms
84 bytes from 10.2.2.1 icmp_seq=2 ttl=64 time=0.568 ms


```

* Table MAC de SW1

```

SW1-1# show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6803    DYNAMIC     Et0/1
   1    0050.7966.6804    DYNAMIC     Et0/2
   1    aabb.cc00.0310    DYNAMIC     Et0/1
   1    aabb.cc00.0410    DYNAMIC     Et0/2
   1    aabb.cc00.0420    DYNAMIC     Et0/3
Total Mac Addresses for this criterion: 5
```


```
SW1#show spanning-tree

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0200
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec


SW2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0200
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

SW3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0200
             Cost        100
             Port        3 (Ethernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

```
- Le port du SW3-1 Et0/1 est bloqué


### Spanning Tree Protocol

- SW1 est Forwarding et SW2 aussi
```
SW1#show spanning-tree summary
Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
VLAN0001                     0         0        0         16         16
---------------------- -------- --------- -------- ---------- ----------
1 vlan                       0         0        0         16         16
```

- Et SW3 est Blocking
```
SW3#show spanning-tree summary
Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
VLAN0001                     1         0        0         15         16
---------------------- -------- --------- -------- ---------- ----------
1 vlan                       1         0        0         15         16

```

- VLAN avec Wireshark
```
18.491070 Private_66:68:09  Private_66:68:0b  ARP 68  10.2.20.1 is at 00:50:79:66:68:09
18.491650 10.2.20.2 10.2.20.1 ICMP  102 Echo (ping) request  id=0xe464, seq=1/256, ttl=64 (reply in 47)
18.492118 10.2.20.1 10.2.20.2 ICMP  102 Echo (ping) reply    id=0xe464, seq=1/256, ttl=64 (request in 46)
```
