# Cloud (Azure) Administration - based on the AZ-104

Based on the Azure AZ-104 administrator exam.

All headings/subtopics are taken from the Microsoft Learn path for the AZ-104.

 General advice: delete the resource group the lab you create is in when you're done with labbing, so you don't accrue any unnecessary costs.

## Prerequisites for Azure administrators 

### Cloud Governance - building a comprehensive Cloud governance strategy, through things such as access policies, resource locks, tags, through services such as Azure Policy and Azure Blueprints.

- Governance - establishing rules, policies, ensuring they're followed. According to what standard? Could be industry standards, or defined by the organisation.

	- Cloud Adoption Framework

		- Guidance to help you with your Cloud journey. A comprehensive set of guidelines. 

Wrote this before in the foundations mind-map but this is a whole job in and of itself. From start to finish.

			- 1. Define strategy

				- Motivations. Why are we moving to the Cloud? Meet with important people/stakeholders to find out why. What's the tradeoffs?
				- Business outcomes - meeting with different departments
				- Business justification - what's the ROI on moving to the Cloud
				- Prioritise a certain project - an achievable project. Start off small. Same principles with Automation.

			- 2. Make a plan

				- Digital Estate - current inventory of what workloads you're running that can be migrated to the Cloud e.g. VMs
				- Organisation alignment - ensure the right people are involved, technical and non-technical.
				- Skills readiness - ensure the skills are there
				- Cloud adoption plan - comprehensive plan bringing it all together

			- 3. Ready your organisation

				- Azure setup (or any other platform for that matter) - read the Azure setup guide 
				- First landing zone - building out subscriptions for each area of business. Includes Cloud infrastructure, governance, accounting, and security.
				- Expanding the landing zone - ensure it meets operations, governance, and security needs
				- Best practices - recommended and proven practices to ensure scalability and maintainability 

			- 4. Adopt the Cloud

				- Migrate

					- Azure migration guide - to deploy first project 
					- Migration scenarios - in-depth guides for migration 
					- Best practices - Azure cloud migration best practices checklist 
					- Process improvements - ways to make the whole thing more efficient 

				- Innovate

					- Azure innovation guide - accelerate development and build a MVP for your idea
					- Innovation scenarios - ensure investments add value and meet customer needs.
					- Best practices - verify progress maps to recommended practices
					- Process improvements - check in with customers to verify it's what they need

			- 5. Govern and manage your environments

				- Govern

					- Methodology - consider your end state solution. Steps from start to full Cloud governance 
					- Benchmark - governance benchmark tool, assess current and future state.
					- Initial best practice - create an MVP that captures the first steps of the governance plan.
					- Governance maturity - iteratively adding governance controls on to reach the end state solution. 

				- Manage

					- Management baseline - minimum commitment to operations management 
					- Business commitments - documenting supported workloads
					- Operations baseline - applying best practices on initial management baseline 
					- Operations maturity - advanced operations and design principles

	- Subscription governance strategy

(Part of the higher organisational structure in Azure, with there being management groups, subscriptions, resource groups, and resources)

		- Billing

			- One billing report created per subscription. Can organise departments per subscription, and then do billing reports based on the expenditure of each department.

		- Access Control

			- Remember that subscriptions are your deployment boundary. Each subscription associated with one AD tenant. Set granular access through RBAC.

				- Use cases: Multiple teams, controlling what resources they have access to.

Allowing one user to manage VMs, another to manage VNETs

Allowing DB administrator to manage SQL databases in a subscription

Allow user to manage all resources in a resource group

Allow an application to access all resources in a resource group
				- Operations: applied to a scope (resource, resource group, subscription, management group)

Permissions are inherited by child scopes.

Owner role to a management group > can manage all subscriptions within the management group

Reader role to a subscription> view every resource group and resource within a subscription

Contributor role to resource group > manage resources within that resource group.

Enforced on any resource passing through the Azure Resource Manager. 

Resource Manager - uses an allow model. Allows you to perform actions (read/write/delete). Combines permissions from different role assignments.
				- Configuration: Access Control (IAM) pane in the Azure Portal.

			- Resource locks - prevent resources from being deleted or changed.

				- Use case: preventing an IT admin from deleting critical resources.

What if someone deletes the lock? Can use Azure Blueprints to define resources that org requires, and replace it if it is removed.
				- Operations: locks applied to subscriptions, resource group, or resources.

Lock levels: CanNotDelete (read/modify only), or ReadOnly (read only)

Applies regardless of RBAC permissions.

Resources in a group, that have a lock applied to the resource group, are also applied to the resources themselves.
				- Configuration: done from the Azure Portal (under locks), PowerShell, CLI, ARM templates. 

View/add/delete locks in Locks, under Settings.

To modify a locked resource, first remove the lock, then modify the resource.

		- Subscription limits

			- Resource limitations. Might need to add more subscriptions if there's resource limits.

		- Management groups

			- Managing multiple subscriptions. Manages access, policies, compliance, across multiple subscriptions.

		- Resource Tags

			- Another way of organising resources - others being subscriptions, resource groups. Provides extra metadata about resources.

				- Use case: resource management, cost management and optimisation, operations management, security, governance and regulatory compliance, workload optimisation and automation

Azure Policy to ensure tagging rules and conventions 
				- Operations: consists of a name and a value. E.g. Impact:"Mission-Critical."
				- Configuration: done through PowerShell/CLI/ARM templates/REST API/Portal, Azure Policy, 

		- Azure Policy

			- Ensuring resources stay compliant. Create/assign/manage policies to audit resources

				- Use case: enforcing different rules and effects over resource configurations, to stay compliant with standards. 

E.g. restricting deployment of a resource to a specific location.

Good idea to: use policy initiatives, and then modify the policy initiative, removing the need to change policy assignment for resources.
				- Operations: defining policies, and groups of policies (initiatives), built in policies.

Enabled policies evaluate current resources, and are applied when new resources are created or changed.

Can automatically remediate non-compliant resources.

Integrating with DevOps by applying CI/CD policies, during pre and post deployment phases of applications.

Policy initiatives - grouping many policy definitions into one set. Can also include compliance standards e.g. ISO 27001.
				- Configuration: 

1. Create the policy - what to evaluate, and what action to take.

2. Assign the definition - assigned to resources. Policy assignment/initiative assignment is within a specific scope (management group/subscription/resource group)

Child resources inherit from parent resource group policy assignments.

3. Review the results 

Existing resources marked as either compliant or non-compliant. Happens once per hour.


Defining policy definitions and initiative definitions in Azure Policy. 

		- Azure Blueprints

			- Scaling governance decisions across multiple subscriptions - compliance, access, protecting critical resources.

				- Use case: defining a set of repeatable governance tools and resources your organisation requires

E.g. creating a blueprint definiton based on ISO 27001, assigning it to a management group.

Orchestrates deployment of artifacts - role assignments, policy assignments, ARM templates, resource groups
				- Operations: each component within the blueprint definition is called an artifact. 

Artifacts may or may not have parameters that can be configured, either when creating the blueprint definition or assigning it to a scope.
				- Configuration:

1. Create an Azure Blueprint
2. Assign the blueprint
3. Track the assignments

### Intro to Azure VMs

- On-demand, scalable, computing resources. Install any OS.

	- Considerations

		- Network - VNETs are what provide connectivity between VMs and other services. VMs in one virtual network can access each other.

Considering the topology before putting anything in place - network addresses and subnets.

Ensure you don't configure VNETs that have overlapping addressing, when connecting different VNETs together.

Segregating the network with subnets 
e.g. 10.1.0.0 for VMs, 10.2.0.0 back-end services, 10.3.0.0 SQL VMs

First four addresses and last address is reserved in Azure subnets

Secure the network - NSGs control traffic to and from subnets, and to and from VMs. Rules to each inbound/outbound request at the network interface or subnet level.

Planning deployment - 
What does the server communicate with?
What ports are open?
Which OS?
How much disk space does it use?
What kind of data does this use? Any compliance restrictions?
What's the CPU/Memory/Disk I/O load? Is there burst traffic?
		- Name - choosing a name that's meaningful and consistent 

VMs need the VM itself, storage account, virtual network, network interface, NSGs, and possibly a public IP. Good idea to be consistent with VM names since the resources generate their names from the VM name itself.

E.g. including the environment, location, instance, product/service, role. devusc-webvm01
		- Location - different locations have different hardware offerings. Prices differences at different locations. 

Can place VMs at a certain location for legal/compliance/tax requirements.

Also placing VMs in a certain location close to the end user to improve performance.
		- Size - different VM sizes, with a mix of compute, memory, and storage offerings.

Determine VM size based on the workload you're running.

General purpose - balanced, testing, development, small/med databases, low/med traffic web servers

Compute optimised - high CPU-memory ratio, med traffic web servers/network appliance/batch processes/application servers

Memory optimised - high memory-CPU ratio, relational databases, med/large caches, in-memory analytics

Storage optimised - high disk throughput and IO, ideal for VMs with databases

GPU - GPU VMs for heavy graphics rendering, video editing, model training, deep learning

High performance computes - fastest and most powerful CPU VMs, optional high throughput network interfaces

More the system resources > greater the cost 

Size can be changed whenever. Automatically reboots VM when doing it
		- Pricing model - compute costs and storage costs. 

Compute costs - priced on per-hour, billed on per-minute. Includes charge for Windows based OS. (Save money with Azure Hybrid Benefit)

Storage costs - the storage the VM uses. Even if VMs stopped/deallocated, you're still charged for the storage.

Paying for compute costs either pay-as-you-go (no commitments or upfront payments, on demand) or through reserved virtual machine instances (advance purchase of 1-3 years, commitment made up front, price savings, used when you need a VM for a long time))
		- Storage - all VMs have two VHDs - one for OS, one for temporary storage. Data for each VHD held in Azure Storage as page blobs.

Azure Storage - holds your VHDs. VHDs are in a storage account, for a specific subscription. 

Standard or premium accounts. 

Standard for development/testing.

Premium uses SSDs, high performance, low latency, I/O intensive, production workloads.

Relationship between the storage account and VHD - unmanaged or managed disks.

Unmanaged - responsible for storage accounts. Fixed rate-limit of 20,000 I/O operations per second. If you want to scale out with more disks, then need more storage accounts.

Managed - newer/recommended, specify size of disk, and Azure manages the disk and the storage. No need to worry about limits.
		- OS - OS licensing costs included. 

Search Azure Marketplace for different images. E.g. LAMP image.

Can just create disk images yourself, upload to Azure Storage, and use it to create the VM. 

Azure only supports 64-bit OS (bugger if you're running legacy systems)

	- Configuration

		- Creating a VM, can be done using the portal. (Also PowerShell, CLI, REST API, Client SDK, Blueprints, VM Extensions, Automation Services, ARM templates, Bicep code... etc.)

			- Azure Resource Manager 

				- Use case: Can create ARM templates, to create and deploy a specific configuration.

Creating a VM template for a test environment.
				- Operations: through JSON files, that define the resources you need to deploy the solution.
				- Configuration: under Automation, Export Template.

Using the CLI, PowerShell, or REST APIs (in Python, or whatever language) to process ARM templates.


			- Azure PowerShell

				- Use case: automating repetitive tasks, running a script constantly.

One-off tasks and repeated tasks.
				- Operations: typing in a cmdlet, and providing parameters to it.
				- Configuration: New-AzVM cmdlet. Parameters supplied to it.

			- Azure CLI

				- Use case: another option for scripting and command-line Azure interaction. Streamline administrative workflow 

Doesn't need PowerShell to function. 

Used with other languages e.g. Ruby and Python
				- Operations: typing in commands into the CLI. Accessed through the browser with Cloud Shell too.
				- Configuration: creating a VM with az vm create 

			- REST API

				- Use case: interacting with resources programmatically. 
				- Operations: creating/managing VMs through URIs with HTTP methods GET/PUT/POST/DELETE/PATCH, and a corresponding response.

Azure Compute APIs allow:
Create/manage availability sets
Add/manage VM extensions
Create/manage managed disks/snapshots/images
Access platform images
Retrieve useful information
Create/manage VMs and VM scale sets
				- Configuration: whatever program that you'll use to create the REST APIs. Such as Postman or Curl. See the API documentation too.

			- Client SDK

				- Use case: higher level of abstraction, encapsulating the REST API. 
				- Operations: available in a variety of languages, Python/Go/Ruby/C#/Java/Node.js/PHP
				- Configuration: code in the respective language that does the required function

			- Azure VM Extensions

				- Use case: configuring additional software on VM after deployment 
				- Operations: small applications that configure and automate tasks on VMs after deployment
				- Configuration: run with Azure CLI, PowerShell, ARM templates, portal

			- Azure Automation Services

				- Use case: operating from (yet another - I wonder what's next) another high level. Automating management tasks with ease.
				- Operations: process automation, configuration management, and update management.

Process automation: setting up watcher tasks, responding to events. Taking action and fixing the problem as soon as it's reported.

Configuration management: tracking software updates e.g. Microsoft Endpoint Configuration Manager

Update Management: manage updates and patches for your VMs
				- Configuration: through Azure Automation.

		- Managing availability

			- Availability - percentage of time a service is up for use.

Why? Part of your business continuity and disaster recovery strategy (BCDR).

				- Availability set: grouping VMs so that they aren't all a single point of failure. Same VMs, same functionality, same software.

99.95% SLA for multiple VMs in an availability set.

					- Use case: to keep your services up for as long as possible - part of the BCDR plan.

What if data/software on VM itself is affected? Have disaster recovery and backup techniques.
					- Operations: VMs aren't a single point of failure, and they aren't upgraded at the same time. 

VMs in an availability set are: in different fault domains and update domains.

Fault domain - group that shares common hardware, and therefore a single point of failure. E.g. a rack within a DC. Availability sets put VMs in different racks.

Update domain - logical group of hardware that can undergo maintenance or be rebooted at the same time. 
					- Configuration: Azure Portal, disaster recovery section. 

				- Failover across locations: replicate infrastructure across sites to handle regional failover.

					- Azure Site Recovery

						- Use case: replicates workloads from primary site to secondary location

Don't need to maintain a second DC
Easy testing of failovers 
						- Operations: Outage at primary site fails over to secondary location - seamless usage of applications for the end user 
						- Configuration: Azure Site Recovery.

		- Data backup and recovery

			- Allowing for restoration of data when things go wrong.

				- Azure Backup: backup as a service.

					- Use case: protecting your VMs, wherever they are. On-premises or in the Cloud.

Automatic storage management
Unlimited scaling
Multiple storage options (locally redundant or geo-redundant storage)
Unlimited data transfer
Data encryption
Application-consistent backup (recovery point has all the data you need)
Long-term retention

					- Operations: wide range of backup scenarios, such as files/folders on Windows OS (virtual/physical), snapshots, server workloads, VMs, client machines.

Different components:
Azure Backup Agent
System Center Data Protection Manager
Azure Backup Server
Azure Backup VM Extension

Recovery Services vault: storing backup data. Backed by Azure Storage blobs. Select VMs to backup and backup policy (how often snapshots are taken, and how long they're stored)
					- Configuration: Azure Backup

### Computer Networking Fundamentals (Fundamentals are always important...)

- Networks built on the same principles, that are applied to build a LAN or a Cloud based network (can agree with this one...)

	- Network

		- Collection of network-enabled devices for communication.
		- Of different types:

			- PANs

				- Networking around an individual i.e. bluetooth

			- LANs

				- Networking around a location e.g. a hospital

			- MANs

				- Networking between locations in a city

			- WANs

				- Networking between different geographical locations. E.g. connecting different head offices.

			- Difference between LANs and WANs:

LANs privately operated in a building (usually), WANs geographically separate offices.

LANs usually operate at 10Gbps, WANs lesser (depends also)

LANs less congested, WANs more congested

LANs administered in-house, WANs require 3rd party to set up.

		- Of different topologies:

			- Bus

				- Every device is on the same cable (obsolete, archaic)

			- Ring

				- Every device is connected to form a ring (also obselete)

			- Mesh

				- Every device connected to each other. Physical overhead of connecting all the devices. Partial mesh may be more optimal.

			- Star

				- Most common - devices connect to a centralised switch 

		- Ethernet (802.3)

			- Industry standard - for transmitting in wire-based LAN networks. Also fiber. Standard includes data transmission, error handling, performance thresholds.

				- Fast Ethernet (802.3u) - 100Mbps
				- Gigabit Ethernet (802.3ab) - 1Gbps
				- 10 Gigabit Ethernet (802.3ae) - 10 Gbps
				- Terabit Ethernet - 200-400 Gbps

		- Azure Virtual Network

			- Emulating the structure of on-premises networks, holds your resources, may have a public IP, NSGs, different subnets inside the virtual network.

Extending your network into the Cloud.

Assumed to be private networks. Control the addressing that's used. Subnetting for segmentation with IP addresses assigned to segments.

		- Connectivity between on-premises and Azure through:

			- VPN connection through VPN gateway, ExpressRoute.

	- Network Standards

		- Governing the hardware and software, allowing interoperability between network-enabled devices. ITU, ANSI, IEEE.

			- 802 family of standards - physical networking for both wired and wireless.

	- Network infrastructure

		- Devices - in general, they each have a MAC and IP

MAC - unique identifier for a device.

			- Repeaters (archaic)

				- Repeats network signals

			- Hubs (archaic)

				- multiport repeater, sends out all ports. Fast Ethernet and dual speed. (I don't know why Microsoft included this on the Learning page, but alright.)

			- Bridges (archaic)

				- filter/forward packets between segments, based on MAC

			- Switches

				- Matching speed, PoE, port mirroring, packet sniffers, IDS.

					- Unmanaged - no config, SOHO, automatic packet switching
					- Managed - adjust config of the switch, CLI that is accessed through telnet/ssh/remote console/web interface.

Either smart (half-way between unmanaged and managed, only a few features)

Or Enterprise - fully managed switch 

						- QoS - critical systems given higher priority
						- VLANs - L2 logical groups of devices, security.
						- STP - preventing loops, resiliency if a link fails
						- Port mirroring - network analyzer to diagnose issues and problems
						- Bandwidth rate limit - fine control of bandwidth for ports, depending on applications 
						- MAC address filtering - control what devices can be used/have access through switch
						- SNMP client - network monitoring

			- Routers/Gateway

				- Linking your networks together. Forwarding packets to the correct network, using the IP address.

					- Has a routing table containing preferred route for each of the networks.

Routes shared between routers using BGP (from exterior gateway to exterior gateway, that is)

						- Access router: home routers, simple routing need.
						- Distribution routers: compiling routing data from multiple routers. Designed to hold much more routing information, and is used on a WAN for QoS.
						- Edge routers: boundary between your network and other networks. Gateways, to filter traffic. May come with access controls/firewalls, and run services e.g. DNS or DHCP.
						- Core routers: enterprise routers, higher bandwidths, connecting different geographic locations, packet forwarding to edge routers, minimising packet loss and congestion.
						- Wireless router: routing capabilities of regular router, but offers wireless access to wireless devices.

		- Azure topologies

			- Hub-spoke

				- Reference architecture, hub acts as central point between Cloud and on-premises, spoke is the VNET, peered to the hub. Connections from on-premises to Cloud made using VPN gateway or ExpressRoute.

			- ExpressRoute

				- dedicated circuit, higher bandwidth, hosted by a connectivity partner, resilient conneciton.

	- Network Protocols

		- How do network devices communicate with one another? 

Protocol: set of conditions and rules that governs how network devices communicate on a given network.

Network address: unique identifier for a network-enabled device. E.g. MAC or IP addresses.

Data packet: the message two devices use when they send a message to each other. Header contains certain information.

Datagram: packet from an unreliable source.

Routing: making sure packets follow the correct delivery path from sender to receiver.

			- Network Communications 

				- Establishing and maintaining a connection between devices

					- TCP - dividing data into segments. Stable, reliable, connection-orientated, has overhead.
					- IP - addressing, header has addressing information (among other things)
					- UDP - connectionless, low-latency, loss-tolerant, implementation. No reliability
					- HTTP - delivering web page content, downloading/uploading files
					- FTP - transfer files between computers
					- POP3 - email protocol, receiving.
					- SMTP - email protocol, sending.
					- IMAP - email protocol, manage mailbox.

			- Network Security

				- Maintain the security and transfer of data across the network

					- SSL - standard encryption and security, between computer and device over the internet
					- TLS - successor to SSL, security enhancements, protecting communications
					- HTTPS - secure version of HTTP
					- SSH - secure data connection, CLI execution, remote authentication.
					- Kerberos - authentication for client-server-based applications through secret-key cryptography. Assumes endpoints are insecure.

			- Network Management

				- administration and monitoring of networks

					- SNMP - collection of data from devices
					- ICMP - allows network-enabled devices to send warning and error messages, along with other information about requests, or service unavailability.

			- Ports

				- Routing incoming messages to a specific process. Server can receive multiple requests on each port and service each of them without conflict. TCP or UDP.

			- IP suite

				- collection of communications protocols, protocol stack. Known as TCP/IP. Layered model.

					- Application

						- Application communications e.g. telnet/ftp/smtp/dns/snmp/ssh

					- Transport

						- Host-to-host communication, flow control, reliability (or none), e.g. TCP or UDP

					- Internet 

						- Exchanging datagrams, addressing, routing, IPSec, IPv4, IPv6, ICMP, IP

					- Network Access

						- Defining how data's sent across the network i.e. Ethernet/PPP/ATM/ARP/DSL/ISDN

			- Monitoring networks in Azure 

				- Maintaining and monitoring the health of the network 

					- Azure Network Watcher - capture packet data, look at flow of data, troubleshoot network-related problems
					- Network Performance Monitor - monitor and report on health of network, insights, connectivity, for both on-premises and in the Cloud (Note: Connection Monitor is the new thing nowadays)
					- Performance Monitor - capability within Network Performance Monitor, across on-premises and the Cloud, report networking issues and monitor routes, particular network segments. Doesn't need SNMP. (Note: Connection Monitor is the new thing nowadays)

	- IP address standards and services

		- ARP - mapping L2 to L3 address. When building a packet, you have the source and destination IPs. But what about the destination MAC? ARP is the answer. RARP - reverse version of ARP.

IPv6 uses NDP.
		- TCP/IP - what the Internet works on. Defines how data is shared through the end-to-end communication process. 

Datagrams. How the packet is addressed/transmitted/routed/received. Determining the most efficent route.

Stateless - each request is separate from one another.

			- Application: what the applications use to transmit data for communication e.g. HTTP/DNS/FTP/IMAP/LDAP/POP/SMTP/SNMP/SSH/TELNET/TLS/SSL

				- Azure DNS

					- Hosting registered domain names by using the Azure infrastructure

Managing DNS records: A/AAAA/CNAME/SOA/NS/MX

Doesn't replace domain registrars (where you register/purchase domain names)

Alias record - route traffic to an Azure resource

			- Transport - splitting data into chunks, determining the right port. TCP or UDP.
			- Internet - ensuring packet gets to the destination. Addressing. IP, IPv4, IPv6, ICMP, IPSec.

				- IPv4

					- 32-bit, has a network part, and a host part. E.g. 192.168.0.1/24, 192.168.0. is your network, and the .1 is the host part.
					- Classes of IPv4 - A/B/C/D/E.
					- Subnets: logical networks within an A/B/C network. CIDR notation. For routing performance. 

						- Special addresses:

							- 10.0.0.0-10.255.255.255
Private network
							- 127.0.0.0-127.255.255.255
Loopback, host
							- 172.16.0.0-172.31.255.255
Private network
							- 192.88.99.0-192.88.99.255
Reserved
							- 192.168.0.0-192.168.255.255
Private network
							- 255.255.255.255
Subnet - limited broadcast

						- Private IP addresses

A solution to running out of public IP addresses - also NAT, classless networks, CIDR.

Communication in a private network to a public network only happens through NAT at a routing gateway.

Connecting two private networks together - would need to use a VPN, which encapsulates each private network packet, and encrypts it (as sending over a public network).

							- Private IPs:

								- 10.0.0.0/8
								- 172.16.0.0/12
								- 192.168.0.0/16

				- IPv6

					- Address problem of IPv6 address exhaustion, 128 bits address space.

						- Simplified network configuration - IPv6 has address autoconfiguration
						- Security - through IPsec
						- New Service Support - eliminates need for NAT, so easier for peer-to-peer networks.
						- Multicast and anycast functionality - multicast for transmitting messages in a one-to-many fashion, anycast for having multiple routing paths to the destination.

			- Network Access - defining how data is sent across the network. ARP, MAC, Ethernet, DSL, ISDN.

### Network Security Fundamentals

- How do we share resources in a secure way, and ensure only authorised users have access to them. What network appliances can we use for security, and how do we monitor network access and traffic usage.

	- Client and server

Client: accesses and interacts with a service available on a server.

Server: software/hardware system that exposes various services and functions to network clients.

		- Client types:

Thick - local storage, local CPU (process and store data locally without using a server)

Hybrid - non-local storage, local CPU
(limited local data processing but no local storage. E.g. device that renders content and stores results on the server.)

Thin - non-local storage, non-local CPU (can't process or store data locally, relies on server, i.e. web applications)
		- Server models:

Request-response - client sends a request to the server, server carries it out, and sends back a response.

Peer-to-peer (P2P) - every network device is both a client and server. Unstructured network, ad hoc usage.

Publish-subscribe - clients subscribe to a service on a server, server sends out a message to every client that's subscribed. E.g. RSS feed.

			- Server types:

				- Application: hosts an application. Any network device with access
				- Computing: high amounts of CPU and memory available to the client. Any networked computer that requires more CPU/Memory to complete an activity is a client.
				- Database: maintains and provide access to a database. Client is one that requires access to structured data.
				- File: makes available shared files and folders. Clients need access to those shared resources.
				- Game: provisions a multiplayer game environment. Clients are your PCs/tablets/smartphones/game consoles
				- Mail: hosts your email, makes it available. Clients are email applications.
				- Media: enables media streaming. Clients are web/mobile applications.
				- Print: share printers over the network. Clients are devices that need to print.
				- Web: hosts webpages on the internet or on the private internet networks. Clients are devices with a browser.

			- Advantages/Disadvantages of client-server networks

Advantages:
Centrally managed, governing access and control to servers and services.
Server architecture can scale if user usage increases
Data stored/accessed centrally
Data more easily safeguarded with centralised storage of data

Disadvantages:
Server fails, users can't access it (due to centralisation)
Client-server architecture requires dedicated hardware and software - expensive
Running/maintaining requires knowledge
Multiple requests may affect operation
			- Peer-to-peer networks: distributed network model.
			- Managing clients and servers in the network:  Azure Resource Manager, Azure Virtual Machines.

	- Authentication and authorisation

		- Only authenticated + authorised users can connect to network resources. Need to make sure that access is conditional.

			- Network Authentication - verify who the users claim to be.

				- Password authentication - users enter in a secret value
				- Two-factor: something they know, have, or are.
				- Token authentication: device purposely built for authentication, that provides a token. Required for successful authentication.
				- Transactional authentication - scrutinise characteristics of the user e.g. the time accessed
				- Computer recognition authentication - looking at the device being used to access the network
				- CAPTCHA - verify if the system requesting access is a human
				- Single sign on - entering in the credentials once - then authenticating across multiple applications and otols.

			- Authentication protocols - shared set of rules on how information exchanged between electronic devices.

				- Kerberos - authentication protocol. 

					- Key Distribution Center (KDC)

						- Authentication server, authenticates, issues tickets to principals (that's the user or service)
						- Database holds information about principals, generates secret keys
						- Another server that generates service tickets based on the initial tickets that the principals present.
						- Principals get tickets that grant them service tickets from the KDC. Using those service tickets to access resources/services/applications.

				- TLS/SSL - encrypting information over the internet. 

					- TLS handshake

						- SYN,
						- ,SYN-ACK
						- ACK
						- ClientHello, 
						- ,ServerHello
						- ClientKeyExchange,
						- Finished(client),
						- ,Finished(server)

			- Authorisation

				- Ensuring authenticated user is accessing the resources they're requesting

E.g. read/write/delete/etc.

Good idea to give users least amount of permissions.

			- Differences between authentication and authorisation

				- Authentication

					- Confirms identity
					- Asks for credentials
					- Before authorisation
					- HR member signs into the app

				- Authorisation

					- Confirms level of access
					- Checks permissions
					- Happens after authorisation
					- HR attempts to delete something, doesn't work, because they don't have the permissions.

	- Firewalls and network security

		- Network security in general is about preventing attacks and weaknesses in the network

			- Strategies to deal with network security

				- Access Control - whether users can access the network or its resources
				- Antimalware tools - monitor and remedy malware
				- Application security - testing applications, that may contain security vulnerabilities
				- Behavioral analytics - identify suspicious changes, if there's a change in the regular behaviour of the network
				- Email security - from suspicious senders and such
				- IDS and IPS - proactive and preventative measures with regards to network traffic. E.g. Azure Network Watcher, open-source IDS.
				- VPN - encrypted networks over the Internet, using TLS or IPSec
				- Web Security - securing the web, e.g. web filters
				- Wireless security - encryption, separate wireless network for guests.

			- Network security zones - has security policies applied to it.

				- Trusted or private zones

					- Contains resources and devices that shouldn't be accessible by people outside the organisation

				- Public zones

					- contains everything outside of the organisation

				- Perimeter network

					- DMZ, zone where resources and services are accessible from outside the organisation.

				- Zone filtering policies

					- Inside-to-outside, inside-to-perimeter

						- filter that scrutinises traffic sourced from the inside and heading to the perimeter network

					- Outside-to-inside

						- filters traffic coming from outside to inside the network

					- Outside-to-perimeter

						- filters traffic from outside to the perimeter network

					- Perimeter-network-to-outside

						- Filters traffic from the perimeter network to outside

			- Network firewalls

				- Security appliance that blocks unauthorised access into the network

Has monitoring, logging, and security policies. 

					- Implementations

						- Hardware firewall

							- Standalone physical device e.g. a router

						- Software firewall

							- installed and configured on a device

					- Types

						- Application-layer firewalls

							- Targeting applications, physical or software based. E.g. HTTP inspection.

						- Packet filtering firewalls

							- Scrutinising each packet

						- Circuit-level firewalls

							- Checks whether TCP or UDP connections are valid before data is exchanged

						- Proxy server firewalls

							- Controls the information that goes in and out of the network. Acts as an intermediary, hiding details about the requesting client.

						- Stateful firewalls

							- Inspects characteristics about connections. Monitors packets over time and maintains a state table.

						- NGFWs

							- Stateful firewalls, but have more features - such as packet filtering, VPN support. Deep packet inspection.

					- Why?

						- Attackers employ malware
						- Theft of sensitive data
						- Ransomware

			- Azure Network Security tools

				- Network Security Groups (NSGs)

					- Protect your virtual networks
					- Filter traffic to/from resources
					- Log NSG flows with Azure Network Watcher

				- Azure Firewall

					- Cloud-based fully managed firewall
					- Preconfigured with high availability

				- Connectivity - VPN gateways

					- Connecting on-premises to Azure through site-to-site VPN
					- Point-to-site VPN for individual users and clients to connect to Azure resources

				- Azure Network Security Appliances

					- Developed by Microsoft partners on Azure Marketplace. Offers a bunch of different features.

				- Azure virtual network service endpoints

					- Only critical Azure services can connect to virtual networks, and not to the public internet. 

Applies to:
Azure SQL Database
Azure Storage
Azure App Service
Azure Key Vault

				- General practices

					- Disable SSH/RDP. Used for managing VMs, however is an inherent risk. Create a point-to-site VPN before enabling SSH/RDP for remote management.
					- Load balancing - improve performance and availability of the network. Distributing network traffic among the devices.

				- Azure DDoS Protection

					- Provides traffic monitoring and mitigation - access to experts/additional features with Standard tier.

	- Network monitoring

		- Monitoring all the components of the network - finding faults and analysing information.

			- Types of monitoring

				- Agent-based 

					- Software running on the device being monitored, sent back to the monitoring solution

				- Agentless

					- Networking solution gathers all the data. Avoids having to configure an agent. Might not be as granular. 

			- Monitoring intervals

				- How frequently you want to poll for information from the devices you're monitoring. Depends on what you're monitoring.

Too much monitoring - may cause overload.

			- Protocols

				- SNMP

					- Majority of network devices, linux servers use SNMP. SNMP agent installed on the device. Communicates with NMS.

				- WMI (Wireless Management Instrumentation)

					- Windows devices use WMI. Windows Management Infrastructure is the newer version, coming with upgraded capabilities (i.e. better integration with PowerShell to run scripts)

				- Syslog

					- Event logging.

			- Best practices - tasks/functions to manage a network properly.

				- FCAPS

					- Fault Management

						- processes and tasks relating to solving problems

					- Configuration management

						- collecting information based on changes to devices/hardware/network/software

					- Accounting/administration

						- usage monitored, for tracking utilisation and billing for users, also administration - managing permissions, passwords.

					- Performance management

						- managing the performance of the network, e.g. throughput, usage, response times

					- Security

						- all the tasks to secure the network

				- Alerting and reporting

					- Using alerts to capture information about issues

				- Azure Network Monitoring solutions

					- Azure Monitor - unified platform, collecting data, log analysis, on-premises and in the Cloud. Varies in scope of abilities.
					- Log Analytics - query and aggregate large amounts of log data for analysis

### Azure CLI

- Getting stuff done more quickly by using the CLI. A command-line program that allows you to connect to Azure and execute commands.

Installed on: Linux/Mac/Windows

	- Accessed through:

		- Locally, through the command-line
		- Cloud Shell in the web browser

	- Scripts in the CLI

		- Syntax looks like this for variables in Bash:

variable="value"
variable=integer

PowerShell:
$variable="value"
$variable=integer

	- Common commands

		- az login
		- az --version
		- az find 
		- az group create --name <name> --location <location>
		- az group list 

			- az group list --output table

		- az group --delete --resource-group <name>
		- export variable_name=value

### Azure Automation: PowerShell

- Another tool to choose from, the others being the portal or the CLI. For automation. Can be suited for those who have experience with administering Windows based operating systems

	- Composed of

		- Base PowerShell product (PowerShell Core on macOS and Linux)
		- Azure Powershell module

	- Common commands

		- Get-AzResourceGroup | Format-Table
		- New-AzResourceGroup
		- Import-Module Az
		- Connect-AzAccount
		- New-AzVm

	- Creating scripts in PowerShell

### Identity: Azure Active Directory

- Cloud-based identity and access management solution. Internal, external, customer identities.

Separate from Active Directory, that's used on-premises.

	- Use case: securing applications on the Internet
	- Authentication

		- SAML
		- OAuth
		- WS-Federation

	- Hierarchy

		- Tenants (organisation)

			- Groups

				- Users

	- Secure Score
	- Hybrid identity

		- Linking on-prem AD to Azure AD. Single identity to authenticate with

			- Authentication methods

				- Azure AD password hash synchronisation
				- Azure AD pass-through authentication
				- Federated authentication - smart cards, on-prem MFA

	- Data storage

		- B2B

			- Stored in US DCs

		- B2C

			- No personal data outside of Europe, policy config data in US DCs

		- MFA

			- Phones/texts from US DCs
			- Global providers routing
			- OAuth US
			- Push notifications for Microsoft Authenticator app, US DCs

	- Licensing 

		- Free
		- Pay-as-you-go for specific features
		- Office 365 Apps
		- P1
		- P2

	- Terminology

		- Identity

			- Something that's identified and authenticated

		- Account

			- Identity and data

		- Azure AD account

			- Identity created in Azure AD/Microsoft 365

		- Azure Subscription

			- Level of access to Azure

		- Azure AD tenant

			- Instance of Azure AD, organisation

		- Multi-tenant

			- Multiple organisations access same applications and services

		- Azure AD directory

			- Created when you subscribe to Azure - each representing a tenant

		- Custom domain

			- Domain customised for Azure AD Directory

		- Owner role

			- Manage all resources in Azure

		- Global admin

			- Administrative capabilities in Azure AD

	- Comparison of member and guest default permissions (Member-Guest)

		- Users and contacts

			- Member: Can view and change own details.
			- Guest: Can view mostly, change own password.

		- Devices

			- Member: Read and manage all properties of owned devices
			- Guest: Can only delete owned devices

		- Applications

			- Member: register new applications
			- Guest: Can only delete owned applications

		- Policies

			- Member: Can only read properties and manage owned policies
			- Guest: None

		- Subscriptions

			- Member: read all subscriptions, enable service plan members
			- Guest: None

		- Roles and scopes

			- Member: read all roles and memberships
			- Guest: None

	- Features

		- B2B

			- Invite external users to tenant - available on all access tiers

		- B2C

			- Manage customers' identity. Available on pay-as-you-go.

		- AD DS

			- Adding VMs to domain without domain controllers. Org can use AD DS to handle infrastructure if running apps on-premises and in the Cloud. Pay-as-you-go.

		- App Management

			- Azure AD App Gallery Notifications
			- Custom applications
			- Non-gallery applications
			- On-premises applications

		- Conditional access policies

			- Pass additional authentication challenges before accessing app. P1 and P2 tiers.

		- Monitor app access

			- Monitoring app sign-ins. All tiers.

		- AD Identity Protection

			- Detect/remediate identity risks for users. Export data. Based on risk policies. P2.

	- Get started with AD

		- Deployment

			- Building a secure foundation

				- Assign more than one global admin
				- Use regular admin roles (not global admins for admins)
				- Configure PIM to track administrators (P2)
				- Configure self-service password reset
				- Banned passwords
				- Warnings for reusing credentials
				- Passwords to never expire for cloud-based user accounts
				- Enforce MFA through conditional access (P1)
				- Azure AD Identity Protection (P2)

			- Add users/manage devices/synchronise

				- AD connect - sync with on-premises instance of AD
				- Password hash synchronisation
				- Password writeback (P1)
				- Azure AD connect health (P1)
				- Licensing at a group level
				- Azure AD B2B
				- Device-management strategy 
				- Authentication methods not requiring passwords (P1)

			- Manage applications

				- Identify applications
				- SaaS applications in Azure AD Gallery
				- Integrate on-prem applications using Application Proxy

			- Monitor/review/automate

				- PIM to control admin access (P2)
				- Access reviews for Azure AD directory roles in PIM (P2)
				- Dynamic group membership policies (P1/P2)
				- Group-based application assignment
				- Automated user account provisioning and deactivation

			- Create a tenant
			- Associate a subscription

- On-Premises Active Directory

	- Use case: on-premises authentication and authorisation
	- Authentication 

		- Kerberos
		- NTLM

	- Structure

		- Forests

			- Domains

				- Organisational units

### Intro to Docker Containers 

- Containerisation - not configuring the software/hardware. More efficient. Security by isolating containers from one another.

Container - environment allowing us to run our software images. Has everything we need inside of it. 

Container images - has the software and dependencies in one package

	- Docker - containerisation platform (no hypervisor)

		- Use-cases

			- Managing hosting environemnts
			- Having all the dependencies in each deployment
			- Efficiency - use of hardware

				- Application
				- Libraries
				- Docker
				- Host OS

			- Portability
			- Isolation
			- Delivery
			- Deployments

				- Azure Container Instances
				- Azure App Service
				- Azure Kubernetes Services

			- Not using it:

				- Worrying about security - single host OS kernel point of attack
				- Virtualisation - some instances, may be better to use a VM
				- Monitoring is harder

		- Architecture

			- Client

				- Interacts with server, through REST API

			- Server

				- A daemon, responds to requests from client through REST API and interacts with other daemons

			- Objects

				- Supports deployments - networks/storage volumes/plugins, created/deployed as needed.

			- Docker Hub

				- SaaS docker registry, stores containers

			- Azure Container Registry

				- Store containers on Azure

		- Images

			- Looking at the complete stack of software 

				- I.e. a .NET Core MVC app, deployed using nginx reverse proxy, on Ubuntu Linux

			- Container image 

				- immutable - can't be changed
				- When run - becomes a container

			- Host OS 

				- OS on which Docker Engine runs (for managing file system/network/process shceduling/memory management)

					- On Linux, shares host OS kernel
					- Windows needs container OS

			- Container OS

				- OS part of the packaged image. Where the application is run in.

			- Creating the images with

				- Unionfs (Stackable Unification File System) for creating Docker images.

					- Container File System
					- Image 
					- Parent image

						- Image that you create your images from. E.g. image already based on Ubuntu. Usually has container OS.

					- Base image

						- image using the Docker scratch image. 
						- Assumes application can use host OS kernel
						- Doesn't create a filesystem layer
						- More control over the contents of teh final image

					- Boot file system

				- DockerFile

					- text file containing instructions to build/run the Docker image

				- Operations

					- Building/removing/listing images
					- Image tags - used for versioning. (latest being the default)

			- Containers

				- Operations

					- Running, pausing, unpausing, killing, stopping, restarting, containers
					- Container names are unique
					- Storage generalities 

						- Containers are temporary when it comes to storing data
						- Storage is coupled to the underlying host machine
						- Less performant

					- Volume

						- Stored on a host filesystem

					- Bind mount

						- Mounting any file or folder

					- Network configurations

						- Bridge

							- Default, internal, private network, can access other bridge connected containers

						- Host

							- Run container on host network directly

						- None

							- Disabled networking

						- OS considerations

							- Some networking configs may only be able on some operating systems

### Azure Data Storage

- Classify data

	- Structured/relational data

		- Adheres to a strict schema
		- Allows data to be easily searched e.g. SQL

	- Semi-structured data/non relational/noSQL, has variety to it

		- Expression and structure defined by a data serialisation language

			- Data serialisation languages used to write data from system to systems

				- XML
				- JSON
				- YAML

	- Unstructured data

		- files, photos, videos.

- Operational needs

	- Performance requirements

		- Are you going to be doing simple lookups using an ID?
		- Do you need to query for one or more fields
		- How many create/update/delete operations do you expect
		- Do you need to run complex analytical queries
		- How quickly do the operations need to be completed
		- How often is it accessed
		- Is it read only

- Transactions

	- A logical group of database operations that are executed together. Affects some other bit of data.

		- Use case

			- Will a change in one piece of data impact another? If yes, then need transactions
			- Product catalog data

				- Data classification: semi-structured (due to need to change schema)
				- Latency/throughput: high throughput, low latency
				- Transanctional support: data changes, so transactional support.
				- Azure Service: Azure Cosmos DB (supports semi-structured data, SQL, ACID compliant, data replication, consistency levels)

Also could use Azure SQL DB for common properties in SQL columns, and semi-structured data in JSON columns

Azure Table Storage, Azure HDInsight, Azure Cache for Redis, can store NoSQL data too.

Querying multiple fields, CosmosDB best fit. Indexes fields by default.

			- Photos and videos

				- Data classification: unstructured
				- Operations: retrieved by ID, high read operations, low latency, infrequent create/read, that can have higher latency than read operations
				- Latency/throughput: retrievals by ID need to be low latency and high throughput, create/updates can have higher latency
				- Transactional support: not required
				- Azure service: Blob service, uses the Azure CDN. Reduces latency. Different tiers: hot/cool/archive.

Could use Azure App Service for images on the server serving up the images, but better performance with CDN with Blob Storage.

			- Business data

				- Classification: Structured
				- Operations: Read only, complex analytical queries
				- Latency/throughput: some latency
				- Transactional support: not required
				- Azure service: Azure SQL Database. Allows for queries with SQL.

Azure Analysis Services for making semantic model over data in SQL database. Then using business intelligence tool to gain insights.

Azure Synapse: Supports OLAP solutions and SQL queries. Doesn't support cross-database queries. 

Azure Stream Analytics: real-time data.

		- Transactions requirements

			- Atomicity

				- Transactions must be executed all at once, or none of it is.

			- Consistency

				- Data is consistent before and after the transaction

			- Isolation

				- One transaction doesn't impact another

			- Durability

				- Changes made by the transaction are permanently saved

		- OLTP (Online transaction processing)

			- Transactional databases

				- Lots of users
				- Quick response time
				- Large volumes of data
				- Highly available
				- Simple transactions

		- OLAP (Online analytical processing)

			- Another form of transactional databases

				- Fewer users
				- Longer response times
				- Less available
				- Large/complex transactions

## Manage identities and governance in Azure

### Azure AD

- User accounts

	- Account access consists of

		- Type of user
		- Role assignments
		- Ownership of objects

	- Types

		- Administrators
		- Members
		- Guests

			- Adding through B2B

				- Better than AD FS, which requires an established trust, web app proxy required for outside users
				- AD FS use case: when you want local authentication when getting access to resources

			- By default, users/admins can invite guest users

- Permissions

	- Control access rights through roles

Has different permissions for each role

		- E.g. user administrator role
		- User Administrator
		- Global Administrator
		- Member user
		- Guest users

- Apps and resource access

	- Either on-prem, cloud-based, resources

		- Through Azure AD roles

			- Built-in roles

				- Owner
				- Contributor
				- Reader

			- Role definitions (P1/P2)

				- JSON file

					- ID
					- IsCustom
					- Description
					- Actions

						- * = all actions

					- NotActions
					- Data Actions
					- Data NotActions
					- Scope

						- / = entire scope (subscription, RGs, resources, etc)

				- Example

					- NotActions

						- Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write, Microsoft.Authorization/*/elevateAccess/Action

		- Also through RBAC roles

	- Assignment

		- Direct assignment
		- Group assignment
		- Rule-based assignment

- Architecture

	- Whole idea behind it

		- Identities for the user
		- Just enough access (for a time period)

	- Hierarchy

		- Directory/Tenant

			- Has multiple subscriptions

		- Subscription

			- Billing + security boundary
			- Has resources
			- Associated with a single AD Directory
			- Has an owner
			- Has multiple users and groups

		- Groups

			- Organises your users, to assign permissions to the whole group

				- Security groups

					- Manage access to shared resources

				- Microsoft 365 groups

					- Collaboration opportunities

	- Operations

		- Accessing Azure Resources

			- User account contains information to authenticate user
			- Authenticate
			- AD builds access token to authorise user 

		- User definition

			- Cloud identities
			- Directory-synchronised identities (AD Connect - SSO to access local and cloud-based resources)

				- Azure AD Connect

					- Integrate existing users and groups with Azure AD 

						- Sync services
						- Health monitoring
						- AD FS

							- Hybrid environment

								- Domain join SSO
								- AD sign-in policy
								- Smartcard/third-party MFA

						- Password hash synchronisation
						- Pass-through authentication

			- Guest users

				- Caveat: needs to create Microsoft Associated Account if they haven't already

		- Bulk adding users

			- Uses CSV files

		- Self service password reset

			- Use case

				- Reducing time spent resetting passwords for users

			- Operations

				- "Can't access your account"

					- Localisation
					- Verification
					- Authentication

						- Mobile App
						- Mobile App Code
						- Email
						- Mobile phone
						- Office phone
						- Security questions

					- Password reset
					- Notification

				- Can have a minimum number of auth methods. (1/2)

					- For administrators, two-method auth policy applied by default

				- Notifications on

					- users on password resets
					- All admins when other admins reset password

				- Licensing 

					- SSPR P1 or P2

				- Scope

					- Disabled
					- Enabled
					- Selected (for a certain security group)

		- Directory branding

			- Branding of organisation that shows up on the sign-in page (premium)

- Config drills

	- Adding users
	- Deleting users
	- Recover users
	- Creating AD organisation
	- Create groups
	- Add users to group

		- Statically
		- Dynamically

			- Through characteristics of user
			- Through characteristics of device

	- Adding guest users

		- Themselves through invite
		- To groups
		- To applications

### Azure RBAC

- Differences between Azure Roles and Azure AD roles

	- Azure resources and Azure AD have independent permission systems 

		- Azure roles

			- Manage access to resources (VMs, etc)

				- Common roles

					- Owner
					- Contributor
					- Reader
					- User Access Administrator

				- Scopes applied to

					- Management groups
					- Subscriptions
					- Resource groups
					- Resources

		- Azure AD roles

			- Manage access to AD resources (users accounts/passwords/domains)

				- Common roles

					- Global Administrator
					- User Administrator
					- Billing Administrator

				- Scopes applied to

					- Directory

- Elevating access when

	- Regaining access to Azure subscription or management group
	- Grant another user or yourself access to Azure subscription or management group
	- View all subscriptions or management groups
	- Grant automation app access to all subscriptions or management groups
	- When task is complete, after elevating access to User Access Admin, Global Administrator revokes their own permissions

### Azure Resource Manager

### Costs

### Moving resources around

## Implement and manage storage in Azure

## Deploy and manage Azure compute resources

### Creating Linux VMs

- On-demand compute service 

	- Considerations

		- Deployment

			- Portal
			- Scripts (PowerShell, CLI)
			- ARM templates

		- Resources (Contained within a resource group)

			- CPU and memory resources
			- Storage account to hold disks
			- Virtual disks to hold OS/applications/data
			- VNET to connect to other resources/on-prem
			- Network interface to communicate with VNET
			- Public IP (optional)

		- Image

			- Can have the OS and applications you need

		- Size

			- Determines processing power, memory, and storage capacity
			- Quota limits increased - open support ticket

		- Storage

			- HDD or SSDs (standard or premium for SSDs)
			- VHDs represent physical disks

				- Stored as page blobs in Azure Storage account
				- Choose whether they're HDDs or SSDs
				- Two VHDs created for VM

					- OS disk
					- Temporary disk
					- Additional disks: data disk

			- Unmanaged vs managed

				- Unmanaged

					- Making storage accounts yourself
					- Fixed IO rates

				- Managed

					- Azure takes care of the storage accounts

		- Network communication

			- Communicating through the VNET, that's divided up into subnets
			- Good idea perhaps to create the VNETs first as part of the design, and then place the VMs inside the VNETs as they're created
			- Remote access

				- Default way for Linux VMs is SSH

					- Allowing encrypted connection over unsecured connections

						- Authentication methods

							- Username/pass

								- Open to brute-force attacks

							- SSH key pair

								- Public keys

									- Placed in VM, shared with everyone

								- Private keys

									- For verifying identity, kept a secret

								- Across multiple VMs

									- Can use same SSH key pair across multiple VMs/services

								- Operations

									- Creating SSH key pair
									- Using SSH key pair with Azure Linux VM

										- Either when creating Linux VM
										- Or adding to existing Linux VM

									- Creating the VM

										- Standardised names

											- E.g. test-web-eus-vm1

										- IP address

											- Public IP can be used for communicating over the Internet

												- Dynamic IPs by default

											- Or set up a VPN connection from on-premises to Azure, and have private IPs on the VM

									- Connecting over SSH

										- Need public IP
										- Username
										- Public key
										- Access to corresponding private key
										- Port 22 open

									- Initialise data disks

										- Need to create a primary partition
										- Then a file system
										- Mount the drive

									- Install software on VM

										- E.g. Apache web server

									- Opening up ports

										- Such as port 80
										- Since only inbound traffic from VNET and load balancer allowed
										- Done through

											- Create NSG

												- Applied to

													- Interfaces
													- Subnets
													- Or both

											- Allow inbound traffic on certain ports
											- No NSG = all traffic allowed = security risk
											- Lowest priority first
											- Implicit deny rule
											- Ensure you lock down ports for administrative access

												- Creating VPN to link from private network to virtual network
												- Only allowing SSH/RDP from that address range
												- Could also change port for SSH

													- However only makes it a little harder for attacks to discover

### Creating Windows VMs

- Same on-demand computing resource, but with Windows installed on it

Managed with RDP, which offers the UI of the Windows computer

	- Deployment

		- Portal
		- Scripts (CLI/PowerShell)
		- ARM templates

	- Resources

		- CPU and memory
		- Storage account
		- Virtual disks

			- Applications
			- OS
			- Data

		- VNET
		- Network interface
		- Public IP (optional, or private)
		- Resource Group

	- Choosing the image
	- Sizing the VM

		- Different categories
		- Quota limits - submit support ticket

	- Storage options

		- HDD
		- SSD

			- Premium
			- Standard

	- Disks

		- OS disk
		- Temporary disk
		- Data disk

	- Unmanaged vs managed

		- Managed

			- Azure takes care of storage accounts

		- Unmanaged

			- You need to take care of storage account

	- Network communication

		- Through VNETs

	- Operations

		- Creating the Windows VM
		- Accessing VM

			- Through S2S connection from local network to Azure on-premises, to access VMs from local network
			- Or RDP (needs a public IP. Dynamic by default)

				- RDP provides remote connectivity

					- Control computer through UI of it
					- RDP clients

						- Windows

							- Remote Desktop Connection

						- macOS
						- iOS
						- Android
						- Linux

							- Remmina

				- Certificate warnings

					- Publisher warning

						- .rdp file needs to be signed

							- RDPSIGN.EXE

					- Certificate warning

						- Machine certificate not trusted

							- Machine certificate placed in client's trusted root certification authorities store

		- Installing software

			- RDP

				- Public IP, (or private if a VPN connection is made)
				- Port number

			- Custom scripts
			- Custom VM images

		- Opening up ports

			- By default they're locked down

				- E.g. ports 20 and 21 for FTP

### Manage VMs with Azure CLI

- Create/start/stop/management tasks related to VMs quickly/efficiently

Scripts for repetitive tasks

PowerShell for Windows admins, CLI for Linux admins/developers

	- Operations

		- Creating a Linux VM
		- SSH to Linux VM
		- Explore VM images
		- Sizing VMs
		- Query system and runtime information about a VM

			- JMESPath for query language for JSON objects

		- Stop/start/restart VMs
		- Installing software

			- Such as nginx web server
			- Opening up port 80

### Add and size disks in VMs

- Storage options

	- VMs use virtual hard disks (VHDs)

		- OS
		- Temporary storage
		- Data storage

	- VHDs stored as page blobs
	- Can be attached to VMs

- Operations (CLI)

	- Adding a data disk to a VM

		- Adding empty data disk to VM
		- Partition, format, mount data disk
		- Programmability - use custom script extension

	- Expanding disks

		- Can expand but not shrink
		- Some VM sizes only support standard disks
		- Need to deallocate VM before expanding disk on portal/CLI and then in the VM on Windows through Disk Manager or diskpart, and Linux, parted and resize2fs

### Securing Azure VM disks

- Protection for VM disks - encryption

	- Encryption

		- Symmetric
		- Assymetric
		- Key management

			- Managed by Microsoft or the customer

	- Methods

		- Azure Storage Service Encryption (SSE)

Automatic, by default

			- Physical disks in DC
			- Protecting data at rest
			- 256-bit AES

		- Azure Disk Encryption (ADE)

			- Encrypts the disk image
			- Managed by owner
			- Uses Bitlocker on Windows VMs, DM-crypt on Linux VMs

	- Operations

		- Encrypting existing disks with ADE

			- Creating key vault
			- Set key vault access policy to support disk encryption

				- Disk encryption
				- Deployment
				- Template deployment

			- Use key vault to store encryption keys for ADE

		- Process

			- Encrypting existing VM disks

				- Creating key vault
				- Set up key vault to support disk encryption
				- Telling Azure to encrypt VM disks in key vault

					- Creating backup of managed disks before encrypting

						- Snapshots
						- Azure Backup

				- Decrypting drives

			- Secure VM deployments by adding encryption to ARM templates

				- ARM templates JSON files. Can be generated for some resources
				- Example templates are available
				- Templates stored in Github

### Keeping VMs automatically updated (security and critical) - Azure Update Management

- Install OS updates and patches for Windows and Linux VMs, deployed on-prem, in Azure, other Cloud providers

	- Operations

		- No agents (?)/additional config
		- Run updates without logging into VM, no passwords
		- Easy-to-read format for missing updates and information
		- Using them among multiple subscriptions in same tenant

			- Machines in a different tenant - onboard them as non-Azure machines

		- Operating systems

			- Windows Server
			- CentOS
			- Red Hat Enterprise
			- SUSE Linux Enterprise
			- Ubuntu

		- Components

			- Microsoft Monitoring Agent (MMA) - Windows/Linux
			- PowerShell Desired State Configuration (DSC) - Linux
			- Automation Hybrid Runbook Worker
			- Microsoft Update/Windows Server Update Services (WSUS) - Windows PCs

		- Hybrid worker groups

			- Windows computers directly connected to Log Analytics workspace

		- Operations Manager Management packs

			- Systems Center Operations Manager group installs certain management packs in Operations Manager, and on Windows computers.

		- Process

			- Create VM
			- Onboard Update Manager to the VM
			- Verify agent connectivity and schedule recurring updates

				- Compliance scan for updates, then forward information to Log Analytics
				- Scheduled and recurring deployment of updates

					- Define what target computers receive updates, could be through groups in log analytics

						- Updates installed by runbooks in Azure Automation

				- Verifying agent connectivity by opening up Microsoft Monitoring Agent on the VM, and going to Azure Log Analytics tab
				- Also going to Event Viewer, Application and Services Logs, Operations Manager
				- Scheduling Update Deployments in the portal, in Update Management

### Building ARM templates

- Expressing deployments as code - deploying and deleting, automation

	- Azure Resource Manager

		- The thing that deploys Cloud resources
		- Organises Resource Groups
		- ARM templates

			- Defines all resoures in a deployment
			- Declarative

				- Define what you need, rather than how

			- JSON file
			- Benefits

				- Consistency
				- For complex deployments
				- Reducing manual tasks
				- Templates as code/IaC
				- Reusability
				- Linkable

			- ARM template specifics

				- JSON

					- Sending data between applications
					- Expressing data as an object
					- Parameters
					- Variables
					- Functions
					- Resources
					- Outputs

				- Writing them

					- Writing them from scratch
					- Taking existing templates and modifying them
					- Creating them based on existing group in portal
					- Azure Quickstart templates
					- Visual Studio Code ARM tools extension
					- Verifying with a JSON linter

				- Using

					- Validate template
					- Deploy template
					- Verify deployment
					- Extensions

						- Downloading and running scripts on Azure VMs

							- Custom Script Extension

								- Storing scripts in Azure Storage
								- GitHub

						- Creating multiple templates, linking them together
						- Modifying existing templates

					- Building the Custom Script Extension resource

						- Gathering requirements

							- What are the steps that you need to do?

						- Locating the resource schema

							- Such as finding Azure Quickstart template and modifying to your needs
							- Looking at the reference documentation

						- Specify required properties

							- Look at schema definition on reference documentation

						- Specify additional properties

							- Additional properties you may need

						- Specify dependent properties

							- Correct order to apply resources

						- Then build/verify/deploy the template

### Deploying Azure VMs from VHD templates

- Standardise/automate deployments of custom VMs. Generalised and specialised versions of images.

	- VHDs

		- Holds the OS, applications, data, and runs on a VM
		- Several benefits compared to physical hard disks

			- HA/reliability
			- Physical security
			- Durability
			- Scalabilty
			- Cost/performance

		- VHDs created from VM image. Can include extra software
		- Generalised image (removing VM specific data)

			- Creating from scratch

				- Blank virtual disk
				- Create a VM
				- Install OS and software

			- Customising image from Azure Marketplace
			- Considerations

				- Resetting certain OS data to a default state (generalisation)

Done using a tool e.g. Sysprep for Windows, waagent for Linux

					- Host name of VM
					- Username/credentials
					- Log files
					- Security identifiers

		- Specialised virtual image

			- Has specific information attached to it
			- Snapshot of a VM at a certain point in time

		- Processes

			- Generalising a server

				- Windows

					- Sysprep
					- Back up VM first before running Sysprep
					- Stopping/deallocated/generalise VMs

				- Linux

					- Waagent
					- Deallocate/generalise VMs

			- Creating images from generalised VMs

				- Capture in the Portal
				- Creating VMs from generalised image
				- Multiple VHDs within one virtual image

					- Can take snapshots of VHDs

				- Rebuilding VMs from VHD snapshots
				- Specific: creating image of an Azure VM from the Azure CLI and provision a new VM (custom image)

					- Have custom VM with specific software running on it
					- Generalise the VM (e.g. run sysprep)
					- Create image from generalised VM
					- Use image to create another VM

### Building a scalable application with VM scale sets

- Adjusting to changes in load, minimising costs

	- Virtual machine scale sets

		- Use case

			- Running applications on a set of VMs
			- Bringing up/down VMs in an agile manner, based on demand
			- Handle fluctuating loads with a load balancer, based on usage
			- Same configs on each VM

		- Operations

			- Health probe pings the instance to check availability (done through a certain port)
			- Horizontal scaling
			- Vertical scaling
			- Scaling

				- Scheduled scaling
				- Autoscaling (metrics)

					- Based on metrics - such as CPU usage/response time/network flow
					- Scale rules, metrics, scale actions

			- Low-priority scale sets

				- Provisioning VMs through underused compute resources

					- Kinds of removal

						- Delete
						- Deallocate

		- Config

			- Deploying scale sets

				- By default, 2 instances + a LB

			- Configuring VMSS

				- Deploying it
				- Scale-in and scale-out rules

			- Installing/updating applications in VM scale sets

				- Through custom script extension

					- Through portal
					- With ARM templates as part of templated deployment
					- Upgrade policy modes

						- Automatic

							- Causes service outage

						- Rolling

							- In batches across VMs

						- Manual

							- Existing VMs aren't updated. Default mode

### Protecting VM settings with Azure Automation State Configuration (config consistency)

- Desired state configuration script, checks that a service is installed.

Onboard VMs for management with Azure Automation

Automatically install service on VMs when that service is missing

	- Benefits

		- Prevents configuration drift

	- Operations

		- Config consistency in a VM cluster
		- Declarative
		- Azure Automation State Configuration is built on PowerShell Desired State Configuration

			- DSC handles use cases and offers that declarative language to define the end state

		- Pull server. Target nodes to pull configs, conform to desired state, report back
		- Virtual or physical appliances
		- Azure Monitor logs to view compliance
		- Local configuration manager

			- Windows Management Framework

				- Responsible for updating node to reach desired state

					- Get
					- Test
					- Set

				- Modes

					- Push

						- Admin manually sends config to nodes. LCM does its get/test/set process

					- Pull

						- Pull server holds the config information. LCM on each node polls pull server at regular intervals

	- Config

		- Using PowerShell DSC to specify desired state for a VM

			- Built in PowerShell DSC resources
			- Installing/configuring software and services even if you're not familiar with the technical steps to do so
			- DSC Resource Kit - complex resources like AD integration
			- DSC code block

				- Configuration
				- Node
				- Resource
				- MyDscConfiguration

			- Configuration data blocks

				- Providing data configuration needs
				- To each node or globally

			- Secure credentials

				- PSCredential object to avoid storing credentials in plaintext
				- Encrypting by using certificates

			- Pushing configs

				- Start-DscConfiguration cmdlet

			- Pulling configs

				- For more VMs
				- Create Azure Automation account
				- Configure Azure Automation account as a pull service
				- Register VMs with the account
				- Add DSC Config script
				- Add required modules
				- On the VM

					- Install DSC VM Extension
					- Install WMF
					- LCM applies desired state

				- Creating Azure Automation account, uploading PowerShell DSC, onboarding existing VM into Azure Automation, then running code to install/configure service on VM

### Deploying and running a containerised web app with Azure App Service

- Creating Docker images, storing them in a repository in Azure Container Registry, deploying web app based on docker image, continuous deployment for web app through webhook monitoring Docker image for changes

	- Azure Container Registry

		- Store Docker images in an Azure storage account
		- More secure
		- Scalability
		- Automate tasks such as redeploying apps upon images being rebuilt

	- Config

		- Building and storing images
		- Deploying web apps from images in ACR
		- Updating the image and automatically redeploying the app

			- Through webhooks - app subscribes to webhook to receive notifications about updates to an image
			- Container registry tasks, building something based on source code in a version control system

				- May ned access token

### Scaling App Service apps to meet demand

- Through scaling up and scaling out

	- Methods

		- Manually
		- Monitoring metrics and then scaling 

	- Different App Service plans
	- Process

		- Creating App Services plan
		- Deploying a web app using the plan
		- Monitoring performance
		- Scaling out the app

			- Or up, which may cause interruptions

		- Verifying performance improvements

### Running Docker containers with Azure Container Instances

- Process

	- Running containers in Azure Container Instances

		- Why?

			- Fast startup
			- Per second billing
			- Hypervisor-level security
			- Custom sizes (systems resources, CPU, memory, IO, etc)
			- Persistent storage
			- Linux and Windows

	- Control what happens when container exits
	- Environment variables to configure container
	- Data volume for persistent data
	- Troubleshooting

- Config

	- Creating the container

		- Select the image
		- DNS label
		- Open ports

	- Control restart behaviour

		- Configurable restart policies

Stop when process completed

			- Always

				- Ensuring processes are available even if a restart is required

			- Never

				- Never restarting

			- OnFailure

				- Only restarting on failure

	- Setting environmental variables

		- Configure application or script the container runs

			- Normal environment variables
			- Secured environment variables

	- Creating file shares
	- Get storage credentials

		- Storage account name
		- Share name
		- Storage account access key

	- Deploy containers and mount the file share
	- Troubleshooting

		- Pulling container logs
		- Viewing container events
		- Attaching to a container instance
		- Executing commands in containers
		- Monitoring CPU and memory usage

### Intro to Azure Kubernetes Service

- Container orchestration, deployment management, automatic updates, self healing

	- Containers (Writing again in case you're new like me and forget easily - good time to lab it up)

		- Packages code/dependencies/configuration in one
		- Splitting up applications into services
		- Immutable
		- Lightweight
		- Fast
		- Disposable
		- Start/stopped/destroyed as needed

	- Kubernetes

		- Declarative config to orchestrate containers
		- Deployment as clusters

			- One master machine at least
			- One or more workers machines/agent nodes

		- Azure Kubernetes Service

			- Makes it simple to deploy and manage containerised applications

	- Process

		- Creating AKS cluster

	- Use cases

		- Identity and security management

			- AKS cluster integrated with Azure AD

		- Integrated logging and monitoring

			- Azure Monitor

		- Auto Cluster node and pod scaling

			- Horizontal pod autoscaler or cluster autoscalers

		- Cluster node upgrades

			- Managed by AKS

		- GPU enabled nodes

			- Supported by AKS

		- Storage volume support

			- Static and dynamic storage volumes in AKS

		- Virtual network support

			- AKS clusters deployed in VNETs

		- Ingress with HTTP application routing support

			- HTTP application routing add-on in AKS

		- Docker image support

			- AKS supports Docker image format

		- Private container registry

			- AKS integrates with Azure Container Registry

## Configure and manage virtual networks for Azure administrators

### Azure network capabilities, connectivity services, application protection, application delivery, network monitoring services

(This is where it starts getting fun and in my side of things. That being said, everything else is interesting and nice to learn. What gets used? Depends on the business requirements)

- Configuring the network for VMs

	- Azure virtual networking

		- Allows Azure resources to communicate with one another

			- Features

				- Isolation/segmentation (subnets)

					- Through subnets
					- Name resolution through internal/external DNS, or built in DNS into Azure

				- Internet communications (through VPN)

					- VMs connecting out to Internet by default
					- Incoming connections from the Internet through public IP or public LB
					- VM management via CLI, RDP, SSH

				- Communication between resources

					- VNETs
					- Service endpoints

						- Linking Azure resources to other VNETs
						- Such as Azure Key Vault

				- Communication with on-premises (through VPN)

					- P2S VPNs
					- S2S VPNs
					- Azure ExpressRoute

				- Routing

					- Route tables
					- BGP

				- Filtering

					- NSGs
					- NVAs

				- Connecting VNETs

					- Peering

			- Config

				- Creating a VNET
				- Creating two VMs attached to VNET
				- Connecting to one VM and prove network connectivity to other VM in same network
				- Creating a VPN gateway (point to site)

					- Creating a route based VPN gateway
					- Uploading the public key for a root certificate for authentication purposes
					- Generating a client certificate from the root certificate, installing client certificate on each client computer that connects to the VNET
					- Creating VPN client config files

			- VPN gateways

				- For integrating on-premises environment with Azure
				- Encrypted
				- Types of connections

					- Network-network
					- Site-to-site

						- Over the Internet
						- Or a dedicated network (ExpressRoute)

					- Point-to-site

						- Over the Internet

				- Planning factors

					- Throughput
					- Backbone
					- Availability of static IPs
					- VPN device compatibility
					- Multiple client connections, or site-to-site link?
					- VPN gateway type

						- Route based
						- Policy based

					- VPN gateway SKU

				- Workflow for cloud connectivity

					- Design connectivity topology, with address spaces
					- Create VNET
					- Create VPN gateway
					- Create and configure connections to on-prem and other VNETs
					- Create/configure P2S connections for VPN gateway

				- Design considerations

					- Subnets can't overlap
					- IP address must be unique
					- VPN gateways must have gateway subnet called GatewaySubnet

			- Azure ExpressRoute

				- Private connection

					- Faster speeds, 50mbps-10gbps
					- Lower latency
					- Reliability
					- Secure
					- Connectivity to all Azure services
					- Global connectivity (premium add on)
					- Dynamic routing over BGP
					- SLAs for uptime
					- QoS

				- Connectivity models

					- IP VPN network
					- Virtual cross-connection through Ethernet exchange
					- Point-to-point Ethernet connection

				- L3 connectivity

					- Through BGP

				- Operations

					- ExpressRoute circuits
					- Routing domains
					- Monitoring (Network Monitor)

						- Availability
						- Connectivity
						- Bandwidth

### Designing IP addressing scheme for Azure deployment

- Planning public and private IPs, with room for growth

	- Subnets

		- Smallest one has a /29 mask
		- Largest one has a /8 mask
		- Ensuring there's no private IP address overlap with on-premises
		- First three IPs and last IPs reserved in a subnet, number of hosts is 2^n-5 (n being host bits)

	- Requirements

		- How many devices do you have?
		- How many devices are you planning to have?
		- What devices need to be separated?
		- How many subnets? Devices per subnet?
		- How many devices are you going to add to the subnets?
		- Sizes of subnets?

	- Config

		- Creating virtual networks and subnets inside of them

### Distributing services across VNETs and integrating them with VNET peering

- Connecting VNETs together. Allows resources to communicate with one another. Routed through Azure network.

Used in hub-spoke topology.

	- Types of peering

		- Virtual network peering (same region)
		- Global VNET peering (different regions)

	- Process/considerations

		- Connecting through creating connections in each VNET
		- Works even when VNETs are in different subscriptions (might need to grant roles though to administer)
		- Nontransitive - only directly peered VNETs can communicate with one another

			- A-B-C

				- To get A to talk to C, need to configure a peer from A to C

		- Gateway transit - enabling on-premises connectivity without connecting VNET gateways to all VNETs. Sharing hub VNETs VNET gateway to peers
		- Can't overlap addresses between peers
		- Connectivity between VNETs possible through ExpressRoute circuit or VPN gateways.

			- Gateway and peering configured, peering gets chosen for traffic flow

		- Use case

			- Network connectivity between services in different VNETs
			- Alternatives to existing VPN or ExpressRoute connections, accessed from a peered VNET?
			- Services behind Azure basic LBs, accessed from a peered VNET?

	- Config

		- Creating VNETs
		- Creating VMs in each VNET
		- Configure peering connections

			- Don't forget to do it reciprocally 

		- Verify by SSH between VMs

			- Lack of connectivity for transitive connections

### Securing and isolating access to Azure resources through NSGs and service endpoints

- Securing resources from unauthorised access (through NSGs and service endpoints)

	- NSGs

		- Filtering traffic to/from resources through security rules

			- Lower priority takes precedence
			- Stateful connections
			- Default rules

				- Inbound

					- 65000 AllowVnetInbound
					- 65001 AllowAzureLoadBalancerInbound
					- 65500 DenyAllInBound

				- Outbound

					- 65000 AllowVnetOutbound
					- 65001 AllowInternetOutbound
					- 65500 DenyAllOutBound

			- Augmented rules

				- Multiple parameters in one

					- Multiple IPs
					- Multiple ports
					- Service tags
					- App security groups

			- Service tags

				- Allow/deny to a specific service

			- App Security Groups

				- Applying security rule to group of resources (network interfaces)

		- Applied to network interface or subnet (for lesser administration)
		- Applied to both too

			- Inbound

				- Subnet evaluated first then interface

			- Outbound

				- Interface evaluated first then subnet

		- Supports TCP/UDP/ICMP, L4.
		- One NSG applied per subnet or interface
		- Process

			- Creating VNETs and NSGs
			- Create VMs
			- Checking connectivity
			- Creating security rules
			- Creating app security groups

	- Service endpoints

		- Direct connection to Azure services within a VNET (without exposing to the Internet)

			- Reduces the attack surface
			- Process

				- Turn off public access to a service
				- Add service endpoint to a VNET
				- Accessing from on-premises using NAT IPs, used by ExpressRoute. 

					- Added into firewall config of Azure service

			- Config

				- Adding rules to allow outbound access to service endpoint from within the VNET
				- Possibly denying outbound access to the Internet
				- Configuring storage accounts and file shares
				- Enabling service endpoint

### Connecting to VMs through portal through Azure Bastion

- Connecting to VMs directly, acting as a jumpbox. Managing and monitoring.

	- Bastion

		- Remote connection from portal to VMs over TLS
		- Used in same VNET (or peered) as VMs to connect to them through the jumpbox
		- VMs IPs not publicly exposed
		- NSGs can be applied to subnets to allow RDP/SSH from Bastion only
		- Process

			- Creating Bastion subnet
			- Creating public IP for Bastion
			- Creating Bastion resource
			- Monitor and manage remote sessions

				- Configuring diagnostic settings in Bastion
				- Viewing current sessions in sessions in Bastion

		- Config

			- Creating VMs
			- Creating subnet for Bastion
			- Deploying Bastion
			- Connecting to VMs

### Hosting domain on Azure DNS

- Creating DNS zone, DNS records.

	- Azure DNS

		- A hosting service for DNS domains
		- Resolves the domain to the IP address
		- Using Microsoft infrastructure
		- DNS

			- Protocol for translating domain names to IPs
			- Uses a global directory comprised of DNS servers
			- DNS server

				- Has local cache of locally used domains and IPs
				- Can't find domain - passes onto another server
				- Maintains key-value pairs of IP addresses and hosts/subdomains that DNS server has authority over
				- Network enabled devices use DNS servers to access web-based resources
				- Domain lookup requests

					- Checks to see if domain name stored in cache
					- Then contacts DNS servers on the web
					- Then responds with a 'domain cannot be found error' if all else fails.

				- DNS settings

					- Configured for each host type

				- Record types

					- A

						- Host record, maps domain to IP

					- CNAME

						- Name that isn't an alias

					- MX

						- Mail exchange record, mapping mail requests to mail server

					- TXT

						- Text record, associate text strings with domain name (can be used to verify domain ownership)

					- And more
					- Record sets

						- Multiple resources defined in a single record

		- Use-cases

			- Security (Doesn't support DNSSEC though)

				- RBAC
				- Activity logs
				- Resource locking

			- Ease of use

				- Managing DNS records for Azure services, and DNS for external resources

			- Private DNS domains

				- Name resolution for resources within a VNET and between VNETs
				- Own custom domain names rather than Azure provided names

			- Alias record sets

				- Point to an Azure resource (A, AAAA, CNAME)

					- Public IP
					- Traffic Manager profile
					- CDN endpoint

		- Process (public DNS zones)

			- Creating DNS zone
			- Getting Azure DNS name servers
			- Update the domain registrar setting (NS details, domain delegation)
			- Verify delegation of domain name services (nslookup the SOA record)
			- Configure custom DNS settings (for resources)

				- A record

					- Name
					- Type
					- TTL
					- IP address

				- CNAME record

					- Name
					- TTL
					- Record type

		- Process (private DNS zones)

			- Creating private DNS zone
			- Identify virtual networks
			- Linking virtual network to a private DNS zone

		- Config (public DNS zones)

			- Create a DNS zone
			- Create a DNS record
			- Verify (nslookup)

		- Aliases

			- For improving resiliency, through a load balancer.

				- Apex domain

					- Highest level of domain e.g. wideworldimports.com (@ symbol in DNS zone records)
					- Apex domains automatically created

						- NS
						- SOA

					- CNAME records

						- Aren't supported at zone apex level

					- Alias records

						- Supported at apex level
						- Enables zone apex domain to reference other Azure resources from the DNS zone 
						- Routing traffic through traffic manager
						- Or through front door profiles or CDN endpoints
						- Supported record types

							- A
							- AAAA
							- CNAME

			- Use-cases

				- Prevents dangling DNS records (coupling the lifecycle of an Azure resource with DNS record)
				- Updates DNS record when IP addresses change
				- Hosts load-balanced apps at the zone apex (routing to traffic manager)
				- Points zone apex to CDN endpoints (directly reference CDN instance)

			- Config (create alias records)

				- Setting up VNET, LB, VMs
				- Creating alias record in zone apex
				- Verifying alias resolves to LB

### Managing and controlling traffic flow with routes

- Routing

	- System routes assigned by default to each subnet in a VNET

		- Unique to the VNET, next hop type VNET
		- 0.0.0.0/0, next hop type internet
		- 10.0.0.0/8, next hop type none
		- 172.16.0.0/12, next hop type none
		- 192.168.0.0/16, next hop type none
		- 100.64.0.0/10, next hop type none

	- Can be overridden with custom routes

		- One use case is for routing traffic through an NVA or a firewall
		- User defined routes to route traffic someplace specific

			- Next hop types

				- Virtual appliance - typically a firewall. Specify private IP of NIC attached to a VM or LB.
				- Virtual network gateway - routes for a specific address sent to a virtual network gateway
				- Virtual network - overriding default system route in a network
				- Internet - traffic to a specified address prefix routed to the Internet
				- None - dropping traffic sent to a specified address prefix

	- Additional system routes through

		- VNET peering

			- Allowing for VNETs to be connected to one another, creates routes

		- Service chaining

			- User defined routes between peered networks, overriding peering routes

		- VNET gateway

			- For sending encrypted traffic between on-premises and Azure. Contains routing tables and gateway services

		- VNET service endpoint

			- Direct connections to resources. Creates routes in routing table

	- BGP

		- Exchanging routes using BGP, from VNET gateway to gateway. 
		- Also can be from VNET to VNET.
		- Network stability

	- Route selection

		- Longest prefix match
		- (If same prefix) user-defined routes
		- BGP routes
		- System routes

	- NVAs

		- Virtual appliance (get from Azure marketplace)

			- Firewall
			- WAN optimiser
			- Application-delivery controllers
			- Routers
			- Load balancers
			- IDS/IPS
			- Proxies
			- SD-WAN edge

		- Deployment

			- In a perimeter network subnet, examining traffic going to other subnets
			- Microsegmentation

				- Dedicated subnets for the firewall, applications/services in other subnets

			- User defined routes

				- Accessing Internet via on-premises network through forced tunnelling
				- Virtual appliances to control traffic flow
				- Each subnet has only one routing table

			- High availability

				- An important consideration

	- Config

		- Creating custom routes
		- Deploying NVA
		- Enabling IP forwarding for the network appliance
		- Create resources, route traffic between them through the virtual appliance (check with traceroute)

### Connecting on-premises network to Azure using VPN gateway

- VPN gateway

	- Connecting stuff together

		- On-premises to Azure VNET - site-to-site
		- Remote device to Azure VNET - point-to-site
		- Azure VNET to VNET - network-to-network

	- VPN type

		- Policy-based

			- Specify IPs of packets encrypted through the tunnel

		- Route-based

			- IPSec tunnels modeled as a VTI, IP routing decides which tunnel to send out of

	- Authentication

		- PSK

	- Different VPN gateway sizes
	- Deployment

		- Necessities

			- VNET
			- GatewaySubnet
			- Public IP
			- Local network gateway (on-premises config info)

				- On-premises requires a VPN device and a public IP

			- VNET gateway
			- Connection (logical connection)

	- High availability

		- Active/standby
		- Active/active
		- ExpressRoute failover
		- Zone-redundant gateways

	- Config

		- Creating VNETs, subnets, local network gateway (to simulate on-premises to azure connection between two VNETs)
		- Creating VPN gateways
		- Updating remote IPs
		- Creating connections

			- On both sides, referring to the opposite local gateway

		- Verifying the connection

### Connecting on-premises network to Microsoft global network using ExpressRoute

- Private/reliable/high bandwidth connection from on-premises to Azure

	- How it works

		- Circuits
		- Peering on-premises network to networks in the Microsoft Cloud through endpoints
		- BGP sessions for routing domains configured
		- NAT service to translate private to public IPs
		- Reserved blocks of traffic for routing to the Microsoft Cloud
		- Private peering
		- Microsoft peering
		- Creating circuit
		- Creating VPN gateways
		- Creating connections

	- Use-cases

		- Low latency connectivity to the Cloud
		- Accessing high-volume systems in the Cloud
		- Using Microsoft Cloud services
		- Seamless migration from on-premises to Azure
		- Private data connection
		- Large DCs

	- Benefits

		- Predictable performance (SLAs)
		- Data privacy
		- High throughput, low latency
		- Availability and connectivity

	- Alternatives

		- S2S VPNs
		- P2S VPNs

### Improving application scalability and resiliency with Azure Load Balancer

- Fault tolerance and scalability when it comes to applications

	- Azure Load Balancer

		- Distributing traffic based on 5-tuple hash

			- Source IP
			- Source port
			- Destination IP
			- Destination port
			- Protocol type

		- Inbound and outbound
		- Low latency, high throughput
		- Scaling up to many TCP/UDP flows
		- High availability with availability sets and availability zones

			- Availability sets

				- Isolating VMs by having them in different racks

			- Availability zones

				- Groups of DCs with independent systems resources

		- Basic vs standard

			- Basic

				- Port forwarding
				- Auto reconfig
				- Health probes
				- Outbound SNAT
				- Diagnostics with Azure Log Analytics
				- Availability sets only

			- Standard

				- HTTPS Health probes
				- Availability zones
				- Azure Monitor diagnostics
				- HA ports
				- Outbound rules
				- Guaranteed SLA 99.99%

		- Internal vs external LBs

			- External

				- Permits traffic from Internet
				- Public IP
				- Distributes traffic among VMs

			- Internal

				- No traffic from the Internet
				- Private IP
				- Distributes traffic from internal Azure resources to other Azure resources

		- Config

			- Distribution modes (default is distributing traffic equally among VM instances)

				- Five-tuple hash

					- Incompatible with RDP
					- Best for scalability and resilience

				- Source IP affinity (2-tuple or 3-tuple hash) - requests from a specific client sent to same VM behind LB

					- Compatible with RDP
					- For uploading media

			- Deploying an application
			- Creating load balancer (with public IP)
			- Creating health probe
			- Creating load balancer rules
			- Connecting VMs to back-end pool

### Troubleshoot inbound network connectivity for Azure LB

- Multidimensional metrics, Azure Monitor metrics, health probe status

	- Operations

		- At L4
		- Routing and address translation rules
		- Health probes to determine availability
		- Components

			- Front-end IP
			- Back-end pool

				- Adding additional VMs and IPs at any time - scalable

			- Routing rules

				- Specify how requests towards front-end IP are mapped to a back-end pool
				- No match = discard
				- Session persistence can be configured

			- Health probes

				- Determine whether VMs are available through pings

			- Collection of VMs

				- All in a VNET subnet
				- Protected with NSG

		- Problems

			- Unreachable application
			- Unreachable VMs
			- Slow response times
			- Timed out user requests
			- Probing issues

				- Incorrect configuration - wrong URL/port
				- Ports aren't open on VM

			- Data path issues (unable to route)

				- NSG/firewall blocking ports or IPs
				- VM down, not responding, failing, security issue (expired certificate)
				- Application isn't responding, overloading, listening on incorrect port, crashing application

		- Diagnosing issues by reviewing configuration and metrics

			- Azure Monitor

				- Monitor connectivity in Metrics

					- Data path availability

						- Value between 0 and 100

					- Health probe status

						- Value between 0 and 100

					- Service health

						- Resource health

					- Different metrics

			- Common problems

				- VMs not responding to traffic on probe port

					- Not listening in on correct port
					- NSG blocking ports
					- Accessing LB from same VM and VNIC
					- Accessing LB front end from VM in the back end pool

				- Unhealthy VMs

					- Check that the VM responds to ssh/ping/rdp from another VM in the back-end pool
					- Check that the application is running if VM works
					- Netstat -an to see if ports are listening, for the health probe and the application

				- Misconfigurations

					- Validating routes through various tools

						- psping
						- tcping
						- netsh
						- Message analyzer/wireshark

				- VM firewall or NSG blocking ports
				- Limitations of LB

					- Load balancing and port forwarding for TCP/UDP only, not ICMP
					- L4 only
					- Invisible to the clients
					- Load balancing based on contents of messages

						- Azure Application Gateway
						- Proxy web server

			- Config

				- Creating environment (with LB and VMs)
				- Monitoring health through charts
				- Reconfiguring LB and retesting
				- Testing VMs
				- Checking health probes and routing rules (in service health. VIP = routing rule, DIP = health probe)
				- Checking NSG rules

### Load balancing traffic with Application Gateway

- Application layer routing to a pool of servers based on URL

	- Routing

		- Path-based routing

			- Sending requests with different paths in the URL to different back-end servers

		- Multiple site routing

			- Configuring more than one web application on the same gateway instance
			- Multiple DNS names (CNAMES) for the IP address of the application gateway. Each specificies the name of a site, ending up with routing to different servers in the back-end pool

		- Redirection
		- Rewriting HTTP headers
		- Custom error pages

	- Load balancing

		- Works at L7
		- Round-robin automatically
		- Session stickiness can be configured - routing to the same server during a session
		- Based on host names and paths
		- Support for HTTP/HTTPS/HTTP/2/WebSocket
		- Web application firewalls
		- End-to-end request encryption
		- Autoscaling
		- Has health probes

	- Config

		- Creating web sites (VMs, app service)
		- Application gateway config/creation

			- Front-end IP
			- Listeners

				- Based on protocol/port/host/IP
				- Routes requests based on routing rules
				- Basic

					- Routes request based on path in URL

				- Multi-site

					- Routes request based on hostname of URL

				- Handles SSL certificates

			- Routing rule

				- Binds a listener to the back-end pools
				- Interpreting hostname and path elements in the URL of a request
				- Then directing to an appropriate back-end pool
				- HTTP settings

					- Whether traffic is encrypted between Application Gateway and back-end servers
					- HTTP/HTTPS
					- Session stickiness
					- Conenction draining
					- Request timeout period
					- Health probes, probe URL, timeout

			- Back-end pool

				- References collection of web servers
				- Providing IP of web server and port on which it listens on

			- WAF

				- Handles incoming requests before reaching a listener

					- Checks for threats (OWASP)
					- SQL injection
					- Cross-site scripting
					- Command injection
					- HTTP request smuggling
					- HTTP request splitting
					- Remote file inclusion
					- Bots/crawlers/scanners
					- HTTP protocol violations and anomalies
					- Different OWASP rule sets

			- Health probes

				- Determine which servers are available for load balancing (HTTP status 200-399, healthy)

			- Network requirements

				- Requires a VNET to operate in
				- Dedicated subnet
				- Uses a number of private addresses
				- Exposed through public IP or private IP

			- Tiers

				- Standard

					- Small
					- Medium
					- Large
					- V1
					- V2

						- Supports availability zones

				- WAF

					- Small
					- Medium
					- Large
					- V1
					- V2

						- Supports availability zones

			- Scaling

				- Manual scaling and autoscaling

			- Actually creating

				- Configuring network
				- Creating application gateway
				- Creating back end pools
				- Creating front end ports
				- Creating listeners
				- Health probes
				- HTTP settings
				- Creating path maps
				- Creating routing rules
				- Deleting original rule

### Monitor and troubleshoot Azure network infrastructure with network monitoring tools

- Network Watcher monitoring, network diagnostics tools, metrics, logs, to help solve networking issues

	- Tools

		- Monitoring

			- Topology

				- Graphical display of VNET

			- Connection Monitor

				- Checking if there's communication between resources
				- Measures latency
				- Probing VMs for changes
				- Examining IPs or FQDNs

			- Network Performance Monitor

				- Centralised view of network
				- Track and alerts on latency and packet drops
				- Endpoint to endpoint connectivity

		- Diagnostics

			- IP flow verify

				- If packets are allowed/denied for a VM. If NSGs deny. Based on IPs, protocol, and ports used.

			- Next hop

				- Determine how a packet gets to a VM

			- Effective security rules

				- Displaying effective NSGs applied at an interface, and for open ports

			- Packet capture

				- Record packets sent to/from a VM
				- Uses Network Watcher Agent VM Extension installed on the VM

			- Connection troubleshoot

				- Check TCP connectivity between VMs
				- Shows latency, probe packets sent, hops

			- VPN troubleshoot

				- Diagnose problems with VNET gateway connections

	- Use-cases

		- Connectivity issues

			- IP flow verify

		- VPN connections aren't working

			- VPN troubleshoot

		- No servers listening on destination ports

			- Connection troubleshoot

	- Metrics and logs

		- (Metrics) Usage and quotas

			- Each resource has its quotas
			- Based on usage and resource location

		- Logs

			- NSG Flow logs

				- Ingress and egress IP traffic on NSGs
				- JSON file
				- Visualising flow logs in Power BI
				- Or Elastic Stack/Grafana/Gray Log

			- Diagnostic logs

				- Enable and disable logs for network resources
				- Query/view log entries for those that interest you

			- Traffic analytics

				- Investigate user and app activity across Cloud networks
				- Network activity across subscriptions
				- Diagnose security threats
				- Analyses NSG flow logs across subscriptions
				- Optimise network performance with data
				- Requires log analytics

		- Use-cases

			- Slow performance

				- Azure diagnostics, review the data. Spot trends in the graph.

					- CPU bottlenecks
					- Memory bottlenecks
					- Disk bottlenecks

			- VM firewall rules block traffic

				- IP flow tool, NSG flow logging

			- Inability on front and back end subnets to communicate

				- IP flow verify tool
				- Enable specific ports and protocols required

		- Config

			- NSG flow logging requires microsoft.insights provider, need that configured
			- Create storage account for NSG flow logs
			- Create log analytics workspace
			- Enable flow logs
			- Generate test traffic
			- Diagnose the problem
			- Fix the problem
			- Restart connection

	- Config

		- Configuring VNET and VMs
		- Enable network watcher for region, and in resource group
		- Show the topology
		- Connection monitor to run tests

## Monitor and back up Azure resources

### Monitoring and implementing backup and recover procedures in Azure

- Developing a holistic monitoring strategy

	- Full stack monitoring (situational awareness)

		- Monitoring, triage, diagnosis, of application, infrastructure, and security issues.
		- Applications monitoring

			- Alerts and automated responses
			- During development and deployment

		- Infrastructure monitoring

			- Alerts and automated responses
			- Operational analysis and capacity planning

		- Platform resources monitoring

			- Storage Accounts, Key Vaults, Cosmos DBs
			- Performance metrics and resource logs
			- Visibility into platform resources application and infrastructure depend on

		- Security monitoring

			- Anomaly detection and event management, for combining related events into a single alert

	- Monitoring tools (Security Center and Sentinel both use Monitor Logs as underlying logging data platform)

		- Azure Monitor

			- Collecting from application, infrastructure, platform, and custom sources
			- Information collected and stored in centralised data stores
			- Manage and create alerts, then do stuff based on them - through runbooks and autoscale
			- Azure Monitor Application Insights

				- Analysing stuff related to application's health and performance
				- Improve application's development lifecycle
				- Measure user experience and behaviour
				- Monitoring availability, availability tests
				- Alert rules

					- Responding with runbooks and webhooks

			- Azure Monitor Insights

				- Azure Monitor for VMs

					- Top N Lists under Performance to view highest resource using VMs
					- Includes map tab, with connections

				- Azure Monitor for Containers

					- Could be used for monitoring kubernetes clusters

				- Insights Hub

					- Complete list of insights

		- Azure Security Center

			- Monitoring security of workloads, on-premises and in the Cloud
			- Agents on VMs collect data

				- Log analytics agent

			- Understanding security posture

				- Secure score

			- Identify/addressing risks

				- E.g. JIT VM access
				- Adaptive application controls

			- Securing an infrastructure

				- Responding to threats through security alerts

		- Azure Sentinel

			- Collect data on devices/users/infrastructure/applications across enterprise
			- Hunt for threats and anomalies

				- Threats with AI

			- Respond with orchestration and automation
			- Connecting data sources to Sentinel

				- Log Analytics workspace added to Sentinel
				- Microsoft and non-Microsoft connectors

			- Playbooks to automate responses to alerts
			- Notebooks to automate investigations

### Monitoring health of VMs with Azure Metrics Explorer and metric alerts

- Monitoring health of the VM

	- Azure Monitor for monitoring metrics
	- Metrics (collected by default)

		- CPU usage
		- Network traffic
		- OS disk usage
		- Boot success

	- More metrics

		- Need Azure Diagnostics on the VM and Log Analytics agent, for Windows and Linux
		- (Needs a storage account)
		- Investigate boot issues with enhanced boot diagnostics
		- Archive logs and metrics for future analysis
		- Autoscale VMSSs
		- App-level metrics with application insights
		- Automate OS updates
		- Track VM config changes over time

	- Boot diagnostics

		- Create and associate storage account
		- View screenshot of boot sequence - and serial log
		- Without extensions

	- KPI dashboard

		- Monitoring more than one VM's performance

	- Azure Diagnostics

		- More detailed performance metrics for Windows and Linux machines
		- Installing Azure Diagnostics Extension
		- Moving logs to storage account, or other services, Azure monitor 
		- Having alerts, and then action, based on the metrics being captured

	- Diagnostics data case studies

		- DDoS attacks

			- Monitoring public IP with DDoS metrics

		- Increased CPU load

			- Basic or more granular metrics
			- Create alert rules
			- Adding actions based on rule

	- Config

		- Setting up VM with boot diagnostics

			- Creating VM and storage account
			- Viewing metrics and boot diagnostics

		- Installing and configuring Azure Diagnostics extension
		- Creating custom KPI dashboard
		- Creating alerts based on CPU usage
		- Triggering (testing) the alert
		- Observing the action

### Analysing infrastructure using Azure Monitor logs (to then query and evaluate)

- Features of Azure Monitor logs

	- Data collection (Extended by enabling diagnostics and adding agents)

		- Metrics

			- Useful for alerting
			- Stored in a time-series database
			- Represents aspect of a system at a point in time

		- Logs

			- Log in a log analytics workspace
			- Run queries
			- Changes made to resources
			- Analyse with Kusto

		- Queries

			- Extracting information about log data through queries
			- Log Analytics

		- Config

			- Creating basic Azure Monitor log queries to extract info from log data

### Monitor performance of VMs through Azure Monitor for VMs

- Azure Monitor

	- Log data stored in log analytics workspace
	- Azure Monitor for VMs

Provides additional information for data collection of VMs (without queries)

		- Relies on Azure Monitor Logs
		- Provides monitoring experience with little config required
		- Process data with and without queries
		- Metrics vs Logs

			- Metrics stored in a specific structure
			- Logs stored in different types

		- Platform logs

			- Sent out by Azure resources
			- Resource logs
			- Activity logs
			- Azure AD logs

		- Log Analytics workspace

			- Where the Azure monitor data is aggregated
			- Different access control modes

				- Access mode

					- Workspace-context
					- Resource-context

				- Access control mode

					- Require workspace permissions
					- Use resource/workspace permissions

				- Table-level RBAC

					- Workspace-context
					- Resource-context

			- Reducing number of workspaces you have

				- Making administration and query easier
				- Unless specific design requirement

		- Agents

			- Log Analytics agent
			- Azure diagnostics extension
			- Dependency agent

		- Kusto Query language

			- Log analytics workspace (used by different services) is equivalent of a database inside Azure Data Explorer
			- Log queries - understanding the data

				- Alert rules
				- Dashboards
				- Export
				- PowerShell
				- Azure Monitor logs API

			- Read-only
			- Databases, tables, and columns
			- Schema

				- Series of tables grouped together

		- Config

			- Create and configure a Log Analytics workspace
			- Set up environment
			- Onboarding VMs to Azure Monitor for VMs
			- Building queries (using the query pane) and editing queries

### Incident response with alerting in Azure Monitor

- Azure Monitor

	- Data types

		- Metric-based
		- Log-based

	- Signal types

		- Metric alerts
		- Activity log alerts
		- Log alerts

	- Alert rules

		- Resource

			- Target resource(s)

		- Condition

			- Signal type
			- Alert logic

		- Action(s)

			- May be comprised of action group

		- Alert details

			- Name
			- Description
			- Severity

	- Managing alert rules

		- Disabled/enabled as needed

	- Viewing alerts

		- Filtering as needed

	- Alert states

		- New
		- Acknowledged
		- Closed

	- Metric alerts

		- Monitoring specific resources
		- Based on a certain threshold
		- Static or dynamic
		- Dimensions, alert applied to multiple instances
		- Scaling, monitoring multple resources
		- Config

			- Creating VMs
			- Creating metric alerts
			- Viewing metric alerts

	- Log alerts

		- Data from any resource, historical, analytics and trends
		- Log search rule

			- Log query
			- Time period
			- Frequency
			- Threshold

		- Log search results

			- Number of records
			- Metric measurement

		- Stateless

	- Activity log alerts

		- Specific events happening to an Azure resource
		- Also including service health related information alerts
		- Config

			- Creating Azure activity log monitor
			- Adding email alert action
			- Doing something to trigger alert
			- Viewing activity log alerts

	- Smart groups

		- Automatic feature
		- Managing number of alerts
		- Reduces noise
		- ML algorithms, joining alerts together in groups

### Capture and view page load times in Azure web app with Application insights

- Application insights

	- Features

		- Monitoring performance and behaviour of web applications
		- Capturing data

			- Events
			- Metrics

		- Sending data from app to Application Insights

			- Runtime instrumentation (without changing source code)
			- Build-time instrumentation
			- Client-side instrumentation

	- Processes

		- Enabling Application Insights on a web app

### Protecting VMs with Azure Backup + protecting infrastructure with site recovery

- Azure Backup - part of a BCDR plan, data loss or disk corruption (or ransomware)

	- Benefits

		- Zero infrastructure backup
		- Long term retention
		- Security

			- RBAC
			- Encryption
			- No internet connectivity
			- Soft delete

		- High availability
		- Centralised monitoring + management

	- VM Snapshots and vault tiers
	- VM Backup policy
	- Processes

		- Enabling backups for VMs
		- Monitoring backups
		- Restoring VM data

			- Stopping VM as you do it
			- Restoring
			- Viewing all jobs to monitor it

- Site recovery - disaster recovery, replication, failover, failback, of VMs

	- Features

		- Replicating VM workloads between regions
		- Migrating VMs from on-prem to Azure
		- Snapshots and recovery points
		- Disaster recovery drills
		- Failover and failback

			- RPO and RTO

		- Reprotection

			- Replicating new target environment back to source environment where it started, upon failover

		- Azure Mobility Service

			- Installed on every VM that you replicate
			- Keeps cache of VM data

				- Replicated to target environment storage account

	- Processes

		- Adding recovery services vault
		- Organising target resources
		- Configuring outbound connectivity
		- Setting up replication on existing VMs
		- Running disaster recovery drills

			- Recovery plans
			- Drills and production failovers

		- Failover and failback

			- Can be potentially automated using Azure Automation runbook

### Microsoft Azure Well-Architected Framework - Reliability

- Handling infrastructure and service failure, loss of data, disaster recovery

	- Highly available architecture

		- Defining features

			- Handling loss or degradation of a component of a system

		- Evaluating HA

			- Determining SLA of application

				- May have SLOs for specific performance requests
				- How many nines do we need?

			- Evaluate HA capabilities of application

				- Failure analysis

					- Single points of failure
					- Critical components

			- Evaluate HA capabilities of dependent applications

				- SLAs of resources that resource depends on

		- Platform

			- Availability sets

				- Update domains
				- Fault domains

			- Availability zones
			- Load balancing

				- Traffic Manager
				- Application Gateway
				- Load Balancer

			- PaaS HA capabilities

	- Disaster recovery strategy

		- Disaster recovery plan

			- What to do and where to go when disaster strikes
			- Requires knowledge of application workflows, data, infrastructure, and dependencies
			- Risk analysis

				- Examine disasters

			- Risk assessment

				- Consider critical processes

			- Recovery objectives (established through investigation)

				- RPO
				- RTO

			- Recovery steps

				- Backups
				- Data replicas
				- Deployments
				- Infrastructure
				- Dependencies
				- Configuration and notification

			- Data recovery

				- Restoring lost data
				- Backups and replication

			- Process recovery

				- Recovering services and deploying code
				- Failover to a separate environment

			- Testing the disaster recovery plan

	- Backup and restore

		- Last line of defence against permanent data loss
		- Solutions

			- Azure Backup

				- Azure Backup Agent
				- System Center Data Protection Manager
				- Azure Backup Server
				- Azure IaaS VM backup

			- Azure Blob Storage
			- Azure SQL database
			- Azure App Service

		- Verify and test

