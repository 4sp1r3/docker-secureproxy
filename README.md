## Requirement
+ Docker 1.0 & base-image ubuntu:trusty
+ A domain and an SSL certificate signed by a trusted CA, (e.g. [StartSSL.com](https://www.startssl.com))
+ Google Chrome

## Usage
1. Clone the git repo

	```bash
	$ git clone https://github.com/catatnight/docker-secureproxy.git
	$ cd docker-secureproxy
	```

2. Save your own ```.key``` and ```.crt``` files in ```certs/```
3. Build container and then manage it as root

	```bash
	$ sudo ./build.sh

	$ sudo ./manage.py -h
	usage: manage.py [-h] [--proxy_port PROXY_PORT]
				 [--radius_server RADIUS_SERVER]
				 [--radius_secret RADIUS_SECRET] [--ncsa_users NCSA_USERS]
				 {create,start,stop,restart,delete}
	# proxy authentication methods
	# 1) Uses a RADIUS server for login validation
	$ sudo ./manage.py create --proxy_port 1234 --radius_server radius.example.com --radius_secret radpass
	# 2) Uses an NCSA-style username and password file
	$ sudo ./manage.py create --proxy_port 1234 --ncsa_users user1:pwd1,user2:pwd2
	```

4. Using a Secure Web Proxy with Chrome by three optional ways
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
