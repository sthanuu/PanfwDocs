# Palo alto network Basics #
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
- Enable if the management interface is configures as DHCP , then  enable the access DHCP server
- By default MGT port is used to access external services like update services,DNS services,NTP  services etc.
- Optionally configure an in-band port to access external services , for enabling in-band port , service route configuration is mandatory which can configure the source interface and ip address 

#Configuration Management#
1. Running configuration (running-config.xml) is the actual configuration controlling the operation of the firewall.
2. During Firewall Startup running config is copies to the candidate config . In progress edits are made to the candidate config.
3. After commit the candidate config overwrites the current running config 

**Global Configuration Managment**
- Revert , save , load operations all manage configurations local to the firewall
- Export operations transfers xml formatted files from the firewall to the host running the web interface
- Import - transfers xml formatted files from the host to the firewall running the web interface

**Boot Process**
- On boot the latest configuration on disk is loaded to the coandidate configuration on the control plan memory , auto commit copies the candidate configuration to the running configuration in the control plan memory and then pushed to the running configuration in the data plan memory where it is use to inspect and control traffic.
- If admin do any changes which is saved in candidate configuration and once the commit is made the changes are writes to the running configuration in data and control plan memory and firewall saves the running configuration with the latest date and time stamp .

#Licensing and software updates#
- Acitvate the firewall :- Register -> activate support license -> activate license for each subscription purchased in palo alto networks 
- Firewall Must be configured with the Ip address, net mask , default gateway and DNS server IP address before activating the license . 
- Software updates :- Device -> software

#Interface Configuration#
**#Security zones & Interface**

By default security policy allows intrazone traffic which allows systems in same zone can communicate with each other .however interzone traffic is denied .

**Inbank Network Interfaces**
- single slot firewall
- Mutislot firewall
- Logical interface 
- Each fireall interface supports mutiple logical interface called subinterfaces in the web interface
- A physical port  or sub interface can be assigned to only single security zone. A zone can contain mutiple physical or logical interface .

**Interface types and zones**
- Tap Zone and Interfaces
- Virtual wire zone and Interfaces
- Layer 2 zone and Interfaces
- Tunnel Zone no interfaces assigned
- Layer 3 zones , Layer 3 , Vlan, loopback, Tunnel interfaces - These interfaces are assigned IP address.
- HA interface
1. not used to control normal network traffic
2. Not placed in security zone
3. used for synchorination of a pair of firewalls deployed in a high availability configuration
- MGT interface is used for firewall management not assigned to zone

**Tap Interface**

1. Enable passive monitoring of switch traffic from the SPAN or mirror port
2. cannot control traffic or perform traffic shaping
3. If the span or mirror passes encrypted traffic , the tap interface supports only SSL inbound decryption
4. even though a firewall doesn't block the traffic flowing into the tap interface , the firewall still thorughly  identify the traffic

**Configuring the Tap interface**
1. Network -> interface -> interface type "set as Tap" -> Security zone  "Select as per ur requirement"

**Virtual Wire Interface**

1. Binds two firewall interface together
2. Virtual wire configuration is required , when there is no switching or routing required.No configuration required for the adjacent network devices
3. Network traffic flows through a firewall in a virtual wire , which means firewall can examine , traffic shape and block traffic.
4. Traffic inspection and control

**Configuring the Virtual wire interface**
1. in two steps  ---Create virtul wire object -- Network -> Virtual wire -> assign the 2 interfaces here . 

2. configure the virtual wire interfaces ----Network -> interface -> interface type "set as Virtual wire" for 2 interfaces in & out 

Virtual wire subinterface

Read and process traffic based on vlan tags , IP classifiers or both , IP classifiers can be specific address, range of address or subnet addresses

**Layer 2 Interface**

1. providing switching between 2 or more interface thorugh a VLAN Object.
2. Configure the firewall to perform App-d, content-id,user-id,ssl decryption and QOS in layer 2 deployment
3. Traffic inspection and control
4. used when no routing is needed.

**Configuring the Virtual wire interface**
1. in two steps  ---Create virtul wire object -- Network -> Virtual wire -> assign the 2 interfaces here . 

2. configure the layer 2 interface that the vlan object connects  ----Network -> interface -> interface type "set as Virtual wire" for 2 interfaces in & out

Layer 2 subinterface

- Assign sib interface to zones 
- useful configuraions for multi-tenant networks
- vlan traffic isolated by sub interfaces * Need route between VLAN's  * security policy blocks interzone traffic by default .

**Layer3 interface**
1. deployment enable route traffic between multiple layer 3 interfaces
2. can require network reconfiguration in the enterprises
3. Network traffic flow thorugh the firewall between layer 3 interfaces which means firewall can examine , traffic shape and block traffic.
4. routing between 2 layer 3 interfaces requires virtual router
5. Layer 3 interface supports IPv4 , IPv6

**Configuring Layer 3 interface**
- Network -> interface -> interface type "Layer 3" -> virtual router -> security zone-> ipv4 - enable dhcp client
- Interface Management profiles :- Create under Network -> Network profiles -> Interface Management 
- Attach Management profiles in the advanced in Network interface

Layer 3 subinterface

Traffic in each VLAN is isolated
- Need a virtual router to connect VLAN's
- security zone blocks interzone traffic by default.
- configure appropriate security policy rules to allow traffic to flow between different security zones.

**Virtual Routers**
- Firewall uses virtual router to obtain routes to other subnets
- Manually define one or more static routes 
- configure a virtual router to participate in one or more dynamic routing protocols
supported dynamic protocols :- BGPv4, OSPFv2,OSPFv3,RIPv2.
- supports multicast routing protocols  - PIM-SM , PIM-SSM

**Configuring Virtual Router**
- Network -> Virtual router -> Attach the layer 3 interfaces and the static route to configure default route .

**Configure VLAN interface**
- Network -> Interface -> VLAN ->  Add -> id -> VLAN object -> VR -> security Zone - IPv4 -(static IP) -> Management Profile if required .

**Loopback Interface**
- Logical Interface with an IP address
- Behaves like a host interface
- Used to provide access to firewall services using https to global protect,Ipsec VPN tunnel, gateway services

Configure loopback interface
- Network -> Interface -> loopback ->  Add -> id -> VR -> security Zone - IPv4 -(static IP) -> Management Profile if required .

**Policy based forwarding**
- Path monitoring feature to verify connectivity to an expternal IP addresses
- Enables firewall to direct traffic through an alternate egress interface
- Firewall uses ICMP to verify the specific IP addresses is reachable
- consider enabling User-id now for more granular control

Configuration :- Policies -> Policy based forwarding -> Add ->
General -> Name -> 
source tab -> ( zone -> any -> any) ->
destinaion (any-> application -> source -> any ) 
forwarding tab:- action -> forward -> egress interface -> next hop -> ip address - Monitor -> profile -> Monitor ip address asme as next hop address

# site2site VPN #
1. Why VPN connectivity is required ?.
Virtual Private Network, allows you to create a secure connection to another network over the Internet

**VPN is PAN**
- Three Basic requirments for configuring VPN in PAN
1. Interface
2. IP addresses 
3. PSK for the IKE gateway

- system that receives traffic for destined for remote private network looks up the next hop in routing table
- routing table points to a logical tuneel interface in case of remote netwwork
- tunnel initerface is not physical which has all required information for the creation of ipsec tunnel
- after traffic is sent to the tunnel VPN is created and traffic is forwarded through it
- Internet key exchange version 1 & 2 are supported .

**What are the 2 phases of IKE in PAN ?** 
1. IKE Phase 1
- identifies the end point of the VPN
- uses peer id to identify the devices
- three settings ( aggressive, main, auto, )
- five pieces of information passed during this phase ( Authentication, diffie-hellman key exchange ,symmetric key algorithm , hashing algorithm , lifetime ) 
2. IKE phase 2
- each side of the tunnel has pxoxy id to identify traffic
- netwroks are identified by proxy-id
- five pieces of information passed during this phase ( Ipsec type/mode, diffie-hellman key exchange ,symmetric key algorithm , hashing algorithm , lifetime )

**Route based site to  site VPN in PAN **
- Each tunnel is bound to an interface
- Each tunnel interface can have a max of 10 IPsec tunnels
- tunnel interface appears to the system as normal and exisitng routing infra structure can be applied. 

**Configuring site to site tunnels**
1. Phase 1 - Object IKE gateway 
- Object IKE cryptographic profiles - Network -> networkprofiles -> IPgateway -> General (ikever1,)
- Object - IPsec cryptographic profiles
2. Phase 2 - Object IPsec cryptographic 













  












