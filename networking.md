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

###Example with WordPress site and two VMs
An example was when I had two VMs running on one host: a webserver and a database server. I wanted to connect the two VMs. The database server was using port forwarding to run on the host machine's localhost at port 3306, therefore, in order to connect the VMs, I simply had to get the webserver VM to connect to the host. By writing the host's gateway address into the host section of the database url in the webserver's connection options, the webserver was able to find the database server and connect.

##Example nginx server configuration for HTTPS redirect
```bash
server {
	listen 80 default_server;
	listen [::]:80 default_server ipv6only=on;

	listen 443 ssl;

	server_name example.com;

	ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

	root /usr/share/nginx/html;
	index index.html index.htm;
	server_name localhost;

	location ~ \.well-known\/acme-challenge\/.+ {
		try_files $uri $uri/ =404;
	}
	
	location / {
		return 301 $scheme://www.example.com$request_uri;
	}
	
}
```

If hosting the app on Heroku, in addition to the above configuration, you will need the following records and config:
```bash
APEX = @ => example.com
CNAME = @ => www.example.com
CNAME = @ => www.example.com.herokudns.com
```

Heroku config vars:
```bash
ACME_KEY_1 = 123example123
ACME_TOKEN_1 = 123example123
HOME_URL = https://www.example.com
SITE_URL = https://www.example.com
```

In order to run a proxy (not necessary for this task), you can add the following lines to the location block:
```bash
proxy_set_header Host $host;
proxy_pass https://example.herokuapp.com/;
```