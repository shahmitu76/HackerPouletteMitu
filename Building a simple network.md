# Building a simple network

- Repository: `building_a_simple_network`
- Type of Challenge: `Consolidation`
- Duration: `1 day`
- Deadline: `dd/mm/yy H:i AM/PM`
- Team challenge : `solo`

## The Mission

[Hackers Poulette](http://estelle.daubry.be/projet/becode/formulaire/index.php#home) a young startup that has just been created calls on your services to begin the installation of their infrastructure.

For now, there are only three people working on it, and all on the same set. But Robert Tappan Morris, the CEO, insists on the fact that Hackers Poulette is bound to grow, that the ambitions of the startup are great. So he wants an installation that is easily scalable. All three hosts must also be connected to the internet.

**Addressing table :**

| Devices    | LAN  | IP           | Mask          |
| ---------- | ---- | ------------ | ------------- |
| PC-Robert  | Eth  | 192.168.1.10 | 255.255.255.0 |
| PC-Camille | Eth  | 192.168.1.11 | 255.255.255.0 |
| PC-Renaud  | Eth  | 192.168.1.12 | 255.255.255.0 |

**Resource Requirements :**

- 1 switch (Cisco 2960 with Cisco IOS version 15.0(2) image lanbasek9 or similar)
- 3 PCs (Windows 10)
- Three Ethernet cables
- 1 router
- 1 DNS server

## Mission objectives

You will create a simple network with three hosts and a switch. You will apply IP addressing to the computers to allow communication between these three devices. Use the ping utility to verify connectivity between the hosts.

**Goals :**

- to be able to create a simple network
- to be able to configure interconnectivity between hosts
- to be able to connect hosts to the internet



## Creating Simple Network

To create a simple network, it is as simple as it sounds. I have linked all 3 PCs with a switch. That's it!

### Selecting PC's and a Switch

Select 3 PCs in cisco packet tracer. You can find it in End devices menu. 

Change the name of each PC by clicking on the text box under the respective PC.

Now we have 3PCs named: PC-Robert,PC-Camille,PC-Renaud

Select a Switch now by going in Network devices-Switch and select the first one. I have selected 2960 switch.

### Linking PC's with 2960 switch

To link PC's with switch, we need  a cable. We always use straight -through cable to link 2 different devices. 

Choose first straight through cable( under lightning bolt section) and click on 1st PC-Robert. You will find many different interfaces. Select fastEthernet0. Once selected, take the other point of cable towards switch and click on the switch. Select first fast Ethernet port available. This way you have linked PC- Robert to the switch through fast ethernet connection. Do the same for other 2 PCs.



## Configuring interconnectivity between hosts

### Configuring PCs' IP addresses

Now we have to give IP addresses to our 3 PCs. Please note that Switch is a layer 2 device , so it doesn't have IP address.

Click on PC-Robert. Go to config - interface - fastEthernet0-IP Configuration. Go to static( as we are assigning addresses manually). In IPV4 address section write the IP address of PC-Robert which is 192.168.1.10 and its subnet mask 255.255.255.0

Repeat the same for other 2 PCs assigning the correct IP address( mentioned in the start) and subnet mask remains same 255.255.255.0

Once you have configured all your PCs' IP addresses, you will find that the interfaces will go green(sometimes it might take some time. Don't panick!). Now, if you try to ping from PC to another, it will work.

This way we have created a simple LAN network.

## Connecting hosts to the Internet

### Adding additional devices

1. In order to connect our system to internet, we need 2 more devices: a router and a DNS server. Router will route the data to and from the outer world and DNS server is the device which is                                 responsible for changing website names into IP addresses. So, let's connect these two devices.

2.  Select Router from Network devices. I have selected first one ISR 4341 router . Please note that your router must have gigabyte connection to connect to outer world. To select a server, go in end devices and select Server-PT and change its name to DNS server.

3. Connect one end of the router to the switch by selecting gigabit ethernet connection of the router and connecting to fast ethernet port of switch. Other end of the router with gigabit ethernet port will be linked to fast ethernet port of DNS server.

   ### Configuring new devices

   1. Now it's turn to configure our router and DNS server.

   2. To configure router, we must understand first that there are 2 networks in our whole system which are linked by the router. 

   3. First Network: 192.168.1.0 in which all three PCs , switch lie. Represented by red rectangle in the file

   4. Second Network: 192.168.2.0 in which DNS server is there.Represented by green area in the file.

      #### Configuring DNS server

      1. Since DNS server is on second network: 192.168.2.0, let's give it an IP address: 192.168.2.1. So, click on server and go to Config-Interface-fastethernet-static-ipv4: write IP address 102.168.2.1  and subnet mask 255.255.255.0 of the server. Always make sure that port status is on and now interface will be up and running.
      2. Next, go to services and choose DNS service. Here we have to add all the names and IP addresses of PCs which can send request or receive one from DNS server and also the DNS server itself. Here we will be adding the record of www.cisco.com in the DNS server itself.
      3. Name-PC0, Record type-A, IP Address: 192.168.1.10
      4. Name-PC1, Record type-A, IP Address: 192.168.1.11
      5. Name-PC2, Record type-A, IP Address: 192.168.1.12 
      6. Name- www.cisco.com, Record type-A, IP Address: 192.168.2.1
      7. Now we have to link all PCs to this DNS server too. Therefore, click on PC0 and go to Config-                                      global settings-Gateway/DNS ipv4-static, write IP address of DNS server-192.168.2.1 in the DNS column. We have left default gateway, we will fill it later. Repeat it for other 2 PCS.
      8. At this point we have told all PCs that DNS server exists and viceversa, But still they cannot communicate as they are on different networks. So, we have to configure the interfaces of router to make them talk.

#### Configuring Router

1. Configuring interface g/0/0/0: click on router and go to interface-g/0/0-IP configuration. Fill the IP address of the router: 192.168.1.254( it's a good idea to use the last address for the router) and subnet mask 255.255.255.0

2. Configuring interface g/0/0/1: click on router and go to interface-g/0/0/1-IP configuration. Fill the IP address of the router: 192.168.2.254 and subnet mask 255.255.255.0.

3. Now go to Routing-Static routes. Here we have to tell the routes from which data might come and go

4. First route: network: 192.168.1.0, subnet mask: 255.255.255.0 and next device(hop) will be PC0-192.168.1.10

5. Second route: network: 192.168.1.0 ,subnet mask: 255.255.255.0 and next device(hop) will be PC1-192.168.1.11

6. Third route: network: 192.168.1.0, subnet mask: 255.255.255.0 and next device(hop) will be PC2-192.168.1.12

7. Fourth route: network: 192.168.2.0, subnet mask: 255.255.255.0 and next device(hop) will be DNS server-192.168.2.1

8. Now, we have to also set the default gateway of our DNS server which will be 192.168.2.254. and all the PC's which will be 192.168.1.254

   

   ## Final Conclusion

   Now , our whole network system is working. We can test this by clicking on any PC and go to command terminal. Give the command ping www.cisco.com. We will get reply.



 ["C:\cisco packet tracer\hackers poulettefinal -mitu.pkt"]()





### 