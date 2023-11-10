VLAN 100 10.0.0.128/26 - 10.0.0.191 255.255.255.192
VLAN 200 10.0.0.0/25 - 10.0.0.127     255.255.255.128
VLAN 300 10.0.0.192/27 - 10.0.0.223  255.255.255.224
M2R1 – M2R2 100.0.0.0/30 - 1;2         255.255.255.252
LAN1 172.16.0.0/24 - 172.16.0.255      255.255.255.0


---------------------ipv4--------------------------------
M2S1
en
conf t
hostname M2S1
banner motd #This Network is Secured by BTU#
line console 0
password btu-mid-con
login 
exit
vlan 100 
name marketing
vlan 200 
name sales
vlan 300 
name management
vlan 400 
name native
vlan 500 
name unused
exit
int fa0/8
switchport mode access
switchport access vlan 200
int fa0/6
switchport mode access
switchport access vlan 100
int fa0/3
switchport mode access
switchport access vlan 300
int range fa0/4-5
switchport mode trunk
switchport trunk native vlan 400
int range fa0/7, fa0/1, fa0/9-24, gi0/1-2
switchport mode access 
switchport access vlan 500
shutdown
exit
int vlan 300
ip address 10.0.0.197 255.255.255.224
exit
ip default-gateway 10.0.0.194
exit
exit
copy run-start




M2S2
en
conf t
hostname M2S2
banner motd #This Network is Secured by BTU#
line console 0
password btu-mid-con
login 
exit
vlan 100 
name marketing
vlan 200 
name sales
vlan 300 
name management
vlan 400 
name native
vlan 500 
name unused
exit
int fa0/2
switchport mode access
switchport access vlan 200
int fa0/4
switchport mode access
switchport access vlan 100
int range fa0/3
switchport mode trunk
switchport trunk native vlan 400
int range fa0/1, fa0/5-24, gi0/1-2
switchport mode access 
switchport access vlan 500
shutdown
exit
int vlan 300
ip address 10.0.0.198 255.255.255.224
exit
ip default-gateway 10.0.0.194
exit
exit
copy run-start


M2R1
en
conf t
hostname M2R1
enable secret btu-mid
int gi0/1
description link_To_M2R2
ip address 100.0.0.1 255.255.255.252
no shutdown
exit
int gi0/0
no shutdown
exit
int gi0/0.100
encapsulation dot1q 100
ip address 10.0.0.130 255.255.255.192
int gi0/0.200
encapsulation dot1q 200
ip address 10.0.0.2 255.255.255.128
int gi0/0.300
encapsulation dot1q 300
ip address 10.0.0.194 255.255.255.224
exit
router eigrp 10
no auto-summary
network 10.0.0.128 0.0.0.63 
network 10.0.0.0 0.0.0.127
network 10.0.0.192 0.0.0.31
network 100.0.0.0 0.0.0.3
passive-interface gi0/0
exit
exit
copy run start




M2R2
en
conf t
hostname M2R2
enable secret btu-mid
int gi0/1
ip address 100.0.0.2 255.255.255.252
no shutdown
exit
int gi0/0
ip address 172.16.0.2 255.255.255.0
no shutdown
exit
router eigrp 10
no auto-summary
network 172.16.0.0 0.0.0.255
network 100.0.0.0 0.0.0.3
passive-interface gi0/0
exit
ip domain-name btu.ge
crypto key generate rsa general-keys modulus 4000
username exam-mid secret btu-mid-ssh
line vty 0 15
transport input ssh
login local
exit
exit
copy run start
x


-------------------------------ipv6--------------------------
2002:db8:b2b2:0000:  0:  0:  0:  0
2002:db8:b2b2::/48

2002:db8:b2b2:0::/64 - LAN1
2002:db8:b2b2:1::/64 - LAN2
9
R1
int gi0/1
ipv6 address 2002:db8:b2b2::b/64
ipv6 address fe80::10 link-local
no shutdown
exit
int se0/3/0
ipv6 address 2002:db8:acad::9/64
ipv6 address fe80::10 link-local
no shut
exit

ipv6 unicast-routing
ipv6 route 2002:db9:acad::/64 se0/3/0 fe80::20
ipv6 route 2002:db8:b2b2:1::/64 se0/3/0 fe80::20


R2
int gi0/1
ipv6 address 2002:db9:acad::10/64
ipv6 address fe80::20 link-local
no shutdown
exit
int se0/3/0
ipv6 address 2002:db8:acad::a/64
ipv6 address fe80::20 link-local
no shut
exit

ipv6 unicast-routing
ipv6 route 2002:db8:b2b2::/64 se0/3/0 fe80::10
ipv6 route 2002:db8:b2b2:1::/64 gi0/1 fe80::30


R3
int gi0/1
ipv6 address 2002:db8:b2b2:1::b/64
ipv6 address fe80::30 link-local
no shutdown
exit
int gi0/0
ipv6 address 2002:db9:acad::11/64
ipv6 address fe80::30 link-local
no shut
exit

ipv6 unicast-routing
ipv6 route 2002:db8:acad::/64 gi0/0 fe80::20
ipv6 route 2002:db8:b2b2::/64 gi0/0 fe80::2
