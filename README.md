# sk

[1] Kurose&Ross, [Sieci Komputerowe](http://helion.pl/ksiazki/sieci-komputerowe-ujecie-calosciowe-wydanie-v-james-f-kurose-keith-w-ross,sieuc5.htm), wyd. V

[2] Krysiak, [Sieci komputerowe. Kompendium](http://helion.pl/ksiazki/sieci-komputerowe-kompendium-wydanie-ii-karol-krysiak,adsi2v.htm), wyd. II

[3] Forouzan, [Computer Networks: A Top-Down Approach](https://www.amazon.com/Computer-Networks-Top-Down-Approach/dp/0073523267/ref=sr_1_3?s=books&ie=UTF8&qid=1478721337&sr=1-3&keywords=forouzan+computer)

---
2016.11.17

- konfiguracja VLAN, IEEE 802.1Q
- trunking
- polaczenie z routerem przez telnet, konfiguracja tty

---
2016.11.17

- routing statyczny, brama domyslna
- tablice routingu, flagi, metryka
- konfiguracja trasowania routera i VPCSa
- ping, traceroute
- zmiana metryki w tablicy routingu
- routing dynamiczny, konfiguracja routera
- protokoly routingu dynamicznego, RIPv2, OSPF
- algorytm Bellmana-Forda, algorytm Dijkstry
- wplyw zmiany topologii sieci na tablice routingu

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

f) wlaczenie routingu dynamicznego RIPv2 na routerze R1 podlaczonego do sieci 192.168.1.0 oraz 192.168.3.0
```
R1#configure terminal
R1(config)#router rip
R1(config-router)no auto-summary
R1(config-router)version 2
R1(config-router)network 192.168.1.0
R1(config-router)network 192.168.3.0
R1(config-router)exit
R1(config)#exit





