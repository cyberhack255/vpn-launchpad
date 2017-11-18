# vpn-launchpad
Simple scripts for launching Amazon EC2 instances with L2TP and Shadowsocks VPN running out of box


## Why vpn-launchpad?
With vpn-launchpad, you can:
 - Create VPN server on demand with the rate as low as $0.0058 per hour (t2.nano instance locate in Ohio) ATM.
 - Destory the server after using to avoid trail leaking as well as unnecessary cost.


## Prerequisites
A Linux system (Ubuntu, Debian RHEL/CentOS etc.) or MacOSX system with ssh, nc and awsclii (Amazon Command line Interface) installed  is necessary for running vpn-launchpad

Simple instructions for awscli installation on Ubuntu/Debian Linux
```
$ sudo apt-get update
$ sudo apt-get install python-pip
$ pip install awscli --upgrade --user
$ echo 'export PATH=$PATH:~/.local/bin' >>.bashrc 
$ export PATH=$PATH:~/.local/bin
$ aws --version
aws-cli/1.11.161 Python/2.7.13 Linux/4.12.1-kirkwood-tld-1 botocore/1.7.19
```

Simple instructions for awscli installation on MacOSX10
 - Download Python-3.6.3 from the following link <https://www.python.org/ftp/python/3.6.3/python-3.6.3-macosx10.6.pkg>
 - Double click downloaded file to install it
 - Run the following command from Terminal
```
$ curl -O https://bootstrap.pypa.io/get-pip.py
$ python3 get-pip.py
$ pip3 install --user awscli
```
Following this link to config credential for awscli
<http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html>


## AWS free-tier user
If you are AWS free-tier user (First 12 month after your AWS account registeraion). You can run the scripts out off the box after filling ~/.aws/credential with your credential. Follow this guider please for more details:
<http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html>


## Non-free tier AWS user
Please modify the "INSTYPE" field in vlp-build from "t2.micro" into "t2.nano" to half your hourly rate.


## How to create VPN server:
`$ ./vlp-build [OPTIONAL-STACK-NAME]`


## How to query existing server status:
`$ ./vlp-query [OPTIONAL-STACK-NAME]`


## How to remove the existing VPN server instance and the resources associated:
`$ ./vlp-purge [OPTIONAL-STACK-NAME]`


## How can I change the L2TP login credentials? Username, password, preshared secret, etc:
 - Modify USERS and PSK configurations in docker-sevpn/sevpn.env please
 - Run vlp-purge to destroy existing VPN server instance if any
 - Run vlp-build to create a new VPN server with updated login credential.

Here's what docker-sevpn/sevpn.env looks like
```
$ cat docker-sevpn/sevpn.env
PSK=YOUR-SHARED-SECRET
USERS=user0:pass0;user1:pass1;
SPW=SRV-MGT-PASS
HPW=HUB-MGT-PASS
```
Please refer softethervpn project for more details. All credits to Tomohisa Kusano:
<https://github.com/siomiz/SoftEtherVPN>


## How to setup L2TP client on iPhone, Android, Linux, Mac or Windows:
<https://www.softether.org/4-docs/2-howto/9.L2TPIPsec_Setup_Guide_for_SoftEther_VPN_Server>


## How can I change the shadowsocks configuration like port, password and encryption method?
 - Modify the fields in docker-shadowsocks-libev/shadowsocks-libev.sh accordingly.
 - Run vlp-purge to destroy existing VPN server instance if any.
 - Run vlp-build to create a new VPN server with updated login credential.

Here's the sample configuration in shadowsocks-libev.sh
```
$ cat shadowsocks-libev.sh
#!/bin/sh

SSPASS="YOUR-SHADOWSOCKS-PASS"
SSTCPPORT="8388"
SSUDPPORT="8388"
SSMETHOD="aes-256-cfb"
...
```
