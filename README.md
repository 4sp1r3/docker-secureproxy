## Requirement
+ Docker 1.0 & base-image ubuntu:trusty
+ A domain and an SSL certificate signed by a trusted CA, (e.g. [StartSSL.com](https://www.startssl.com))
+ Google Chrome

## Installation
1. Build image

	```bash
	$ sudo docker pull catatnight/secureproxy
	$ curl https://raw.githubusercontent.com/catatnight/docker-secureproxy/master/manage.py -o manage.py
	$ chmod +x manage.py

	# OR build on localhost
	$ git clone https://github.com/catatnight/docker-secureproxy.git
	$ cd docker-secureproxy
	$ sudo ./build.sh
	```

2. Save SSL certs in the same directory

	```bash
	$ mkdir -p certs
	$ cp {file.key,file.crt} certs/
	```

## Usage
1. Create container and manage it as root

	```bash
	$ sudo ./manage.py -h
	usage: manage.py [-h] [-p PROXY_PORT] [--radius_server RADIUS_SERVER]
				 [--radius_secret RADIUS_SECRET] [--ncsa_users NCSA_USERS]
				 {create,start,stop,restart,delete}
	# proxy authentication methods
	# 1) Uses a RADIUS server for login validation
	$ sudo ./manage.py create -p 1234 --radius_server radius.example.com --radius_secret radpass
	# 2) Uses an NCSA-style username and password file
	$ sudo ./manage.py create -p 1234 --ncsa_users user1:pwd1,user2:pwd2
	```

2. Using a Secure Web Proxy with Chrome by three optional ways
	1. add command-line argument ```--proxy-server=https://<your.proxy.domain>:<proxy_port>```
	2. proxy auto-config (PAC) file

		```
		function FindProxyForURL(url, host) {
			return "HTTPS <your.proxy.domain>:<proxy_port>";
		}
		```
	3. chrome extension [falcon proxy](https://chrome.google.com/webstore/detail/falcon-proxy/gchhimlnjdafdlkojbffdkogjhhkdepf)


## Note
+ squid3 needs to use port 3128
+ accounting information (data transfer) will be sent to a RADIUS server everyday by ```squid2radius```
+ swap needed on host machine since docker 0.10 (especially to DigitalOcean user)

## Reference
+ [Chrome - Secure Web Proxy](http://www.chromium.org/developers/design-documents/secure-web-proxy)
+ [tatsuhiro-t / nghttp2](https://github.com/tatsuhiro-t/nghttp2)
+ [jiehanzheng / squid2radius](https://github.com/jiehanzheng/squid2radius)
+ [tatsuhiro-t / spdylay](https://github.com/tatsuhiro-t/spdylay)
+ [搭建 Spdy SSL Proxy (二) - 洋白菜的博客](http://blog.chaiyalin.com/2013/07/spdy-ssl-proxy-2.html)
+ TBD
