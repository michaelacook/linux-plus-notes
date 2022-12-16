# Networking cheat sheet

View all network devices 

```
nmcli dev show [device]
```

View all network connections

```
nmcli con show
```

Shut down a connection

```
nmcli con down "connection name"
```

Start up a connection

```
nmcli con up "connection name
```

View device statuses

```
nmcli device status
```

Create an autostarting connection

```
nmcli connection add con-name "connection name" type [type] ip4 [IP/mask] gw4 [default gateway] ifname [interface name]
autoconnect yes
```

Delete a connection

```
nmcli con delete "connection name"
```

Specify DNS for a connection

```
# can specify multiple DNS server IPs in sequence
# start the connection after adding DNS

nmcli con mod "connection name" ipv4.dns "[IP]"
nmcli con up "connection name"
```

Verify DNS for a connection

```
nmcli -f ipv4.dns con show "connection name"
```

Set up a new connection with prompts

```
nmcli con edit
```

Verify IP address 

```
ip address show
```

Verify routing table

```
ip route show
```

Change IP until next reboot/restart of interface

```
ip addr add [IP/mask] dev [interface]
```

Delete IP until next reboot/restart of interface

```
ip addr del [IP/mask] dev [interface]
```

Shut down network interface

```
ip link set [inteface] down
```

Start up a network interface

```
ip link set [interface] up
```

Modify the default route

```
ip route add default via [IP] dev [device]
```

Delete a route 

```
ip route del default via [IP] dev [device]
```

Set a new hostname

```
hostnamectl set-hostname "hostname"
```

Verify hostname 

```
hostname
```