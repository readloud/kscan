# Kscan - Simple Asset Mapping Tool
<a href="https://github.com/lcvvvv/kscan"><img alt="Release" src="https://img.shields.io/badge/golang-1.6+-9cf"></a>
<a href="https://github.com/lcvvvv/kscan"><img alt="Release" src="https://img.shields.io/badge/kscan-1.62-ff69b4"></a>
<a href="https://github.com/lcvvvv/kscan"><img alt="Release" src="https://img.shields.io/badge/LICENSE-GPL-important"></a>
![GitHub Repo stars](https://img.shields.io/github/stars/lcvvvv/kscan?color=success)
![GitHub forks](https://img.shields.io/github/forks/lcvvvv/kscan)
![GitHub all release](https://img.shields.io/github/downloads/lcvvvv/kscan/total?color=blueviolet)

## 1 Introduction

````
|#| /#/ Lightweight Asset Mapping Tool by: kv2
|#|/#/  _____   _____ *  _     _   _
|#.#/  /Edge/  /Forum|  /#\   |#\ |#|
|##|   |#|__   |#|     /###\  |##\|#|
|#.#\  \#####\ |#|    /#/_\#\ |#.#.#|
|#|\#\ /\___|#||#|___/#/###\#\|#|\##|
|#| \#\\#####/ \####/#/v1.67\#|#| \#|

[+]2022/03/09 15:44:48 > The current environment is: linux, the output encoding is: utf8
````

kscan is an asset mapping tool that can perform port scanning, TCP fingerprinting and banner capture for specified assets, and obtain as much port information as possible without sending more packets. It can perform automatic brute force cracking on scan results, and is the first open source RDP brute force cracking tool on the go platform.

## 2 is written in front

At present, there are actually many tools for asset scanning, fingerprint identification, and vulnerability detection, and there are many great tools, but Kscan actually has many different ideas.

- Kscan hopes to accept a variety of input formats, and there is no need to classify the scanned objects before use, such as IP, or URL address, etc. This is undoubtedly an unnecessary workload for users, and all entries can be normal Input and identification. If it is a URL address, the path will be reserved for detection. If it is only IP:PORT, the port will be prioritized for protocol identification. Currently Kscan supports three input methods (-t,--target|-f,--fofa|--spy).

- Kscan does not seek efficiency by comparing port numbers with common protocols to confirm port protocols, nor does it only detect WEB assets. In this regard, Kscan pays more attention to accuracy and comprehensiveness, and only high-accuracy protocol identification , in order to provide good detection conditions for subsequent application layer identification.

- Kscan does not use a modular approach to do pure function stacking, such as a module obtains the title separately, a module obtains SMB information separately, etc., runs independently, and outputs independently, but outputs asset information in units of ports, such as ports If the protocol is HTTP, subsequent fingerprinting and title acquisition will be performed automatically. If the port protocol is RPC, it will try to obtain the host name, etc.

![kscan logic diagram.drawio](assets/kscan logic diagram.drawio.png)

## 3 Compilation Manual

[Compiler Manual](https://github.com/lcvvvv/kscan/wiki/%E7%BC%96%E8%AF%91)

## compile

### go version go1.16.10

*clone project source code
original source:
~~~
git clone https://github.com/lcvvvv/kscan
~~~

english version:
~~~
git clone https://github.com/alfazzafashion/kscan
~~~

*enter the project directory
~~~
cd kscan
~~~
*load submodule
~~~
git submodule update --init --recursive --remote
~~~
*install go module
~~~
go mod tidy
~~~
*compile executable
~~~
go build kscan
~~~

## 4 Let's get started

Kscan currently has 3 ways to input targets

- -t/--target can add the --check parameter to fingerprint only the specified target port, otherwise the target will be port scanned and fingerprinted

````
IP address: 114.114.114.114
IP address range: 114.114.114.114-115.115.115.115
URL address: https://www.baidu.com
File address: file:/tmp/target.txt
````

- --spy can add the --scan parameter to perform port scanning and fingerprinting on the surviving C segment, otherwise only the surviving network segment will be detected

````
[empty]: will detect the IP address of the local machine, and detect the B segment where the local IP is located
[all]: All private network addresses (192.168/172.32/10, etc.) will be probed
IP address: will detect the B segment where the specified IP address is located
````


- -f/--fofa can add --check to verify the survivability of the retrieval results, and add the --scan parameter to perform port scanning and fingerprint identification on the retrieval results, otherwise only the fofa retrieval results will be returned
````
fofa search keywords: will directly return fofa search results
````

## 5 How to use

````
usage: kscan [-h,--help,--fofa-syntax] (-t,--target,-f,--fofa,--touch,--spy) [-p,--port|-- top] [-o,--output] [-oJ] [--proxy] [--threads] [--path] [--host] [--timeout] [-Pn] [-Cn] [-- check] [--encoding] [--hydra] [hydra options] [fofa options]


optional arguments:
-h , --help show this help message and exit
-f , --fofa Get the detection object from fofa, you need to configure the environment variables in advance: FOFA_EMAIL, FOFA_TOKEN
-t , --target Specify the detection target:
IP address: 114.114.114.114
IP address segment: 114.114.114.114/24, subnet mask less than 12 is not recommended
IP address range: 114.114.114.114-115.115.115.115
URL address: https://www.baidu.com
File address: file:/tmp/target.txt
--spy network segment detection mode, in this mode, the internal network segment reachable by the host will be automatically detected. The acceptable parameters are:
(empty), 192, 10, 172, all, specified IP address (the IP address segment B will be detected as a surviving gateway)
--check Fingerprinting the target address, only port detection will not be performed
--scan will perform port scanning and fingerprinting on the target objects provided by --fofa and --spy
--touch Get the return package of the specified port, you can use this parameter to get the return package and improve the fingerprint library, the format is: IP:PORT
-p , --port scan the specified port, the default will scan TOP400, support: 80, 8080, 8088-8090
-o , --output save scan results to file
-oJ save the scan results to a file in json format
-Pn After using this parameter, intelligent survivability detection will not be performed. Now intelligent survivability detection is enabled by default to improve efficiency.
-Cn With this parameter, the console output will not be colored.
--top Scan the filtered common ports TopX, up to 1000, the default is TOP400
--proxy set proxy (socks5|socks4|https|http)://IP:Port
--threads thread parameter, the default thread is 100, the maximum value is 2048
--path specifies the directory to request access, only a single directory is supported
--host specifies the header Host value for all requests
--timeout set timeout
--encoding Set the terminal output encoding, which can be specified as: gb2312, utf-8
--hydra automatic blasting support protocol: ssh, rdp, ftp, smb, mysql, mssql, oracle, postgresql, mongodb, redis, all are enabled by default
hydra options:
--hydra-user custom hydra blasting username: username or user1,user2 or file:username.txt
--hydra-pass Custom hydra blasting password: password or pass1,pass2 or file:password.txt
If there is a comma in the password, use \, to escape, other symbols do not need to be escaped
--hydra-update Customize the user name and password mode. If this parameter is carried, it is a new mode, and the user name and password will be added to the default dictionary. Otherwise the default dictionary will be replaced.
--hydra-mod specifies the automatic brute force cracking module: rdp or rdp, ssh, smb
fofa options:
--fofa-syntax will get fofa search syntax description
--fofa-size will set the number of entries returned by fofa, the default is 100
--fofa-fix-keyword Modifies the keyword, and the {} in this parameter will eventually be replaced with the value of the -f parameter
````

The function is not complicated, the others are explored by themselves

## 6 Demo

### 6.1 Port Scan Mode

![WechatIMG986](assets/Kscan_v1.6/WechatIMG986.png)

### 6.2 Survival network segment detection

![WechatIMG988](assets/Kscan_v1.6/WechatIMG988.png)

### 6.3 Fofa result retrieval

![WechatIMG989](assets/Kscan_v1.6/WechatIMG989.png)

### 6.3 Brute-force cracking

![WechatIMG996](assets/Kscan_v1.6/WechatIMG996.png)

![WechatIMG996](assets/Kscan_v1.6/WechatIMG1018.png)

## 7 Special thanks

- [EdgeSecurityTeam](https://github.com/EdgeSecurityTeam)

- [bufferfly](https://github.com/dr0op/bufferfly)

- [EHole(Edge Hole)](https://github.com/EdgeSecurityTeam/EHole)

- [NMAP](https://github.com/nmap/nmap/)

- [grdp](https://github.com/tomatome/grdp/)

- [fscan](https://github.com/shadow1ng/fscan)

## 8 End of article

Github project address (bugs, requirements, rules are welcome to submit): https://github.com/lcvvvv/kscan
