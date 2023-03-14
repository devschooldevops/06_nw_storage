# Networking

## IP Configuration
- Look at your current network configuration: hostname, IP addresses, subnet mask, default gateway, DNS domain and DNS servers. Write this down somewhere
```bash
 hostname
 ip addr
 ip route
 cat /etc/resolv.conf
```
- The ip tool is the new tool to inspect and modify your network configuration, and is installed by default. The LPI exam also requires knowledge of the 'deprecated' tools ifconfig and route. Install these and use them to look at your network configuration as well.
```bash
#install ifconfig with yum
 yum -y install net-tools
 ifconfig
 route -n
```
## DNS Configuration
- Find out what IP address "dns.google.come" has
```bash
host dns.google.com
```
- Change your /etc/hosts file. Deliberately put in a wrong address for "dns.google.come"
```bash
 vi /etc/hosts
9.9.9.9 dns.google.com
```
- Perform a ping to "dns.google.come" Which address is being used?
```bash
ping dns.google.come
```
- Change your /etc/nsswitch.conf file. Look for the "hosts" line, and change it so that dns is used before files.
````bash
 vi /etc/nsswitch.conf
hosts: dns files myhostname
````
- Perform another ping to dns.google.come. Which address is used?
````bash
ping dns.google.come
````
- Restore the /etc/hosts and /etc/nsswitch files to their former configuration.

## Network Troubleshooting
- Use the ethtool to look at the status of your adapters at the link level. Which modes are supported, and which mode is chosen?
```bash
 ethtool ens33
```

- Use ip and ifconfig to look at your adapters at the IP level. Do you see the IP address, subnetmask, MTU and adapter state?
```bash
 ip addr
 ifconfig
```
- Look at your route table using route, netstat -r and ip route. What is your default gateway? Is this route active?
```bash
 route
 netstat -r
 ip route
```
- Perform a ping to 8.8.8.8. Also try a ping -R, traceroute and tracepath to this address.
```bash
 ping 8.8.8.8
 ping -R 8.8.8.8
 traceroute 8.8.8.8
 tracepath 8.8.8.8
```
- Check your /etc/resolv.conf to see which name servers are active.
```bash
 cat /etc/resolv.conf
```
- Use host and dig to perform an IP address lookup of the address dns.google.com. Does this work? Also do a reverse lookup with dig.
```bash
 host dns.google.com
 dig @server dns.google.com a
 host 194.109.6.92
```
- Use the netstat -a command to look at all open ports on your system. Only look for TCP and UDP ports, prevent reverse DNS lookups and show the PID of the process that owns these ports.
```bash
 netstat -anutp
```
- Use the nmap command to perform a portscan on your local system. Does this match the output of the previous command?
```bash
 yum -y install nmap
 nmap -sS -T5 127.0.0.1
```
- Use tcpdump to view all network traffic. (Note: If you are logged in via SSH, filter out the SSH traffic to make sure you don't dump tcpdump traffic.)
```bash
 tcpdump -i ens33 -ln not port ssh
```
- In a different window or session, generate some traffic, for instance by performing a ping.