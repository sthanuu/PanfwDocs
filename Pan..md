# Palo alto network Basics 
The strength of PAN firewall is Single-Pass Parallel processing (SP3)
Single pass
- Single pass Operation per packet
- One single policy (per type )

Parallel Processing 
- Parallel Processing harware engines
- Seperate control and data planes

Control Plane
- Responsible for tasks such as Management UI , logging and route updates
- Date plane responsible for 
1. Signature Matching
includes vulnerability exploits,virus,spyware,CC# and SSN
2. Security process handles security tasks such as APP-id,User-id, URL Match,policy match,ssl/ipsec,decompression
3. Network processor resposible for routing, NAT and network layer communication

**Zero Trust**
- is an alternative security model that address the short comings of failing preimeter centric strategies by removing the assumption of trust.

**Palo Alto Nezt generation firwalls**
1. PA-200 series
2. PA-3200 series
3. PA-5000 series
4. PA-7000 series
5. PA-800 Series

- PA 5280 has double the data plane memory which doubls the session capcity of PA-5260 firewall

**Virtual systems**
- single seperate , logical firewall instances within a single physical PAN firewall
- consists of physical & logical Interfaces and sub interfaces
- Virtual routers
- security zones
- Deployment mode can be the combination of Virtual wire,Layer 2,Layer 3 for each virtual system.

# Initial Configuration
**Administrative controls**
**Management Port**
1. Passes only the Management traffic for the firewall not to be cofigured as Standard Traffic Interface
2. It is used direct connectivity to the managemnt plane of the firewall 
3. The Direct serial console port of the Management port has to be set as RJ45, 9600 8,N,1
4. Default MGT-IP addressing 
-  Most firewall Models :- 192.168.1.1
-  VM series firewall - DHCP client
5. Default access - admin/admin

**Initial System access**
*Reset to factory setting using admin in CLI*
- > request system private-data-reset
*Reset to factory setting if not admin account password is not known from CLI*
- reboot the firewall in Maint mode type maint in cli and then chose reset to factory default

*Steps for MGT interface configuration Initialy*

1. Configure DT or LT ethernet interface in 192.168.1.0/24
2. Connect to the MGT port with an ethernet cable
3. Launch a web brower connection https://192.168.1.1
4. Login using firewall as admin/admin
5. Device -> setup -> Interface 
6. Click Management , configure the network setting for the MGT interface
7. Reconnect web interface with new network configuration 

*MGT interface configuration*
- By default https, ping, ssh are enable in the Mgt interface settings

*configure general settings*

- configure hostname & domain name 
- Enable if the management interface is configures as DHCp , u can enable the access DHCP server








