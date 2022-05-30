Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

1. route-views>show ip route 194.186.207.140
Routing entry for 194.186.207.0/24
  Known via "bgp 6447", distance 20, metric 0
  Tag 6939, type external
  Last update from 64.71.137.241 2w5d ago
  Routing Descriptor Blocks:
  * 64.71.137.241, from 64.71.137.241, 2w5d ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 6939
      MPLS label: none


route-views>show bgp 194.186.207.140
BGP routing table entry for 194.186.207.0/24, version 2290015143
Paths: (23 available, best #19, table default)
  Not advertised to any peer
  Refresh Epoch 1
  8283 31133 35237
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 8283:1 8283:101 35237:600
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x18
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001
      path 7FE17E398A08 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  53767 174 31133 35237
    162.251.163.2 from 162.251.163.2 (162.251.162.3)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22028 53767:5000
      path 7FE0E6419428 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 3216 35237
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4477 3356:2 3356:22 3356:100 3356:123 3356:503 3356:903 3356:2067 3549:2581 3549:30840
      path 7FE16F417B28 RPKI State not found
      rx pathid: 0, tx pathid: 0
 .... More (там была очень большая "портянка")
 
 2. ![image](https://user-images.githubusercontent.com/75790619/171013619-22fff6eb-63c8-4da8-8918-c06e9d291dad.png)
 
 devops@devops-netology:~$ sudo ip route add 8.16.28.0/24 via 10.1.129.1
 devops@devops-netology:~$ sudo ip route add 8.8.28.0/24 via 10.1.129.1
 devops@devops-netology:~$ ip route
default via 192.168.106.2 dev ens33 proto dhcp src 192.168.106.150 metric 100
8.8.28.0/24 via 10.1.129.1 dev dummy0
8.16.28.0/24 via 10.1.129.1 dev dummy0
10.1.129.0/24 dev dummy0 proto kernel scope link src 10.1.129.0
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
192.168.106.0/24 dev ens33 proto kernel scope link src 192.168.106.150
192.168.106.2 dev ens33 proto dhcp scope link src 192.168.106.150 metric 100

3. ![image](https://user-images.githubusercontent.com/75790619/171015216-82e2b2ba-d465-4084-974c-3acc99921c35.png)

22 - (TCP,UDP) - используется под ssh соединение
53 - (TCP,UDP) - используется для DNS

4. ![image](https://user-images.githubusercontent.com/75790619/171015712-fbb808e6-83bb-4579-9d27-a51290efc94e.png)

5. ![image](https://user-images.githubusercontent.com/75790619/171018204-4030bf0d-8496-4957-bf84-c3f8a742e5c8.png)

