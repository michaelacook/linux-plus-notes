# Server roles

## Core network servers
- DNS server
  - translate domain names to IPs
  - port 53 TCP/UDP
  - services provided by 
    - BIND - most common DNS
    - dnsmasq - DNS caching server
    - Unbound- DNS caching
- NTP server - Network Time Protocol
  - synchronize the date and time o a local system with an upstream, network-connected time provider
  - port 123 UDP
  - provided by:
    - chronyd - NTP server
    - ntpd - NTP server
    - systemd-timesyncd (client-only)
  - extrememly important for databases and authentication servers
  - Network Time Protocol
    - Atomic Clock -> NTP server -> stratum 2 NTP servers -> clients
- DHCP server
  - dynamically assign network clients IPs
  - port 67, 68 UDP
  - provided by:
    - dhcpd daemon
    - dnsmasq - dhcp service and dhcp caching

## Infrastructure servers
- authentication server - a centralized server on a network where multiple users and/or services can receive authentication credentials to network systems
  - centrally manage to who has access to what
  - port 389 TCP (LDAP)
  - port 636 TCP (encrypted LDAP (LDAPS))
  - 88 TCP (Kerberos)
  - services provided by:
    - OpenLDAP
    - LDAP - Lightweight Directory Access Protocol
      - store user login info
    - Active Directory
    - Kerberos - combine systems and users into a "realm"
    - Red Hat Identity Management service - sort of like Red Hat's version of Active Directory
- Certificate Authority Servers - known as PKI (public key infrastructure) systems, these platforms provide trusted certificates to hosts and clients to guarantee that the system a user connects to is a trusted system, and that the network communications between client and server are encrypted
  - e.g., connecting to your bank's website - you want HTTPS, not HTTP
  - 443 TCP (for SSL traffic to websites)
  - service provided by:
    - OpenSSL

- Load balancing servers - provide a means to route network traffic to various other servers based on network load. this is to prevent one server from receiving all of a network's traffic, thus causing it to become so saturated that it can no longer process requests
  - ports depends on the service being balanced
  - service provided by:
    - Apache, Nginzx (load balancing web traffic)
    - BIND, dnsmasq (load balancing DNS traffic)
    - HAProxy
    - Keepalived
- server clustering - practice of running more than one server that is configured to provide the same service. with this type of configuration, should one server fail, another is available to continue providing the service. server clustering is typically set up behind a load balancer to ensure service uptime and availability