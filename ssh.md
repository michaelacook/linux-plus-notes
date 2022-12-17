# Secure Shell
- open a secure remote terminal
- `~/.ssh`
  - `known_hosts` - keeps track of known hosts you have logged into

## Configuration
- login with keys instead of password
  - `ssh-keygen`
    - accept defaults
    - add passphrase if wanted
    - creates `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`
    - `id_rsa` is a private key and should stay on your local machine - used to encrypt traffic from the client side of the connection
    - `id_rsa.pub` is the public key sent to the server
    - send public key to the server using `ssh-copy-id [user@hostname|IP]`
- server-side configuration located in `/etc/ssh`
  - global client configuration in `ssh_config`, any configs in `~/.ssh` for a user account will override the global config
  - `sshd_config` is configuration for the ssh daemon or service
    - PermitRootLogin is commented out - keep it that way! do not allow root login over ssh, hackers are trying to login to the root account on your system
    - can allow specific users with directive `AllowUsers` followed by comma-separated list of user accounts that can login over ssh
    - same with groups
    - disabled password authentication and use only keys by setting `PasswordAuthentication no`
      - this is more secure

## SSH and TCPWrappers
- way to protect security by allowing or denying hosts to access the system
- `/etc/hosts.allow`
  - specify hosts to allow to access the system
  - overrides `hosts.deny`
  - for a specific service like ssh, use directive `sshd: [IP, IP, ..., IP]`
    - you can also specify a subnet mask for IP subnets
      - i.e., `ALL: 192.168.2.0/24`
    - specify comma-separated list of hosts/IPs that can access the system
    - can also specify `ALL` option and the host is denied through all ports
- `/etc/hosts.deny`
  - this file is overridden by `hosts.allow`
  - Here you could deny all connections, then specify specific connections to allow in `hosts.allow` for extra security
  - when restricting access over ssh remember the directive uses **sshd**