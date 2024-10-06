# DNS
- `/etc/resolv.conf`

## Domain Names
- . - root
- com - top level domain name 
- google - domain name is assigned to gugle
- www - subdomain 

```bash
dig www.google.com
```

## Network namespaces

```bash
# create new network namespace on Linux host
ip netns add red # where red is namespace name
ip link # list namespaces

ip nets exec red ip link # will list namespaces from the red container 
ip -n red link # same command as above but simpler

arp

# link two network namespaces
ip link add veth-red type veth peer name veth-blue

ip link set veth-red netns red

# assign ip address within namespace
ip -n red addr add 192.168.15.1 dev veth-red

```
