# sk

[1] Kurose&Ross, [Sieci Komputerowe](http://helion.pl/ksiazki/sieci-komputerowe-ujecie-calosciowe-wydanie-v-james-f-kurose-keith-w-ross,sieuc5.htm), wyd. V

[2] Krysiak, [Sieci komputerowe. Kompendium](http://helion.pl/ksiazki/sieci-komputerowe-kompendium-wydanie-ii-karol-krysiak,adsi2v.htm), wyd. II

[3] Forouzan, [Computer Networks: A Top-Down Approach](https://www.amazon.com/Computer-Networks-Top-Down-Approach/dp/0073523267/ref=sr_1_3?s=books&ie=UTF8&qid=1478721337&sr=1-3&keywords=forouzan+computer)

---
2016.12.01

- konfiguracja VLAN, IEEE 802.1Q
- trunking pomiedzy dwoma switchami
- routing pomiedzy sieciami VLAN, 'router on stick'
- routing dynamiczny, konfiguracja routera
- protokoly routingu dynamicznego 
- konfiguracja routera RIPv2, algorytm Bellmana-Forda, 
- wplyw zmiany topologii sieci na tablice routingu
- konfiguracja routera OSPF, algorytm Dijkstry
- zmiana metryki w tablicy routingu

a) wykorzystanie routera Cisco c3725 z modulem NM-16ESW jako konfigurowalnego switcha

  - `PCMCI slot memory > 1 MiB`
  - `erase flash:`

b) konfiguracja dwoch sieci VLAN polaczonych za pomoca trunka pomiedzy switchami S1 i S2

```bash
S1#vlan database
S1(vlan)#vlan 10 name office1
S1(vlan)#vlan 20 name office2
S1(vlan)#vlan 30 name office3
S1(vlan)#exit
S1#show vlan-switch
S1#show ip interace brief
S1# configure terminal
S1(config)# interface f1/0
S1(config-if)# switchport mode trunk
S1(config-if)# exit
S1(config)# interface f1/1
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 10
S1(config-if)# exit
S1(config)# interface f1/2
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 20
S1(config-if)# exit
S1(config)# interface f1/3
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 30
S1(config-if)# exit
```
c) przechwytywanie pakietow ICMP za pomoca Wiresharka pomiedzy VPCS1 a S1 oraz na trunku pomiedzy S1 oraz S2

d) sprawdzenie tagowania wg. standardu 802.1Q, TPID, TCI (PCP,CFI,VID)

e) routing pomiedzy sieciami VLAN, 'router on stick'


h) wlaczenie routingu dynamicznego RIPv2 na routerze R1 podlaczonego do sieci 192.168.1.0 oraz 192.168.3.0
```
R1#configure terminal
R1(config)#router rip
R1(config-router)no auto-summary
R1(config-router)version 2
R1(config-router)network 192.168.1.0
R1(config-router)network 192.168.3.0
R1(config-router)exit
R1(config)#exit
```

i) wlaczenie routingu dynamicznego OSPF na routerze R2

```
R1#configure terminal
R1(config)#router ospf 1
R1(config-router)#network 0.0.0.0 255.255.255.255.255 area 0
R1(config-router)#end
```


---
2016.11.17

- routing statyczny, brama domyslna
- tablice routingu, flagi, metryka
- polaczenie z routerem przez telnet, konfiguracja tty
- konfiguracja trasowania routera i VPCS-a
- ping, traceroute

a) konfiguracja interfejsow sieciowych
```
ifconfig -a
ip addr show
```
```
ifconfig eth0 192.168.1.2 netmask 255.255.255.0 broadcast 192.168.1.255 up
```
lub za pomoca `ip addr add` z pakietu `iproute2`
```
ip addr add 127.0.0.1/8 dev lo
ip link set lo up
```
```
ip addr add 192.168.1.2/24 dev eth0
ip link set eth0 up
```



b) wyswietlanie tablicy routingu 
```
route -n
ip route show
```

c) modyfikacja tablic routingu za pomoca polecenia `route`

```
route {add|del} [-net| -host] <target IP> [netmask <netmask IP>] [gw <gateway IP>] [<interface>]
```
Przyklady konfiguracji trasowania sieci `127.0.0.0`, `192.168.1.0`, `10.0.0.0`
```
route add -net 127.0.0.0 netmask 255.0.0.0 lo
route add -net 192.168.1.0 netmask 255.255.255.0 eth0
route add -net 10.0.0.0 netmask 255.0.0.0 eth1
```
Zdefiniowanie bramy domyslnej
```
route add default gw 10.0.0.1
```

d) modyfikacja tablic routingu za pomoca polecenia `ip` z pakietu `iproute2`

```
ip route add 127.0.0.0/8 dev lo scope link
ip route add 192.168.1.0/24 dev eth0 scope link
ip route add 10.0.0.0/8 dev eth1 scope link
ip route add default via 10.0.0.1 dev eth1
```

e) wlaczenie obslugi routingu na hoscie
```
echo "1" > /proc/sys/net/ipv4/ip_forward
```

f) przypisanie adresu IP do VPCS-a PC1, sprawdzenie IP, sprawdzenie tablicy ARP
```
PC1>ip 10.0.5.3/24 10.0.5.1
PC1>show ip [all]
PC1>trace 10.0.5.2
```

f) zapis i wczytanie ustawien VPCS-a PC1
```
PC1>save PC1.vpc
PC1>load PC1.vpc
```

g) zapis i wczytanie ustawien routera

```
write memory
```
