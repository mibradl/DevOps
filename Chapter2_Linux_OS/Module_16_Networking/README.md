# Chapter2
This module we learned the basic concepts of networking.

# Name: Networking Basics

# Description: 

Networking

1. How do computer networks work?

2. How computers connect to the internet?

3. What is an IP address and port?

4. What is a DNS?


LAN, Switch, Router, WAN, Gateway

Local Area Network - collection of devices connected together in one physical location. This can be as small as a home network, or as large as a huge office building.

Each device is represented by a unique IP address (Internet Protocol)

Devices communicate via these IP addresses  e.g. - 172.16.0.0
    32 bit value
    1 bit = 1 or 0
    IPv4 IP address is comprised of 32 bits, made up of 4 octets or 8 bits
    
    00000000 - 11111111 = 0 - 255
    0.0.0.0 - 255.255.255.255

How do devices talk to each other?

    This is achieved through a switch.

    The switch sits within the LAN

    Facilitates the connection between all the devices within the LAN

How to talk to devices outside the LAN

    Like Facebook, Youtube

    There is another device called a Router

    The Router sits between the LAN and the outside networks (WAN)

Wide Area Network connects devices on the LAN and WAN

    Allows networked devices to access the Internet


So a machine on the LAN will communicate through a router to Internet to connect to Facebook

    Gateway = IP address of the Router

How does a device know whether the other device is inside or outside the LAN?


Subnet

This is determined by the address of the target device

Devices in the LAN belong to same IP address range to identify devices in the same network, which is represented as a sub-network

Example of IP range: 192.168.0.0 255.255.255.255

1) IP address, which is the starting point of the IP range, the first IP in the range

2) Subnet Mask sets the IP range

    192.168.0.x - when the IP address start with this range it belongs to the LAN

    255.255.0.0 - means that 16 bits are fixed

    255.255.255.0 - means that 24 bits are fixed

        value 255 fixates the Octet

        value 0 means a free range

    Subnet Mask dictates how many bits are fixed

        CIDR = Classified Inter-Domain Routing
        Shorthand way of defining the Subnet Mask

        192.168.0.0/16 or 192.168.0.0/24
        /16 bits are fixed      /24 bits are fixed


Any device needs 3 pieces of data for communication:

    IP Address
    Subnet
    Gateway

Network Address Translation (NAT)

IP Address range chosen by an administrator

Each device gets a unique IP from that range

How to make sure that IP addresses don't overlap, when talking to devices in another LAN?

    The addresses that are part of the local area network are not visible to the WAN

    Your laptop's private IP address is replaced by the IP address of the router

    Network Address Translation is the key functionality of the router

    Private IP                               Public IP
    LAN 192.168.1.1 -------  ROUTER ------- 10.0.0.3 WAN


Benefits of NAT:

    1) Security and Protection of devices within LAN
    
    2) Reuse IP addresses without conflicting with each other

Same IP address range within their LAN is Ok, because they don't know about each other

Limited number of IPv4 addresses available:

    4,294,967,296 public addresses

Firewall

By default, the server is not accessible from outside the LAN

What is a FW? A system that prevents unauthorized access from entering a private network

Using Firewall Rules you can define which requests are allowed

Firewall Rules

Define on your network which IP address in your network is accessible

Which IP addresses can access your server

You can e.g. allow any device access to your server

Inbound

Type            Protocol            Port Range          Sources

SSH             TCP                 22                  178.191.162.12

Custom          TCP                 3000                All IPv4

Custom          TCP                 8081                All IPv4


What is a Port?

Every device has a set of ports

Ports are like doors in an apartment building, which represents a home

You can allow specific ports (doors) - e.g. - you unlock some doors so the guest are allowed access for vistors to visit you. But others are locked out

So, you define specific doors (App) that are accessible to the customer (IP Address)

Different applications listen on specific ports and you can connect to that device only on that port

Standard ports for many applications

Browser request port is 80
Mysql DB is 3306
PostgresQL is 5432

For every application you need a port to communicate

Each port is unique on a device

If you try to configure the same port more than once you will get an error that port is already in use.

FW allows device IP address at port to be accessed

PORT FORWARDING

DNS - Domain Name Service

Every device is uniquely identified by an IP address

When you enter the browser you do not use the IP address (machine name)

Why do we use names instead of IP addresses?

    Humans are better at remembering words and names instead of numbers

    Hence, the concept of mapping names to IP addresses was introduced

    We access the application on a device by typing in the name

    That name gets translated to an IP address, thru DNS

How does DNS manage all these IP addresses?

    Domain Names have a hierarchical structure

    Root Domains (apprx 13)

    Top Level Domains (apprx 6 TLDs)

    .mil - military
    .edu - educational institutions
    .com - commercial general purpose
    .org - non-profit organization
    .net - networking technologies
    .gov - government

    Others added later

    .biz
    .int
    .dev

    Geographical Domains

    .at - Austria
    .de - Germany
    .us - USA
    .fr - France

You can buy a domain name e.g. mywebsite.com

Who manages these names?

Who can sell these domain names?

Who keeps track of availability of names?

    Internet Corpation for Assigned Names and Numbers (ICANN)

    Manages the TLD development and architecture of the internet domain space

    Authorizes Domain Name Registrars, which register and assign domain names

                                Subdomains

Root                                .      

Top Level Domain Name               .com

                                    techworld-with-nana

Subdomain                   bootcamp    workshops       courses

e.g. 

www.bootcamp.techworld-with-nana.com



Fully Qualified Domain Name

courses.techworld-with-nana.com.    . stands for Root Domain

Not necessary to specify the dot at the end

How does DNS Resolution work?

    Every device has DNS client on the machine

    When you type in www.facebook.com your device will automatically make a DNS query (Resolver)

    Usually you Internet Service Provider (ISP)

    It will go to the 13 Root Servers

    Requests for top level domains

    These are redundantly around the world, so you get a response quickly

    The Root server sends a response "ask .com server"

    The Resolver server reaches out to the TLD server

        Stores the address information for TLD

        sends "here auth. name server list to Resolver server"

    Authoritative Name Server

        Responsible for knowing everything about the domain, including IP address

    (Authoritative) Name server

        "here is the IP address of facebook.com"

    Now your computer can send this request to facebook.com


    CACHE

    To make future queries more efficient DNS Entries are cached for efficiency



    Network Commands

    ifconfig - IP, subnet mask, gateway

    netstat - shows active connections on the machine listening on their specific ports

    ps aux - monitors PIDs and resources they are consuming

    nslook - will allow you to determine IP address for a DNS Name

    ping - checks if a service is accessible or available






# Usage

 ifconfig

lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
	inet 127.0.0.1 netmask 0xff000000 
	inet6 ::1 prefixlen 128 
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1 
	nd6 options=201<PERFORMNUD,DAD>
gif0: flags=8010<POINTOPOINT,MULTICAST> mtu 1280
stf0: flags=0<> mtu 1280
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=50b<RXCSUM,TXCSUM,VLAN_HWTAGGING,AV,CHANNEL_IO>
	ether 3c:7d:0a:17:77:a2 
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect (none)
	status: inactive
en4: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether ac:de:48:00:11:22 
	inet6 fe80::aede:48ff:fe00:1122%en4 prefixlen 64 scopeid 0x6 
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect (100baseTX <full-duplex>)
	status: active
ap1: flags=8802<BROADCAST,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether 7e:52:30:a4:08:8a 
	media: autoselect
	status: inactive
en1: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=6463<RXCSUM,TXCSUM,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	ether 5c:52:30:a4:08:8a 
	inet6 fe80::45a:9067:2bd4:4b27%en1 prefixlen 64 secured scopeid 0x8 
	inet 192.168.4.186 netmask 0xfffffc00 broadcast 192.168.7.255
	inet6 fd2d:e88d:ea7d:1:868:2273:a61a:dbc5 prefixlen 64 autoconf secured 
	inet6 2600:1700:25c2:425f:468:e52e:95a4:e2c5 prefixlen 64 autoconf secured 
	inet6 2600:1700:25c2:425f:3851:9673:d50c:1463 prefixlen 64 deprecated autoconf temporary 
	inet6 2600:1700:25c2:425f:5de9:8f0c:1326:c4fe prefixlen 64 deprecated autoconf temporary 
	inet6 2600:1700:25c2:425f:99ec:d069:88f9:7654 prefixlen 64 deprecated autoconf temporary 
	inet6 2600:1700:25c2:425f:8c02:c2f6:92f6:c255 prefixlen 64 deprecated autoconf temporary 
	inet6 2600:1700:25c2:425f:e04b:269a:e796:4d76 prefixlen 64 deprecated autoconf temporary 
	inet6 2600:1700:25c2:425f:f173:e036:f092:5314 prefixlen 64 autoconf temporary 
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
en2: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=460<TSO4,TSO6,CHANNEL_IO>
	ether 82:ac:3a:01:94:00 
	media: autoselect <full-duplex>
	status: inactive
awdl0: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether 4a:bd:9f:49:78:44 
	inet6 fe80::48bd:9fff:fe49:7844%awdl0 prefixlen 64 scopeid 0xa 
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
en3: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=460<TSO4,TSO6,CHANNEL_IO>
	ether 82:ac:3a:01:94:01 
	media: autoselect <full-duplex>
	status: inactive
bridge0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=63<RXCSUM,TXCSUM,TSO4,TSO6>
	ether 82:ac:3a:01:94:00 
	Configuration:
		id 0:0:0:0:0:0 priority 0 hellotime 0 fwddelay 0
		maxage 0 holdcnt 0 proto stp maxaddr 100 timeout 1200
		root id 0:0:0:0:0:0 priority 0 ifcost 0 port 0
		ipfilter disabled flags 0x0
	member: en2 flags=3<LEARNING,DISCOVER>
	        ifmaxaddr 0 port 9 priority 0 path cost 0
	member: en3 flags=3<LEARNING,DISCOVER>
	        ifmaxaddr 0 port 11 priority 0 path cost 0
	nd6 options=201<PERFORMNUD,DAD>
	media: <unknown type>
	status: inactive
utun0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1380
	inet6 fe80::222d:cf68:5749:854d%utun0 prefixlen 64 scopeid 0xd 
	nd6 options=201<PERFORMNUD,DAD>
utun1: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 2000
	inet6 fe80::e470:5670:241b:4d08%utun1 prefixlen 64 scopeid 0xe 
	nd6 options=201<PERFORMNUD,DAD>
utun2: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1000
	inet6 fe80::ce81:b1c:bd2c:69e%utun2 prefixlen 64 scopeid 0xf 
	nd6 options=201<PERFORMNUD,DAD>
llw0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether 4a:bd:9f:49:78:44 
	inet6 fe80::48bd:9fff:fe49:7844%llw0 prefixlen 64 scopeid 0x10 
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
utun3: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1380
	inet6 fe80::539f:c385:5a16:6825%utun3 prefixlen 64 scopeid 0x11 
	nd6 options=201<PERFORMNUD,DAD>
utun4: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1380
	inet6 fe80::b87f:7d10:bd73:1d59%utun4 prefixlen 64 scopeid 0x12 
	nd6 options=201<PERFORMNUD,DAD>
en5: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether 08:26:ae:3f:af:b9 
	inet6 fe80::c1:12c5:ac12:d3f7%en5 prefixlen 64 secured scopeid 0x5 
	inet6 2600:1700:25c2:4250:1844:5cbc:cbd0:4499 prefixlen 64 autoconf secured 
	inet6 2600:1700:25c2:4250:e0e3:7325:2057:84cd prefixlen 64 autoconf temporary 
	inet 192.168.1.250 netmask 0xffffff00 broadcast 192.168.1.255
	inet6 2600:1700:25c2:4250::41 prefixlen 64 dynamic 
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect (1000baseT <full-duplex>)
	status: active


