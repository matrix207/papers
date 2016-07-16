---
layout: post
title: "how to do git push under vpn"
description: ""
category: tools 
tags: [git]
---

### Analysis
1. Generally, when using `ssh -vT git@github.com`, it would show as below:

		[dennis@localhost ~]$ ssh -vT git@github.com
		OpenSSH_6.1p1, OpenSSL 1.0.1e-fips 11 Feb 2013
		debug1: Reading configuration data /etc/ssh/ssh_config
		debug1: /etc/ssh/ssh_config line 50: Applying options for *
		debug1: Connecting to github.com [192.30.252.129] port 22.
		debug1: Connection established.
		debug1: identity file /home/dennis/.ssh/id_rsa type 1
		debug1: identity file /home/dennis/.ssh/id_rsa-cert type -1
		debug1: identity file /home/dennis/.ssh/id_dsa type -1
		debug1: identity file /home/dennis/.ssh/id_dsa-cert type -1
		debug1: identity file /home/dennis/.ssh/id_ecdsa type -1
		debug1: identity file /home/dennis/.ssh/id_ecdsa-cert type -1
		debug1: Remote protocol version 2.0, remote software version libssh-0.6.0
		debug1: no match: libssh-0.6.0
		debug1: Enabling compatibility mode for protocol 2.0
		debug1: Local version string SSH-2.0-OpenSSH_6.1
		debug1: SSH2_MSG_KEXINIT sent
		debug1: SSH2_MSG_KEXINIT received
		debug1: kex: server->client aes128-ctr hmac-sha1 none
		debug1: kex: client->server aes128-ctr hmac-sha1 none
		debug1: sending SSH2_MSG_KEX_ECDH_INIT
		debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
		debug1: Server host key: RSA xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx
		debug1: Host 'github.com' is known and matches the RSA host key.
		debug1: Found key in /home/dennis/.ssh/known_hosts:1
		debug1: ssh_rsa_verify: signature correct
		debug1: SSH2_MSG_NEWKEYS sent
		debug1: expecting SSH2_MSG_NEWKEYS
		debug1: SSH2_MSG_NEWKEYS received
		debug1: Roaming not allowed by server
		debug1: SSH2_MSG_SERVICE_REQUEST sent
		debug1: SSH2_MSG_SERVICE_ACCEPT received
		debug1: Authentications that can continue: publickey
		debug1: Next authentication method: publickey
		debug1: Offering RSA public key: /home/dennis/.ssh/id_rsa
		debug1: Server accepts key: pkalg ssh-rsa blen 279
		debug1: Authentication succeeded (publickey).
		Authenticated to github.com ([192.30.252.129]:22).
		debug1: channel 0: new [client-session]
		debug1: Entering interactive session.
		debug1: Sending environment.
		debug1: Sending env LC_MONETARY = en_HK.utf8
		debug1: Sending env LC_NUMERIC = en_HK.utf8
		debug1: Sending env XMODIFIERS = @im=ibus
		debug1: Sending env LANG = en_GB.utf8
		debug1: Sending env LC_MEASUREMENT = en_HK.utf8
		debug1: Sending env LC_TIME = en_HK.utf8
		Hi matrix207! You've successfully authenticated, but GitHub does not provide shell access.
		debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
		debug1: channel 0: free: client-session, nchannels 1
		Transferred: sent 2832, received 1816 bytes, in 1.5 seconds
		Bytes per second: sent 1950.6, received 1250.8
		debug1: Exit status 1
		[dennis@localhost ~]$ 

2. But when using VPN, it show as below:

		[dennis@localhost ~]$ ssh -vT git@github.com
		OpenSSH_6.1p1, OpenSSL 1.0.1e-fips 11 Feb 2013
		debug1: Reading configuration data /etc/ssh/ssh_config
		debug1: /etc/ssh/ssh_config line 50: Applying options for *
		debug1: Connecting to github.com [192.30.252.128] port 22.
		debug1: connect to address 192.30.252.128 port 22: No route to host
		ssh: connect to host github.com port 22: No route to host

3. Reason of the problem is that, your vpn setup routing all your traffic over 
   the VPN, you can update your routing table to route traffic to github back 
   over your ethernet interface rather than the VPN

4. Not run VPN

		[dennis@localhost ~]$ route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		default         192.168.1.1     0.0.0.0         UG    0      0        0 wlan0
		192.168.1.0     *               255.255.255.0   U     9      0        0 wlan0
		192.168.122.0   *               255.255.255.0   U     0      0        0 virbr0
		[dennis@localhost ~]$ 
			 
		[dennis@localhost ~]$ ifconfig
		lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
				inet 127.0.0.1  netmask 255.0.0.0
				inet6 ::1  prefixlen 128  scopeid 0x10<host>
				loop  txqueuelen 0  (Local Loopback)
				RX packets 9114  bytes 4451106 (4.2 MiB)
				RX errors 0  dropped 0  overruns 0  frame 0
				TX packets 9114  bytes 4451106 (4.2 MiB)
				TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

		p1p2: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
				ether XX:XX:XX:XX:XX:XX  txqueuelen 1000  (Ethernet)
				RX packets 0  bytes 0 (0.0 B)
				RX errors 0  dropped 0  overruns 0  frame 0
				TX packets 0  bytes 0 (0.0 B)
				TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

		virbr0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
				inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
				ether 52:54:00:f3:fa:32  txqueuelen 0  (Ethernet)
				RX packets 118  bytes 19982 (19.5 KiB)
				RX errors 0  dropped 0  overruns 0  frame 0
				TX packets 30  bytes 4505 (4.3 KiB)
				TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

		vnet0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
				inet6 fe80::fc54:ff:fedb:760f  prefixlen 64  scopeid 0x20<link>
				ether fe:54:00:db:76:0f  txqueuelen 500  (Ethernet)
				RX packets 118  bytes 21646 (21.1 KiB)
				RX errors 0  dropped 0  overruns 0  frame 0
				TX packets 6974  bytes 365825 (357.2 KiB)
				TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

		wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
				inet 192.168.1.100  netmask 255.255.255.0  broadcast 192.168.1.255
				inet6 fe80::c617:feff:fec3:892d  prefixlen 64  scopeid 0x20<link>
				ether XX:XX:XX:XX:XX:XX  txqueuelen 1000  (Ethernet)
				RX packets 39375  bytes 25884803 (24.6 MiB)
				RX errors 0  dropped 0  overruns 0  frame 0
				TX packets 46628  bytes 7804313 (7.4 MiB)
				TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

5. Using VPN

		[dennis@localhost ~]$ route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		default         *               0.0.0.0         U     0      0        0 ppp0
		10.0.4.4        *               255.255.255.255 UH    0      0        0 ppp0
		192.168.1.0     *               255.255.255.0   U     9      0        0 wlan0
		192.168.122.0   *               255.255.255.0   U     0      0        0 virbr0
		199.119.206.177 192.168.1.1     255.255.255.255 UGH   0      0        0 wlan0
		199.119.206.177 192.168.1.1     255.255.255.255 UGH   0      0        0 wlan0
				 
		[dennis@localhost ~]$ ifconfig
		...
		ppp0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1400
				inet 10.0.4.123  netmask 255.255.255.255  destination 10.0.4.4
				ppp  txqueuelen 3  (Point-to-Point Protocol)
				RX packets 9  bytes 114 (114.0 B)
				RX errors 0  dropped 0  overruns 0  frame 0
				TX packets 11  bytes 176 (176.0 B)
				TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
		...

6. get ip from ping github

		[dennis@localhost ~]$ ping github.com
		PING github.com (192.30.252.129) 56(84) bytes of data.

7. routing tables????(**NOTICE**: this section is unfinished.)

		[dennis@localhost ~]$ su -c 'route add 192.30.252.129 wlan0'
		Password: 
		[dennis@localhost ~]$ route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		default         *               0.0.0.0         U     0      0        0 ppp0
		10.0.4.4        *               255.255.255.255 UH    0      0        0 ppp0
		ip1d-lb3-prd.ia *               255.255.255.255 UH    0      0        0 wlan0
		192.168.1.0     *               255.255.255.0   U     9      0        0 wlan0
		192.168.122.0   *               255.255.255.0   U     0      0        0 virbr0
		199.119.206.177 192.168.1.1     255.255.255.255 UGH   0      0        0 wlan0
		199.119.206.177 192.168.1.1     255.255.255.255 UGH   0      0        0 wlan0

		[dennis@localhost ~]$ su -c 'route add  192.30.252.129  gw 192.168.1.1'

8. ***CONTINUE***

### Reference
* [git push/pull times out](http://stackoverflow.com/questions/757432/git-push-pull-times-out/757462#757462)
* [Using SSH over the HTTPS port](https://help.github.com/articles/using-ssh-over-the-https-port)
