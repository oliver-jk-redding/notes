#Notes on Networking

##How to get guest VM to talk to host machine
On the guest machine, run the command `netstat -rn`.
It should display output like this:
```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 wlan0
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 wlan0
0.0.0.0         192.168.1.1     0.0.0.0         UG        0 0          0 wlan0
```
The host address is the Gateway corresponding to destination 0.0.0.0, in this case 192.168.1.1. The guest machine can connect to the host via this gateway.

###Example with WP site and two VMs
An example was when I had to VMs running on one host: a webserver and a database server. I wanted to connect the two VMs. The database server was using port forwarding to run on the host machine's localhost at port 3306, therefore, in order to connect the VMs, I simply had to get the webserver VM to connect to the host. By writing in the host's gateway address into the host section of the database url, the webserver was able to find the database server and connect.