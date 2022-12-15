# Linux networking

RFC 1918 IP class ranges

|Class|Range|Hosts|Mask|
|-|-|-|-|
|A|1-126|16,777,214|/8|
|B|128-191|65,534|/16|
|C|129-223|254|/24|
|D|224-239|Multicast||
|E|240-254|Reserved for future use/research & development||

Private IP addressing

|IP range|Hosts|CIDR Notation|Classful description|
|-|-|-|-|
|10.0.0.0 - 10.255.255.255|16,777,216|10.0.0.0/8|single class A network|
|172.16.0.0 - 172.31.255.255|1,048,576|172.16.0.0/12|16 contiguous class B networks|
|192.168.0.0 - 192.168.255.255|65,536|192.168.0.0/16|256 contiguous class C networks|

- Last host address on a subnet is always reserved as the broadcast address
  - i.e., for subnet 192.168.0.0 /24 the broadcast is 192.168.0.255
- First host on a subnet is usually the default gateway 
  - i.e., for subnet 192.168.0.0 /24 the default gateway is 192.168.0.1

## NetworkManager
- set of tools to create and modify different types of network connections
- may need to install it. on Debian-based distros, `sudo apt install network-manager`, similar on RHEL-based distros, just look it up
- NetworkManager deals with devices - the physical hardware - and connections - network connections on those devices
- `nmcli` - the NetworkManager CLI. used for configuring network devices and their connection settings
- `nmcli dev` - this is short for 'device' which is the physical hardware (such as a network interface card) that we use to connect to a network
  - `nmcli dev show` to view all network devices on the OS
    - pay attention to device, type, hardware address (MAC), IP address, gateway, route, DNS
    - `nmcli dev show [device]` to get more info about that device
- `nmcli con` - short for 'connection' which contains the network configuration settings assigned to a particular device. we assign our IP address and DNS settings to a connection
  - shut down a connection: `nmcli con down "connection name"`
  - start up a connection: `nmcli con up "connection name"`
- view interfaces with status: `nmcli device status`
- delete a connection: `nmcli con delete "connection name"`
- create a new connection with status IP and auto-start:
  - `nmcli connection add con-name "connection name" type [type] ip4 [IP/mask] gw4 [gateway] ifname [interface name] autoconnect`
- for setting up a new connection on a fresh install you can use `nmcli con edit` and follow the prompts
- specify DNS server for a connection: `nmcli con mod "connection name" ipv4.dns "[IP]"`
  - verify DNS: `nmcli -f ipv4.dns con show "[connection name]"`
  - (note: after specifying a DNS server you will need to start the connection again. Note that if it seems a configuration isn't taking effect, trying stoping and restarting the connection. Don't stop, just start the connection again to refresh)
- verify IP address: `ip address show`
- view routing rable: `ip route show`
- temporarily change interface IP: `ip addr add [IP/mask] dev [device]`
- temporarily delete interface IP: `ip addr del [IP/mask] dev [device]`
  - deleting the IP and setting a new one will be reverted when shutting down and restarting the interface
- shut down interface: `ip link set [device] down`
- turn on interface: `ip link set [device] up`
- modify default route: `ip route add default via [IP] dev [device]`
- delete a route: `ip route del default via [IP] dev [device]`
- set new hostname: `hostnamectl set-hostname "hostname"`
- view hostname: `hostname`


### Naming conventions for network interfaces
- en = ethernet, wl = wireless 
- new naming scheme (in order):
  - eno1   = for **onboard** devices, index provided by BIOS or firmware
  - ens1   = for devices in PCIe hotplug **slots**, index provided by BIOS or firmware
  - enp2s0 = for devices in specific **physical** locations
    - (p = bus, s = slot)
  - enx6a0002f08820 -  device names include MAC addresses, not used by default
  - eth0   = older, traditional naming

## Legacy tools
- `net-tools` package 
  - on RHEL `yum -y net-tools`
- `ifconfig` - previous standard networking utility on Linux. has been deprecated in favour of the `ip` command
  - change IP of an interface: `ifconfig [interface] [ip]`
    - this is temporary, will not persist restarting the interface
- `ifup` - used to bring up a specific network interface
- `ifdown` - used to bring down a specific network interface
- `route` - this utility will display the routing table, and add and delete routes. it has been deprecated in favour of the `ip route` command
  - supply `-n` option to replace names with IPs
  - delete default route: `route del default`
  - add default route: `route add default gw [default gateway]`
  - add a route to the routing table: `route add -net [destination subnet] netmask [mask] gw [destination gateway]`
    - this adds a new route in the routing table, so that all traffic destined for the destination subnet will be passed through the specified gateway


## Testing connectivity
- `ping` best way to test connectivity, see if DNS is functioning
  - `-6` to ping using IPv6
  - `-c` to specify number of ICMP packets to send
- `traceroute [destination IP]` - determine hops between host and destination. used to verify network routing and to look for breaks in network communication
  - use `-6 [IP]` option to run traceroute using IPv6
  - you can also use `traceroute6 [IP]`
- `tracepath` and `tracepath6` - replacement for traceroute
  - uses UDP rather than ICMP
    - this leads to a lot of no reply's because many networks won't reply to UDP pings
  - doesn't need any elevated privileges
- `netstat` - used to display network connections and their state on a system. can also be used to view the routing table. part of the `net-tools` package, it is deprecated
  - supply `-tl` option for just TCP
  - supply `-ul` option for UDP
  - supply `-p` to view processes associated with listening ports\
  - view routing table with `traceroute -r`
- `ss` - socket statistics 
  - modern equivalent of netstat, but does not show the routing table
  - some options used by netstat can be used by ss:
    - `ss -tl`

## DNS resolution
- port 53 is default for DNS
- local configuration files:
  - `/etc/hosts` - contains localhost entry mapped to the loopback addresses (IPv4&6). can also be used to map other hostnames to IP addresses
  - `/etc/hostname` - systems use this file for a computer's hostname. the hostnamectl command will write a system's new hostname to this file.
    - the hostname specified in this file will map to the IP of the network interface
  - `/etc/resolv.conf` - contains the IP address of the DNS name servers that the host will use for name resolution
  - `/etc/nsswitch.conf` 
    - name service switch file
    - among other tasks, this file is used to determine the order in which name resolution occurs
      - by default, the system is going to specify that the `/etc/hosts` file be consulted for name resolution first before going to a DNS server
      - could change this, but not recommended unless necessary as it could slow down the process of name resolution
- commands for querying hostnames
  - `host` - used to resolve domain names to IP addresses
    - requires `bind-utils` package
      - nameserver package
      - set of utils to query nameservers
      - to install on RHEL-based distros run `sudo dnf install bind-utils`
    - `host [hostname]`
      - returns results and specifies a "handled by" number
        - mail
        - A record - address record
        - MX record - email server
          - the number specifies the order in which the servers should be contacted, lower number = higher priority
  - `dig` - used to query DNS servers for particular types of DNS records
    - `dig @[alternate DNS] [hostname]`
    - query for specific type of DNS record with `dig -t [MX|A|etc] [hostname]`
    - can use `-t any` for all types of DNS records
      - `A` - IPv4
      - `AAAA` - IPv6
      - `MX` - email
      - `NS` - nameserver
  - `getent` - directly queries the `/etc/nsswitch.conf` file and its corresponding database locations for information
    - `getent hosts`

## Bonding and link aggregation
- bonding, or teaming, is a configuration that treats two or more network interfaces as a single network interface
- use multiple physical interfaces to create a single logical interface 
- high availability - one goes down, other interfaces remain up
- bonding modes:
  - Mode=1 *active-backup policy* - sets all interfaces ot the backup state while one remains active
  - Mode=2 *XOR policy* - selects an interface to transmit packages to based on the result of an XOR operation
  - Mode=4 *IEEE 802.3ad policy* - creates aggregation groups for which included interfaces share the speed and duplex settings
    - i.e., two Gigabit ethernet interfaces are bonded to create a single logical 2 Gigabit interface
    - requires a switch to support it
  - Mode=5 *adaptive transmit load balancing policy* - ensures the outgoing traffic distribution is according to the load on each interface, and that the current interface recieves all incoming traffic
    - load balancing between interfaces basically
- network bridging - combines two or more networks into a single logical network
  - often used in virtualization settings where a virtual guest's network is configured to communicate on the same network as the host system
    - install the `bridge-utils` package for RHEL and Debian distros
        - `brctl addbr br0`
          - assign interface to bridge `brctl addif br0 [interface]`
          - verify bridge with `brctl show` 