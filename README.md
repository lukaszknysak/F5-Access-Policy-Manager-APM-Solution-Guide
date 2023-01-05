# THIS IS UNDER CONSTRUCTION

# F5-Access-Policy-Manager-APM-Solution-Guide


# Welcome to the Access Policy Manager (APM) lab guide.

The following labs and exercises will instruct you on how to configure and troubleshoot various APM use cases. This guide is intended to complement lecture material provided during the course as well as a reference guide that can be referred to after the class as a basis for configuring Access solutions in your own environment.

[Original Lab Guide](https://github.com/f5devcentral/f5-agility-labs-iam)

# Table of Contents
1. [Example](#example)
2. [Example2](#example2)
3. [Third Example](#third-example)


## Example
## Example2
## Third Example




## Table of Contents
- [Class 1 – Access Policy Manager Solution (F5 APM)](#class-1)
  * [Module 1: APM GUI Overview](#module-1)

- [Class 2 – Access Policy Manager Solution (F5 APM)](#class-2)
  * [Module 1: API Security AWAF and APM modules](#module-1)

## Class 1
## Class 1 – Access Policy Manager Solution (F5 APM)
  * Module 1: APM GUI Overview
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

## Module 1
## Module 1 : APM GUI Overview
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

A per request policy creation will work the same way as a per session policy allowing you to add various items to the main policy and create macros. In addition a per request policy can also contain subroutines.

**Note**

`A per-request policy subroutine is a collection of actions. What distinguishes a subroutine from other collections of actions (such as macros), is that a subroutine starts a subsession that, for its duration, controls user access to specified resources. If a subroutine has an established subsession, subroutine execution is skipped. A subroutine is therefore useful for cases that require user interaction (such as a confirmation dialog or a step-up authentication), since it allows skipping that interaction in a subsequent access.`

You cannot use subroutines in macros within per-request policies. Subroutine properties specify subsession timeout values, maximum macro loop count, and gating criteria. You can reauthenticate, check for changes on the client, or take other actions based on timeouts or gating criteria.

**Note**

`A subsession starts when a subroutine runs and continues until reaching the maximum lifetime specified in the subroutine properties, or until the session terminates. A subsession populates subsession variables that are available for the duration of the subsession. Subsession variables and events that occur during a subsession are logged. Multiple subsessions can exist at the same time. The maximum number of subsessions allowed varies across platforms. The total number of subsessions is limited by the session limits in APM (128 * max sessions). Creating a subsession does not count against the license limit.`

5. If you click on the plus between Start and Allow a new box will appear and you can explore the various components that can be added. At this time we will leave the policy blank and return to populate it in later tasks.

6. Close the VPE tab when you are done exploring.

**Policy Sync**

1. Click on Access –> Profiles/Policies –> Policy sync

BIG-IP APM Policy Sync maintains access policies on multiple BIG-IP APM devices while adjusting appropriate settings for objects that are specific to device locations, such as network addresses. You can synchronize policies from one BIG-IP APM device to another BIG-IP APM device, or to multiple devices in a device group.

A sync-only device group configured for automatic and full sync is required to synchronize access policies between multiple devices.

**Important**

`USE WITH CAUTION. This is an advanced feature and you should consult with your F5 Account team or Professional Services before implementing this configuration.`

**Note**

`In BIG-IP 13.1.0, a maximum of eight BIG-IP APM systems are supported in a sync-only group type.`

**Customization**

1. Click on **Access** –> **Profiles/Policies** –> **Customization**

**What are customization and localization?**

Customization and localization are ways to change the text and the language that users see, and to change the appearance of the user interface that Access Policy Manager presents to client users. Customization provides numerous settings that let you adapt the interface to your particular operation. Localization allows you to use different languages in different countries.

**About the Customization tool**

The Customization tool is part of Access Policy Manager (APM). With the Customization tool, you can personalize screen messages and prompts, change screen layouts, colors, and images, and customize error messages and other messages using specific languages and text for policies and profiles developed in APM. You can customize settings in the Basic Customization view (fewer settings) or change the view to General Customization (many settings). In the General Customization view, you can use the Customization tool in the BIG-IP admin console, or click Popout to open it in a separate browser window. In either view, you can click Preview to see what an object (such as Logon page or Deny Ending Page) will look like.

After you personalize settings, remember to click the Save icon to apply your changes.

![image](https://user-images.githubusercontent.com/51786870/210539400-3cf37ab7-1a23-46d5-a4c0-7b27da61b84c.png)

2. About basic, general, and advanced customization

The Customization tool provides three views that you can use to customize the interface. The General Customization view provides the greatest number of options and is where most of the customization takes place.

![image](https://user-images.githubusercontent.com/51786870/210539479-a4314010-a928-492a-8ba4-3ceff3d45f85.png)

**Note**

`See the APM Customization guide for further details on customization`

3. Under Available Profiles choose the /Common/server1-psp

4. Select Language: **English**

5. Let’s upload a new image. Click **Upload New Image**

6. Browse to Desktop and locate the **Lab01_images** folder

7. Choose an image from the selection and click Open

8. Pick a Background color

9. Pick a Header Background color

10. Change the footer Text

11. Remember to click **Save** icon at the bottom

![image](https://user-images.githubusercontent.com/51786870/210540876-b73ab744-ef51-4a53-a5e1-5c45f14b10f1.png)

12. Click on the **Preview** button (At the top right)

![image](https://user-images.githubusercontent.com/51786870/210540902-ae10a959-50ef-49e5-9f91-3d1a1ab433c1.png)

13. Assign Access Profile **server1-psp** to **server1-https** Virtual Server under the settings in **Local Traffic** -> **Virtual Servers** -> **Virtual Server List** -> **server1-https**

![image](https://user-images.githubusercontent.com/51786870/210542491-f5591740-1dd2-474b-8119-033a090aaa00.png)

14. In the browser open new tab and go to https://10.1.10.101/. What is the result? Why you are blocked and don't see any logon pages?

![image](https://user-images.githubusercontent.com/51786870/210542937-0baf60e2-6c82-4992-914a-258193ca87da.png)

15. Choose **Access Profiles** –> **/Common/server1-psp** –> **Access Policy** –> **Edit Access Policy for Profile "server1-psp"** -> **Edit Endings**

Bonus Answer: Why don’t we see logon pages?

**Hint**

`What is in the policy so far?`


### Task 5: Authentication¶

BIG-IP APM serves as an authentication gateway or proxy. As an authentication proxy, BIG-IP APM provides separate client-side and server-side authentication. Client-side authentication occurs between the client and BIG-IP APM. Server-side authentication occurs between BIG-IP APM and servers.

Loose coupling between the client-side and server-side layers allows for a rich set of identity transformation services. Combined with a Visual Policy Editor and an expansive set of access iRules functionality, BIG-IP APM provides flexible and dynamic identity and access, based on a variety of contexts and conditions.

For example, a client accessing Microsoft SharePoint through BIG-IP APM in a corporate environment may silently authenticate to BIG-IP APM with NT LAN Manager (NTLM) or Kerberos credentials. On leaving that environment, or on using a different non-sanctioned device, the client may be required to go through another potentially stronger authentication, such as a smart card or other client certificate, RSA SecurID, or one-time passcode. You can require additional device vetting such as file, folder, and registry checks and antivirus and firewall software validation.

A BIG-IP APM authentication and SSO features access and identity security posture can automatically change depending on environmental factors, such as who or where the user is, what resource the user is accessing, or when or with what method the user is attempting to gain access.

Data centers and Cloud deployments often face the challenge of offering multiple applications with different authentication requirements. You can deploy BIG-IP APM to consolidate and enforce all client-side authentication into a single process. BIG-IP APM can also perform identity transformation on the server side to authenticate to server services using the best-supported methods. This can reduce operational costs since applications remain in the most-supported and documented configurations. Common examples of identity transformation are client-side public key infrastructure (PKI) certificate to server-side Kerberos and client-side HTTP form to server-side HTTP Basic.

The following figure shows BIG-IP APM acting as an authentication gateway. Information received during pre-authentication is transformed to authenticate to multiple enterprise applications with different requirements.

![image](https://user-images.githubusercontent.com/51786870/210547813-0aa0243f-ec6e-4270-bac2-b4811caffa78.png)

1. Client-side authentication

Client-side authentication involves the client (typically a user employing a browser) accessing a BIG-IP APM virtual server and presenting identity. This is called authentication, authorization, and accounting (AAA).

BIG-IP APM supports industry standard authentication methods, including:

  * NTLM
  * Kerberos
  * Security Assertion Markup Language (SAML)
  * Client certificate
  * RSA SecurID
  * One-time passcode
  * HTTP Basic
  * HTTP Form
  * OAuth 2.0
  * OpenId Connect

After access credentials are submitted, BIG-IP APM validates the listed methods with industry-standard mechanisms, including:

  * Active Directory authentication and query
  * LDAP and LDAPS authentication and query
  * Remote Authentication Dial-in User Service (RADIUS)
  * Terminal Access Controller Access Control System (TACACS)
  * Online Certificate Status Protocol (OCSP) and Certificate Revocation List Distribution Point (CRLDP) (for client certificates)
  * Local User Database authentication

2. Go to Access –> Authentication –> Active Directory

3. Click on basic-ad-servers and review the settings. You can choose to use go direct or use a pool of AD servers.

| General Properties	| Name	| basic-ad-servers |
| ------------------ | ---- | ---------------- |
| Configuration	| Domain Name	| f5lab.local |
| |  	Server Connection	| Use Pool |
| |  	Domain Controller Pool Name	| /Common/basic-ad-pool |
| |  	IP Address	| 10.1.20.7 |
| |  	Hostname	| dc1.f5lab.local |
| |  	Admin Name	| admin |
| |  	Admin Password	| admin |

**Note**

`If you choose to use a pool you can create the pool as you create the AD object. You can also choose to use Direct which allows you to only use one server. Go back and click create to see what this looks like.`

![image](https://user-images.githubusercontent.com/51786870/210548467-a1f1d2c5-d10f-4ded-8a55-7e11b17dd6b4.png)

You now have an object that can be used to facilitate Active Directory authentication in front of any application. The application itself does not need to require authentication. If you were to deploy a policy with AD Auth on a Virtual Server for a web application the policy would preset a login page, prompt for credentials, verify the credentials against this AD object before allowing a user to access the web application.

4. Go to **Access** –> **Profiles/Policies** –> **Access Profiles (Per-Session Policies)**

5. Locate the **server1-psp** and click **Edit**

6. Click the **+** symbol between Start and Deny.

7. From the **Logon** tab select the **Logon Page** radio button

8. Click **Add Item**

9. Notice that you can add fields and change the names of the fields. Click **Save**

10. Click the **+** between **Logon Page** and Deny

11. Click the **Authentication** tab

12. Choose the **AD Auth** radio button and click **Add Item**

![image](https://user-images.githubusercontent.com/51786870/210550024-6cc036e0-7b45-4c36-810a-2dd67a1470a7.png)

13. Under the **Server** field click on the drop down menu and choose the AAA server **basic-ad-servers**

14. Click **Save**

15. On the Success branch click on the **Deny** end point and choose **Allow** then click **Save**

16. Click **Apply Access Policy**

![image](https://user-images.githubusercontent.com/51786870/210548967-25c184ca-872e-4e5f-bddc-818e9f664f56.png)

Now you have a basic policy with AD Authentication that you can leverage for Web Pre-Authorization in front of any application.

17. Go to **Local Traffic** –> **Virtual Servers**

18. Locate **server1-https** and click on it

19. Scroll down to the **Access Policy** section. Next to **Access Profile** click the drop and chose server1-psp

20. Scroll down to the bottom and click **Update**

21. In a new browser tab go to http://server1.acme.com and Login

| username |	user1 |
| -------- | ----- |
| password	| user1 |

22. After correct login attempt you should see ACME backend app.

![image](https://user-images.githubusercontent.com/51786870/210550391-c9ef10ec-b2bb-46a4-b895-3e5e87f305b9.png)

23. Review Access Session logs and reports.

### Task 6: Single Sign-On
**Note**
`All the objects used to demonstrate Server-side Single Sign-On have already been created. The next steps will walk you through what the configuration looks like.`

Client side and server side are loosely coupled in the authentication proxy. Because of this, BIG-IP APM can transform client-side identity values of one type into server-side identity values of another type. You configure SSO within an SSO profile, which is applied to an access profile. The system triggers SSO at the end of successful access policy evaluation and on subsequent client-side requests.

BIG-IP APM supports industry standard authentication methods, including:

  * NTLM
  * Kerberos
  * HTTP Basic
  * HTTP Form
  * Security Assertion Markup Language (SAML)

**Note**

`Client-side authentication methods outnumber server-side methods. This is because BIG-IP APM does not transmit client certificate, RSA SecurID, or one-time passcodes to the server on the client’s behalf.`

1. Go to **Access** –> **Single Sign-On** –> **HTTP Basic**

2. Click **basic-sso**

| General Properties	| Name	| basic-sso | 
| --- | --- | --- |
| Credential Source	| Username Source	| session.sso.token.last.username | 
| |  	Password Source | session.sso.token.last.password | 
| SSO Method Conversion	| Username Conversion	unchecked | 

**Note**

`Username conversion can be enabled if you want domain\username or username@domain to convert to just username.`

3. Click on **Access** –> **Profiles/Policies** –> **Access Profiles (Per-Session Policies)**

4. Locate the **basic-psp** profile and click on the name

5. Click on **SSO/Auth Domains**

6. Under SSO Configuration notice **basic_sso** is selected

7. From the top menu bar click **Access Policy** and click **Edit Access Policy for Profile “basic-psp”** link

![image](https://user-images.githubusercontent.com/51786870/210554675-9de69197-44a9-4e8e-8236-3333f23c3ddc.png)

8. Click on **SSO Credential Mapping**

![image](https://user-images.githubusercontent.com/51786870/210554731-1b1ba4a7-f563-4f05-ae47-ba14b14d9fe4.png)

**Note**

`You can modify these options based on the variables collected in the user’s session. In this case we accept the defaults.`

9. Open an incognito window and try go to https://basic.acme.com

10. You should have been prompted with a windows login. Close the Window

11. Go to **Local Traffic –> Virtual Servers** and open **basic-https**

12. Scroll to **Access Policy** and click the drop down next to **Access Profile**. Choose **basic-psp**

![image](https://user-images.githubusercontent.com/51786870/210554995-61b84651-4f77-4050-8876-395b667c17e9.png)

13. Scroll down click **Update**

14. Open a new incognito tab. Go to https://basic.acme.com

15. Login **user1** and **user1**

16. Now you should have been signed in to the backend server with Single Sign On.


### Task 7: Federation
**Note**

`In this task we will examine SAML IDP and SP configuration. All the configuration has been completed the next several steps you will just be examining the objects and testing the configuraiton. For more in depth instruction on Federatoion consider taking a 300 series course.`

**BIG-IP APM federation with SAML**

BIG-IP APM supports SAML 2.0 and can act as the IdP for popular SPs, such as Microsoft Office 365 and Salesforce. The system supports both IdP- and SP-initiated identity federation deployments.

**IdP-initiated federation with BIG-IP APM**

![image](https://user-images.githubusercontent.com/51786870/210555795-bd466fe3-0a9f-4508-b569-b60a48d0a0f6.png)

  * The user logs in to the BIG-IP APM IdP and the system directs them to the BIG-IP APM webtop.
  * The user selects the SP they want, such as Salesforce.
  * The system retrieves any required attributes from the user data store to pass on to the SP.
  * The system uses the browser to direct the request to the SP, along with the SAML assertion and any required attributes.

1. In a new tab go to https://idp.acme.com

2. Login to the SAML IdP

| username |	user1 |
| -------- | ----- |
| password	| user1 |

![image](https://user-images.githubusercontent.com/51786870/210556032-c5ff38bf-29f0-4eab-9742-33af6f17ee6b.png)

3. You are logged in to a webtop where a SAML SP object resides. Click on the SAML Resource sp.acme.com

![image](https://user-images.githubusercontent.com/51786870/210556084-9a154b75-d1d2-4f0b-9ae1-7dabab519f2d.png)

4. Since you authenticated through the SAML IdP you will not be prompted for authentication again and are connected to the SAML SP resource.

![image](https://user-images.githubusercontent.com/51786870/210556141-b0ba0804-dd30-4250-9b5c-9f17a4a0724a.png)

5. Return to bigip1.f5lab.local. From the left menu click **Access** –> **Profiles/Policies** –> **Access Profiles (Per-Session Policies)**

6. Locate the policy **idp-psp** and click on **Edit**

![image](https://user-images.githubusercontent.com/51786870/210556226-32726380-0751-4f40-8ddb-8559dd4fb402.png)

7. Click AD Auth* object within the Policy. Examine the settings

![image](https://user-images.githubusercontent.com/51786870/210556272-ba2b0693-d9d1-4f86-8cfc-ade986472f35.png)

**Note**

`If you look at the AAA server under Active directory you will find the idp-ad-server object. We are leveraging Active Directory as the credential verification but BIG-IP is acting as a SAML Identity Provider. BIG-IP will verify the credentials against Active Directory and create a SAML Assertion for the user requesting access. That assertion can then be used by the SAML Service Provider to provide access to the SAML SP resource.`

![image](https://user-images.githubusercontent.com/51786870/210556335-9e240127-af9f-4ee5-a650-ca3256d5a602.png)

8. Click Advanced Resource Assign. Examine the settings

![image](https://user-images.githubusercontent.com/51786870/210556566-be2bbb3d-a7a4-4a0b-98c5-878dc140c7ee.png)

**Note**

`You can click on the Add/Delete button and add other SAML Resources (if available). We will cover more on Webtop in next labs.`

9. Return to the BIG-IP click on **Access** –> **Federation** –> **SAML Identity Provider**

![image](https://user-images.githubusercontent.com/51786870/210556735-f9e96cf6-d9ef-4802-bd80-2251da864d8c.png)

In order for the BIG-IP to be configured as a SAML IdP you must define the Identity provider and bind it with a SAML Service Provider. This object contains the settings required to configure BIG-IP as a SAML SP. For more information on SAML and uses with BIG-IP consider taking the Federation lab.

**Note**

`You can export the Metadata of the SAML IdP in this menu by clicking the SAML IdP and clicking the Export Metadata button. It will output an XML file that you can use to upload in to a SAML Service Provider with all the IdP setting particular to this IdP.`

**SP-initiated federation with BIG-IP APM**

![image](https://user-images.githubusercontent.com/51786870/210556814-5cd1e865-71a0-4d61-80e2-cd7fcd6675b1.png)

  * The user logs in to the SP, such as Salesforce.
  * The SP uses the browser to redirect the user back to the BIG-IP APM IdP.
  * The BIG-IP APM IdP prompts the user to log in.
  * The system retrieves any required attributes from the user data store to pass on to the SP.
  * The system uses the browser to send the SAML assertion and any required attributes to the SP.

1. Open a new incognito window and go to https://sp.acme.com

2. Notice that you get redirected to https://idp.acme.com for authentication

![image](https://user-images.githubusercontent.com/51786870/210557082-9dee8935-710d-432f-8a55-6e5554697129.png)

| username |	user1 |
| -------- | ----- |
| password	| user1 |


3. Once logged in you arrive at https://sp.acme.com

![image](https://user-images.githubusercontent.com/51786870/210557211-7db70918-3953-4738-b4d8-d0a910aaff6a.png)

4. Return to the BIG-IP. From the left menu navigate to **Access** –> **Profiles/Policies** –> **Access Profiles (Per-Session Policies)**

5. Locate the sp-psp profile and cick **Edit**

![image](https://user-images.githubusercontent.com/51786870/210557320-4bd105f3-74b9-4770-b5e0-3715e27a8fe3.png)

![image](https://user-images.githubusercontent.com/51786870/210557338-4d099f79-c5f9-4aca-bf58-bee02db2b18d.png)

6. Return to the BIG-IP and navigate to Access –> Federation –> SAML Service Provider

![image](https://user-images.githubusercontent.com/51786870/210557385-60235425-b4f6-4238-8fa2-d66df32febf6.png)

The SAML SP object contains information about the SAML SP object and the binding to the SAML Identity Provider. You can see on the screen that we have a Service Provider object defined and it is bound to a SAML Identity Provider. The configuration of these objects is covered in more detail in the Access Federation labs.


### Task 8: Connectivity/VPN

**Note**
`In interest of time the VPN configuration has already been completed. The next several steps will be observing what the configuration looks like and testing out the connectivity.`

**Policy Walk-Through**

1. Navigate to **Access** –> **Profiles/Policies** –> **Access Profiles (Per-Session Policies)**

2. Locate profile **vpn-psp** and click on **Edit**. This opens the Visual Policy Editor (VPE) and we can take a look at the policy

![image](https://user-images.githubusercontent.com/51786870/210550760-f9b96cc7-b8a2-40ac-8986-61a2a718f74b.png)

3. A user enters their credentials into the logon page agent. - Those credentials are collected, stored as the default system session variables of session.logon.last.username and session.logon.last.password.

4. The AD Auth Agent validates the username and password session variables against the configured AD Domain Controller.

5. The user is assigned resources defined in the Advanced Resource Assign Agent

6. The user is granted access via the Allow Terminal

7. If unsuccessful, the user proceeds down the fallback branch and denied access via the Deny Terminal

**Policy Agent Configuration**

The Logon Page contains only the default setting

![image](https://user-images.githubusercontent.com/51786870/210551123-93409251-f669-4968-8cad-0a9713b1a15b.png)

The AD Auth agent defines the AAA AD Servers that a user will be authenticated against. All Setting are the default.

![image](https://user-images.githubusercontent.com/51786870/210551170-dcf3f56b-0f43-4d2c-ac77-965cfe2fd695.png)

The Advanced Resource Assign agent grants a user access to the assigned resources.

![image](https://user-images.githubusercontent.com/51786870/210551204-192d0cce-1456-490d-8eff-b43ab12a9886.png)

**Supporting APM Objects**

**Network Access Resource**

Navigate to **Access** –> **Connectivity/VPN** –> **Network Access (VPN)** –> **Network Access Lists**

Click the **vpn** Network Access Profile

The Properties page contains the Caption name **VPN**. This is the name displayed to a user.

![image](https://user-images.githubusercontent.com/51786870/210551405-e3b04d05-6705-4e71-ab66-a67cc9246bc7.png)

  * The Network Settings tab assigns the **lease pool** of ip addresses that will be used for the VPN.

  * Split Tunneling is configured to permit only the **10.1.20.0/24** subnet range inside the VPN.

![image](https://user-images.githubusercontent.com/51786870/210551538-c382bdf1-777e-4a0f-8c8f-c219a7594524.png)


**Lease Pool**

Navigate to **Access** –> **Connectivity/VPN** –> **Network Access (VPN)** –> **IPV4 Lease Pools**

Click **vpn-vpn_pool** lease pool object

A single address of **10.1.20.254** is assigned inside the lease pool.

![image](https://user-images.githubusercontent.com/51786870/210551633-cb35491e-97f2-4db3-8e19-9a7edc5f1e67.png)


**Webtop Sections**

Navigate to **Access** –> **Webtops** –> **Webtop Sections**

Click on **vpn-network_access**

A single section is configured to display a custom name.

![image](https://user-images.githubusercontent.com/51786870/210551708-819923ca-6798-4011-a9ec-7daea88244c1.png)

**Webtop Lists**

Navigate to **Access** –> **Webtops** –> **Webtop Lists**

Click on **vpn-webtop**

  * A Full Webtop was defined with modified default settings.
  * The Minimize to Tray box is checked to ensure the Webtop is not displayed when a user connects to the VPN.

![image](https://user-images.githubusercontent.com/51786870/210551805-67267735-d738-44b7-ace5-7784e2ada704.png)


**The Policy from a user’s perspective**

1. The connects to https://vpn.acme.com with the following credentials

| username |	user1 |
| -------- | ----- |
| password	| user1 |

![image](https://user-images.githubusercontent.com/51786870/210551919-3baabc96-a52b-4cbb-a3fc-dd3916d3e999.png)

2. Once authenticated the user is presented a Webtop with a single VPN icon.

![image](https://user-images.githubusercontent.com/51786870/210551961-94f08ac6-6258-48be-a4e9-1836a312f0c0.png)

3. Assuming the VPN has already been installed the user is notified that the client is attempting to start

![image](https://user-images.githubusercontent.com/51786870/210551999-b5983f5e-6884-4edc-9459-8592e46e2e90.png)

**Note**

`You may be prompted to download the VPN update. This is what a user will experience if you have auto-update enabled in the VPN Connectivity Profile. Click Download and wait for the components to update.`

4. A popup opens displaying the status of the VPN connection. The status will eventually become Connected

![image](https://user-images.githubusercontent.com/51786870/210552072-18ad8b64-f573-4274-ac2e-20b2dbf25fcb.png)

**Note**

`If you lose the pop-up check the system tray for the little red ball. Right click and choose restore`

5. Click **Disconnect**

**Note**

`For more information on API Protection consider taking the API Protection lab. For more information on SWG, ACL and Webtops see the appendix or further APM labs.`

### Task 9: Lab Cleanup

1. Open a new tab and click on the Access: PORTAL bookmark then select **CLASSES**

2. Locate the **APM GUI Overview** Tile and click on the **Stop** button

![image](https://user-images.githubusercontent.com/51786870/210558784-6de5fe68-f5f3-46ed-9401-a16194bf0271.png)
![image](https://user-images.githubusercontent.com/51786870/210558798-88c9bb09-18d9-41a0-b1bf-0dfb934bc100.png)

3. Wait about 30 seconds for the processing to begin

![image](https://user-images.githubusercontent.com/51786870/210558838-4479bda4-bb15-4359-8486-8f5ff23da8ca.png)

4. This process will take up to 30 seconds. Scroll to the bottom of the script and verify no issues.

**Module 1 is now complete.**

# Module 2: Building a Basic Access Policy

The environment is the same, but you will have to build the configuration with Postman/Newman scrits automating the configuration.

1. Click the **Classes** tab at the top of the page.

![image](https://user-images.githubusercontent.com/51786870/210562696-d4abb398-8925-481a-ab91-4ee8fc838b3d.png)

2. Scroll down the page until you see **102 Access Building Blocks** on the left

![image](https://user-images.githubusercontent.com/51786870/210562840-570ec080-181d-408d-b34f-a4d6ebf1d4e2.png)

3. Hover over tile Building a Basic Acces Policy. A start and stop icon should appear within the tile. Click the Play Button to start the automation to build the environment

![image](https://user-images.githubusercontent.com/51786870/210562912-fcf67ca5-f137-4048-8d6a-783301296a3b.png)
![image](https://user-images.githubusercontent.com/51786870/210562922-fd5311bd-2e53-4e64-8cba-ca7e8ecaf90e.png)

4. After the click it may take up to 30 seconds before you see processing

![image](https://user-images.githubusercontent.com/51786870/210562951-93f539b0-925f-44f7-b204-4cb986545032.png)

5. Scroll to the bottom of the automation workflow to ensure all requests succeeded.

![image](https://user-images.githubusercontent.com/51786870/210563054-c80e3a78-7fe2-4607-aecf-bc3d20d86615.png)

# Intro to Access Profiles and Policies

Access Policy Manager (APM) provides two types of policies.

**Per-session policy**

The per-session policy runs when a client initiates a session. (A per-session policy is also known as an access policy.) Depending on the actions you include in the access policy, it can authenticate the user and perform other actions that populate session variables with data for use throughout the session.
Per-request policy

After a session starts, a per-request policy runs each time the client makes an HTTP or HTTPS request. Because of this behavior, a per-request policy is particularly useful in the context of a Zero Trust scenario, where the client requires re-verification on every request. A per-request policy can include a subroutine, which starts a subsession. Multiple subsessions can exist at one time. You cannot use subroutines in macros within per-request policies.

You can associate one access policy and one per-request policy with a virtual server.

**Access Session**

An access session is recorded when a client initiates a connection through a per-session policy. Once an access session is established it has a set of timeouts set within the Access profile. A session will terminate if it reaches a timeout or the client ends the session. An access session is now not limited by a license but by the platform running APM. For more information on APM licensing see K15624537: BIG-IP APM Licensing for BIG-IP Standard Platforms

**Subsession**

A subsession is part of the per-request policy framework. It starts when a subroutine (within a per-request policy) runs and continues until reaching the maximum lifetime specified in the subroutine properties, or until the session terminates. A subsession populates subsession variables that are available for the duration of the subsession. Subsession variables and events that occur during a subsession are logged.

Multiple subsessions can exist at the same time. The maximum number of subsessions allowed varies across platforms. The total number of subsessions is limited by the session limits in APM (128 * max sessions). Creating a subsession does not count against the license limit.

# Objectives

The lab has a pre-configured test Virtual Server which will be used throughout the lab. You will use the Visual Policy Editor (VPE) to create and attach a simple Access Profile which performs user authentication.

# Lab Requirements
   * A pre existing virtual server at 10.1.10.101 or https://app.acme.com


### Task 1: Define an Authentication Server

Before we can create an access profile, we must create the necessary AAA server profile for our Active Directory.

1. Click the bigip1 bookmark from within Chrome and login to the BIG-IP, admin/admin

2. From the main screen, browse to Access > Authentication > Active Directory

3. Click **Create** in the upper right-hand corner

4. Configure the new server profile as follows:

| Name:	| lab_sso_ad_server |
| ---- | ---- | 
| Domain Name:	| f5lab.local | 
| Server Connection:	| Direct | 
| Domain Controller:	| 10.1.20.7 | 
| User Name:	| admin | 
| Password:	| admin | 

![image](https://user-images.githubusercontent.com/51786870/210564908-e637c298-a7ed-4563-b10b-2f28eadb89f4.png)

5. Click **Finished**

**Note**

`If you wish you can simply use the **app-ad-servers**.`

### Task 2: Create a Simple Access Profile¶

1. Navigate to **Access** > **Profiles / Policies** > **Access Profiles (Per-Session Policies)**

![image](https://user-images.githubusercontent.com/51786870/210564483-e2eeba11-fa37-4b58-9655-9bfa5b6e1d42.png)

2. From the Access Profiles screen, click **Create…** in the upper right-hand corner

3. In the Name field, enter **MyAccessPolicy** and for the **Profile Type**, select the dropdown and choose **All**

![image](https://user-images.githubusercontent.com/51786870/210565408-3a4a23b0-df7d-4300-84f7-89d27ed7ffb0.png)

4. Under “**Language Settings**”, choose **English** and click the **<<** button to slide over to the **Accepted Languages** column.

![image](https://user-images.githubusercontent.com/51786870/210565199-0a7ae2d8-3317-4dfe-86cb-db1a578c7f54.png)

5. Click **Finished**, which will bring you back to the Access Profiles screen.

6. On the Access Profiles screen, click the **Edit** link under the Per-Session Policy column.

![image](https://user-images.githubusercontent.com/51786870/210565614-a3e99a8d-dfd9-4da3-86fa-483e9bd322e3.png)

The Visual Policy Editor (VPE) will open in a new tab.

7. On the VPE page, click the + icon on the fallback path, to the right of the Start object.

![image](https://user-images.githubusercontent.com/51786870/210565703-36e1289b-9dbc-420c-b201-f94107173b2f.png)

8. On the popup menu, choose the **Logon Page** radio button under the Logon tab and click **Add Item**

![image](https://user-images.githubusercontent.com/51786870/210565923-42663a07-b969-4dbe-bb8e-a8cb0586e233.png)

![image](https://user-images.githubusercontent.com/51786870/210565937-e68bd31c-051e-4cfe-b7f9-688228c91868.png)


9. Accept the defaults and click **Save**

Now let’s authenticate the client using the credentials to be provided via the **Logon Page** object.

10. Between the **Logon Page** and **Deny** objects, click the **+** icon, select **AD Auth** found under the **Authentication** tab, and click the **Add Item** button

![image](https://user-images.githubusercontent.com/51786870/210566081-4d888884-f513-4b91-85e3-c25adf591538.png)

![image](https://user-images.githubusercontent.com/51786870/210566107-a3567840-082c-4a9a-bdcc-5b2c5cdbb623.png)

11. Accept the default for the **Name** and in the **Server** drop-down menu select the AD server created above: **/Common/lab_sso_ad_server**, then click **Save**

![image](https://user-images.githubusercontent.com/51786870/210571218-5b44711d-437c-430a-902c-fe345eef4c45.png)

12. On the **Successful** branch between the **AD Auth** and **Deny** objects, click on the word **Deny** to change the ending

![image](https://user-images.githubusercontent.com/51786870/210566315-a05be84b-80e8-4f78-b120-c4da200e9e1a.png)

13. Change the **Successful** branch ending to **Allow**, then click **Save**

![image](https://user-images.githubusercontent.com/51786870/210566389-9f6b1c59-b665-4033-b175-7b22cf30d0c0.png)

![image](https://user-images.githubusercontent.com/51786870/210566409-500a0192-6e06-4559-90f9-941e7b7743fa.png)

14. In the upper left-hand corner of the screen, click on the **Apply Access Policy** link, then close the window using the **Close** button in the upper right-hand. Click **Yes** when asked **Do you want to close this tab**?

![image](https://user-images.githubusercontent.com/51786870/210571383-f8da0cbd-1a6a-48c8-9d63-c8328571b62b.png)

![image](https://user-images.githubusercontent.com/51786870/210571444-8e9f340b-9c18-4820-b6a1-f18d42a43f66.png)

### Task 3: Associate Access Policy to Virtual Servers¶

Now that we have created an access policy, we must apply it to the appropriate virtual server to be able to use it.

1. Navigate to **Local Traffic** –> **Virtual Servers** –> **Virtual Server List** and click the name of the virtual server created previously: **app-https**

2. Scroll down to the **Access Policy** section, for the **Access Profile** dropdown, select **MyAccessPolicy**

![image](https://user-images.githubusercontent.com/51786870/210571800-ffa24455-ef09-45f8-af33-756d6c221ece.png)

3. Click **Update** at the bottom of the screen

### Task 4: Testing

Now you are ready to test.

1. Open a new browser window and open the URL for the virtual server that has the access policy applied:

**https://app.acme.com**

You will be presented with a login window

![image](https://user-images.githubusercontent.com/51786870/210572167-9d796471-2c01-4000-90ae-f629fbe21272.png)

2. Enter the following credentials and click **Logon**:

| username |	user1 |
| -------- | ----- |
| password	| user1 |

You will see a screen similar to the following:

![image](https://user-images.githubusercontent.com/51786870/210572287-b3ed94ef-34ac-4d67-a35d-a24998d2c868.png)

### Task 5: Troubleshooting tips

You can view active sessions by navigating Access/Overview/Active Sessions

You will see a screen similar to the following:

Click on the session id for the active session. If the session is active it will show up as a green in the status.

![image](https://user-images.githubusercontent.com/51786870/210572810-57686f25-a88e-416b-8df5-5e3965acbbfb.png)

Click on the “**session ID**” next to the active session. Note every session has a unique session id. Associated with it. This can be used for troubleshooting specific authentication problem.

Once you click on the **session id** you will be presented with a screen that is similar to the following.

![image](https://user-images.githubusercontent.com/51786870/210572975-8e442137-9024-4708-b9f3-ff384bdbd0cc.png)

Note that the screen will show all of the log messages associated with the session. This becomes useful if there is a problem authenticating users.

The default log level shows limited “informational” messages but you can enable debug logging in the event that you need to increase the verbosity of the logging on the APM policy. Note you should always turn off debug logging when you are finished with trouble shooting as debug level logging can generate a lot of messages that will fill up log files and could lead to disk issues in the event that logging is set to log to the local Big-IP.

Please review the following support article that details how to enable debug logging.

https://support.f5.com/csp/article/K45423041

### Step up Authentication with Per-Request Policies

# Objectives
The purpose of this lab is to familiarize the Student with Per Request Policies. The F5 Access Policy Manager (APM) provides two types of policies.

Access Policy - The access policy runs when a client initiates a session. Depending on the actions you include in the access policy, it can authenticate the user and perform group or class queries to populate session variables with data for use throughout the session. We created one of these in the prior lab

Per-Request Policy - After a session starts, a per-request policy runs each time the client makes an HTTP or HTTPS request. A per-request policy can include a subroutine, which starts a sub-session. Multiple sub-sessions can exist at one time. One access policy and one per-request are specified within a virtual server.

**It’s important to note that APM first executes a per-session policy when a client attempts to connect to a resource. After the session starts then a per-request policy runs on each HTTP/HTTPS request. Per-Request policies can be utilized in a number of different scenarios; however, in the interest of time this lab will only demonstrate one method of leveraging Per-Request policies for controlling access to specific URI’s and submitting information from Active Directory as a header to the application.**

# Objective:
   * Gain an understanding of Per Request policies
   * Gain an understanding of use for Per Request Policy

# Lab Requirements:
All lab requirements will be noted in the tasks that follow
   * Estimated completion time: 15 minutes

### TASK 1: Create Per Session Policy

Refer to the instructions and screen shots below:

1. Login to your lab provided **Virtual Edition BIG-IP**

   * On your jumphost launch Chrome and click the bigip1 link from the app shortcut menu
   * Login with credentials admin/admin

2. Begin by selecting: **Access** -> **Profiles/Policies** -> **Per Session Policies** ->

3. Click the **+** Sign next to **Access Profiles (Per-Session Policies)**

![image](https://user-images.githubusercontent.com/51786870/210573911-913ddc97-f83d-41cd-8ef1-7d82a3457ed3.png)

4. Enter the name of the policy, profile type, and profile scope

| Name:	| app.acme.com-PSP | 
| ---- | ---- | 
| Profile Type:	| All | 
| Profile Scope:	| Profile | 
| Accept Languages:	| English (en) | 

**Note**

`You will need a per session policy and a per request policy but we will be leaving the per session policy blank and performing our auth in per Request`

![image](https://user-images.githubusercontent.com/51786870/210574249-47c7a796-279d-408e-8749-19690f6ca443.png)

5. On the app.acme.com-PSP policy click **Edit**

6. Click on the **Deny** and change the Select Ending to **Allow**

![image](https://user-images.githubusercontent.com/51786870/210574623-8172368d-4f00-44d4-9131-1d26d611fe7e.png)

![image](https://user-images.githubusercontent.com/51786870/210574648-be310436-d8c5-4359-9a93-a54180c318b4.png)

7. Click **Save**

8. Click **Apply policy**

**Note**

`Nothing will be set in this policy we will simply establish a session and manage all the authentication in the Per-Request Policy`

9. Close Visual Policy Editor

### Task 2: Step Up Authentication with Per Request Policies

Step-up authentication can be used to protect layers or parts of a web application that manage more sensitive data. It can be used to increase protection by requiring stronger authentication within an already authenticated access to the web application. Step-up authentication can be a part of using the portal access or web application management (reverse proxy) features of Access Policy Manager.

In this example we’re going to use a Per-Request Policy with a subroutine to authenticate user when they access a specific URI, extract information from Active Directory and submit that information as a header

1. Begin by selecting: **Access** -> **Profiles/Policies** -> **Per Request Policies** ->

2. Click the **Create** button (far right)

![image](https://user-images.githubusercontent.com/51786870/210574902-7244424c-c305-4c86-9147-f61d28bc3c22.png)

3. Give the policy a name and select the Language Settings

 | Name:	| app.acme.com-PRP | 
 | ---- | ---- | 
| Accept Languages:	| English (en) | 

![image](https://user-images.githubusercontent.com/51786870/210575055-1dd31315-6ea7-46e7-b554-f87670b8b65a.png)

4. Click **Finished**

5. Back in the **Access** –> **Profiles/Policies** –> **Per-Request Policies** screen locate **app.acme.com-PRP** policy you just created.

6. Click **Edit** to the right of the name

7. Click on **Add New Subroutine**

![image](https://user-images.githubusercontent.com/51786870/210576808-09992a12-5f26-4191-9dde-de18579656cf.png)

8. Give it a name of **AD_Subrutine** and Click Save

![image](https://user-images.githubusercontent.com/51786870/210577246-6e712601-572d-427f-a55b-d3b301f24b06.png)

When you edit created subrutine once again click **Subrutine Settings / Rename**

![image](https://user-images.githubusercontent.com/51786870/210577257-a6b1f4a6-7fba-4d77-baaa-d12839d6ad55.png)

9. Click the **+** between In and Out In the subroutine

10. Click the **Logon** Tab

11. At the middle of the list choose **Logon Page** and click **Add Item**

12. Select **Save** at the bottom of the Logon Page dialog box

13. In the subroutine, between the Logon page and the green **out** terminal click the **+** and select the **Logon Tab** and click the **Logon Page** radio button

![image](https://user-images.githubusercontent.com/51786870/210577979-906a55b9-948b-4201-bf97-1fe479100fd5.png)

![image](https://user-images.githubusercontent.com/51786870/210577998-122075ce-6e32-4e29-a467-1b373194f094.png)

14. Click the + sign between Logon Page and Out and select the **Authentication** tab and click the **AD Auth** radio Button

![image](https://user-images.githubusercontent.com/51786870/210578140-875b110d-2403-4172-8994-9d526b51cb28.png)

15. Select AD Auth and click **Add Item** at the bottom

![image](https://user-images.githubusercontent.com/51786870/210578228-4656d6d4-31ba-4599-9808-8d8870472f8b.png)

16. Give the item a name of **AD_Auth**

17. Select **/Common/lab_sso_sd_server** for the Server option

**Note**

`The lab_sso_ad_server object was created in previous lab`

18. Click the Save

![image](https://user-images.githubusercontent.com/51786870/210578483-55412794-fa8a-4218-940b-83944b0485b8.png)

19. Between **AD Auth** and the Out endpoint click the + Sign

![image](https://user-images.githubusercontent.com/51786870/210578566-974f58d7-3bfe-477b-945d-32ddd64f3cdb.png)

20. Select Authentication and Select the **AD Query** radio button and click Add Item

21. Change the Server option to /Common/lab_sso_ad_server and click Save

![image](https://user-images.githubusercontent.com/51786870/210579017-b58020df-2d99-4bf0-87f9-de56c6264b05.png)

22. Between **AD Query** and the Out endpoint click the + Sign

![image](https://user-images.githubusercontent.com/51786870/210579107-b72199e4-59b4-4b8a-a835-a020bbcb88e3.png)

23. Navigate to the **Assignment** tab and select **Variable Assign** and click **Add Item**

24. Under Variable Assign click **Add New Entry**

![image](https://user-images.githubusercontent.com/51786870/210579235-6e26d4f7-edd2-43d8-ab0e-4a5d6e4d6391.png)

25. Next to “Empty” click the **Change** link

26. Change the drop down on the right hand side to **Session Varaible** and input the following value **subsession.ad.last.attr.memberOf**

27. In the left hand box type the following value **session.adgroups.custom** and then click **Finished** and **Save**

![image](https://user-images.githubusercontent.com/51786870/210580020-d65feddf-e3a5-4016-ac35-d73eef30432e.png)

![image](https://user-images.githubusercontent.com/51786870/210580037-eb63b99e-0a92-4a8e-98b5-3e2a3c5134bf.png)

28. Click the + sign between Start and Allow directly under the Per Request Policy at the top of the page

![image](https://user-images.githubusercontent.com/51786870/210580088-41110908-1a27-412c-8a09-50fd00bf2c6b.png)

29. Select the **Classification** tab, click the **URL Branching Radio Button** and click **Add Item**

![image](https://user-images.githubusercontent.com/51786870/210580274-77359403-6b78-4121-ab45-6400fbbc1500.png)

30. Click the **Branch Rules** tab and then click the **change** hyperlink

![image](https://user-images.githubusercontent.com/51786870/210580357-b2b29bb9-2e43-4bbd-b74c-cf06149f6b8a.png)

31. Change the value domain.com to **app.acme.com/apps/app1/** and click finished

![image](https://user-images.githubusercontent.com/51786870/210580544-9e0411fd-8b5f-4333-a8aa-cedbf1d79448.png)

![image](https://user-images.githubusercontent.com/51786870/210580562-65c1acc4-5150-4ff3-94f6-d9ef21e0808f.png)

32. Change the name from **Allow** to **/apps/app1/** and then click **Save**

![image](https://user-images.githubusercontent.com/51786870/210580668-9945b8de-96e5-4133-a24e-2be4a12accb4.png)

33. Click the + sign after the /apps/app1/ branch you just added and select the subroutines tab and click the AD_Subroutine radio button and click Add Item

![image](https://user-images.githubusercontent.com/51786870/210580716-ea75c739-f367-493d-8f8c-8344605345ff.png)

34. Click the + sign after the AD_Subroutine Box you just added and select the **General Purpose** tab and click the **HTTP Headers** radio Button

![image](https://user-images.githubusercontent.com/51786870/210581133-5f4ac046-cc87-43c6-a977-ec18f4e9eda5.png)

35. Under **HTTP Header Modify**, click **Add new entry**

![image](https://user-images.githubusercontent.com/51786870/210581232-0eaebfb8-5f06-48a5-a105-9d95dc4a2152.png)

36. Type **AD_Groups** for header name and **%{session.adgroups.custom}** for Header Value and click Save

![image](https://user-images.githubusercontent.com/51786870/210581340-651d3517-bb61-4d73-890a-0255e3861e43.png)

37. In the Per-Request Policy follow the fallback branch for the URL Branching. Click on the Reject terminal and change to Allow

38. Your Per-Request Policy should now look like this

![image](https://user-images.githubusercontent.com/51786870/210581620-40bc5f1e-4ecb-489a-9713-7d40fe889420.png)

39. Navigate back to Local Traffic -> Virtual Servers and select your VIP, under the Access policy section of your VIP bind your Per-Session and Per Request policies

![image](https://user-images.githubusercontent.com/51786870/210581843-1b796677-804b-41a8-b32e-e9187bf251e0.png)

40. In a browser on your jumphost access **https://app.acme.com** you should see the webpage listed below, click the **Application1** link

![image](https://user-images.githubusercontent.com/51786870/210581976-c59dabe8-0d63-4c5f-90c7-952763f076db.png)

41. Authenticate with the user1 username and user1 password

![image](https://user-images.githubusercontent.com/51786870/210582342-0eef6594-73f9-48d8-875d-a8537cbc2425.png)

42. Notice the **Ad-Groups** header which contains the extracted AD group information submitted to the application as a HTTP Header

![image](https://user-images.githubusercontent.com/51786870/210582508-81e91a4c-f8a9-4856-b3d0-8b3f189af1d4.png)

What we have demonstrated here is the application of step-up authentication to a portion of the webpage, from there we extracted information from Active Directory to submit to the application in the form of an HTTP Headers

**Module 2 is now complete.**

## Module 3
## Module 3: Server-Side Single Sign-On and Webtop Access Policy Build

The purpose of this lab is to demonstrate Single Sign-On capabilities of APM. The SSO Credential Mapping action enables users to forward stored user names and passwords to applications and servers automatically, without having to input credentials repeatedly. This allows single sign-on (SSO) functionality for secure user access. As different applications and resources support different authentication mechanisms, the SSO system may be required to store and transform credentials to meet these requirements. For example, username and password may be transformed into forms-based authentication, a SAML assertion into Kerberos or Kerberos authentication into SAML.

Although a number of different SSO methods exist, this lab will demonstrate access single SSO method certificate authentication on the client side to Kerberos authentication to the backend server

Objective:

   * Gain an understanding of authentication transformation through APM, client side vs server side
   * Gain an understanding of the leveraging different auth methods
   * Develop an awareness of the different deployment models for single sign on

### Lab Requirements:

Virtual Servers policies and configuration for this lab were completed through the automation run in previous lab. We will review the components involved in this SSO solution.

Estimated completion time: 15 minutes

### Task 1: Policy review
**Note**
`All the objects and configuration have been completed for you. In this lab we will explore the configuration and test.`

1. From the jumphost launch Chrome and login to **bigip1.f5lab.local** as **admin/admin**

2. Navigate to **Access** -> **Profiles/Policies** -> **Access Profiles (Per-Session Policies)**

3. Locate solution6-psp and click **Edit**

![image](https://user-images.githubusercontent.com/51786870/210596291-d9976c28-7278-4d98-b4ba-cf7b03962ae1.png)

Let’s walk through the policy flow:

   * A user is prompted to select their certificate.
   * The validation of the user certificates is controlled via CA bundles selected in the Client-side SSL Profile.
   * The certificate is validated by OCSP if the user presents a certificate issued by a trusted CA
   * The othername field is extracted from the certificate
   * A LDAP query is performed to collect the sAMAccountName of the user
   * The domain and username variables are set
   * The user is granted access via the Allow Terminal
   * If the LDAP Query is unsuccessful, the user proceeds down the fallback branch to the Deny Terminal
   * If the OCSP check is unsuccessful, the user proceeds down the fallback branch to the Deny Terminal
   * If the user fails to present a certificate, the user proceeds down the fallback branch to the Deny Terminal

### Task 2: Supporting APM Objects review

1. Navigate to Access -> Authentication -> OCSP Responder and click on the AAA OCSP Responder object solution6-ocsp-servers

2. Change the configuration from Basic to Advanced. The OCSP Responder has been configured with the following settings:

   * URL: this field is only used if you check the Ignore AIA field
   * Certificate Authority File: contains the root ca bundle
   * Certificate Authority Path: this field is only used if you check the Ignore AIA field
   * The rest of the settings are default

![image](https://user-images.githubusercontent.com/51786870/210596653-7191348d-ad5f-4c8b-8e3e-6787a71d9273.png)

3. Navigate to **Access** -> **Authentication** -> **LDAP** for the AAA LDAP Object

4. Click on **solution6-ldap-servers**. A single LDAP server of 10.1.20.7 has been configured with a admin service account to support queries

![image](https://user-images.githubusercontent.com/51786870/210728534-5a774c67-b44b-4605-af9c-8a54b0bba0f9.png)

5. Navigated to **Access** -> **Single Sign-On** -> **Kerberos** and review the Kerberos SSO Object **solution6-kerbsso**

   * The Username Source field has been modified from the default to reference the sAMAccountName stored in session.logon.last.username
   * Kerberos Realm has been set to the Active Directory domain (realms should always be in uppercase)
   * The service account used for Kerberos Constrained Delegation (Service Account Names should be in SPN format)
   * SPN Pattern has been hardcoded to HTTP/solution6.acme.com (This is only necessary if the SPN doesn’t match the FQDN typed in the web browser by the user)

![image](https://user-images.githubusercontent.com/51786870/210728648-33336656-16e5-4967-bb58-74f818bdfd39.png)

### Task 3: Access Profile Configuration

1. Navigate to **Access** -> **Profiles/Policies** -> **Access Profiles (Per-Session Policies)** and locate the **solution6-psp** profile. Click on the profile and review the customized APM Profile Settings

2. Click on the SSO/Auth Domains of the APM profile and note it is configured with the Kerberos SSO Profile which will be used to authenticate to the backend server.

![image](https://user-images.githubusercontent.com/51786870/210728763-8ac6c972-3c33-4d36-a6ee-9c379c5cccc9.png)

3. Click on **Access Policy** and the Edit Access Policy for Profile “**solution6-psp**” link

**Note**

`You can also see from this screen any AAA server objects associated with this profile/policy. You can see we will be using the OCSP responder.

4. Click on the **On-Demand Cert Auth** box in the VPE. The agent uses the default settings of **Auth Mode = Request**

![image](https://user-images.githubusercontent.com/51786870/210728951-6aa8b876-a86b-4c86-9535-67dd48183263.png)

5. Click **Cancel**

6. Click on the **OCSP Auth** box. The OCSP Agent validates the certificate against the OCSP responder configured

![image](https://user-images.githubusercontent.com/51786870/210729019-949b006c-fcf7-4539-8f17-ced669a850af.png)

7. Click **Cancel**

8. Click the **upn extract** box. Under Assignment click on the **Change** link

![image](https://user-images.githubusercontent.com/51786870/210729125-cd5e0f9a-6dfd-42c7-bc10-440dd26e62a8.png)

![image](https://user-images.githubusercontent.com/51786870/210729279-e4fc65cb-b616-4f37-b052-a8b90e613515.png)

9. Note that a custom variable will be created called session.custom.upn. We will write an expression that will extract the othername:UPN field from the certificate for a new custom variable.

```
set x509e_fields [split [mcget {session.ssl.cert.x509extension}] "n"];
# For each element in the list:
foreach field $x509e_fields {
# If the element contains UPN:
if { $field contains "othername:UPN" } {
## set start of UPN variable
set start [expr {[string first "othername:UPN<" $field] +14}]
# UPN format is <user@domain>
# Return the UPN, by finding the index of opening and closing brackets, then use string range to get everything between.
return [string range $field $start [expr { [string first ">" $field $start] - 1 } ] ];  } }
# Otherwise return UPN Not Found:
return "UPN-NOT-FOUND";
```
10. Click **Cancel** twice

11. Click the **LDAP Query** box. The LDAP query connects to the LDAP server to the **dc=f5lab,dc=local** DN for a user that contains the userPrincipalName matching the value stored in session.custom.upn.

12. You can see that we are using the **AAA LDAP** object created early to validate the variable** session.custom.upn**. The LDAP query requests the **sAMAccountName** attribute if the user is found.

![image](https://user-images.githubusercontent.com/51786870/210729752-cc914f97-cab8-4957-9f84-b1da41d614a0.png)

13. Click on **Branch Rules**. The branch rule was modified to only require a LDAP Query passed condition

![image](https://user-images.githubusercontent.com/51786870/210729951-73093023-dc51-44f7-961f-4fd241a59c3d.png)

14. Click **Cancel**

15. Click the **set_variables** box. Two session variables are set

   * session.logon.last.username is populated with the value of the sAMAccountName returned in the LDAP query
   * session.logon.last.domain is populated with a static value for the Active Directory domain F5LAB.LOCAL

![image](https://user-images.githubusercontent.com/51786870/210730065-7620d779-45db-4ae3-87ae-e3dba1096620.png)

### Task 4: Customized LTM Profile settings

We will need to make some modifications to the client SSL profile to accommodate Certificate authentication.

1. Navigate to Local Traffic from the left menu. Under Partitions select the drop down and choose solution6. This will change the partition so that you can see the LTM objects used in this lab.

**Note**

`We deployed the LTM objects in to another administrative partition for the purposes of separating the objects. If you were to deploy this in your own environment using a partition is not a requirement.`

![image](https://user-images.githubusercontent.com/51786870/210730338-c391ccfd-2616-41f0-85b0-fe8144ed9ee8.png)

2. Navigate to **Local Traffic** -> **Profiles** -> **SSL** -> **Client**. Click on **solution6-clientssl**.

![image](https://user-images.githubusercontent.com/51786870/210730775-ac97e55b-ab1b-4b6d-abf4-d1996aedacba.png)

3. In he Client-side SSL profile scroll down to the **Client Authentication** section and notice it has been modified to support certificate authentication

**Trusted Certificate Authorities has been set to ca.f5lab.local**

   * The bundle validates client certificates by these issuers
   * The bundle must include all CAs in the chain

**Advertised Certificate Authorities has ben set to ca.f5lab.local**

   * The bundle controls which certificates are displayed to a user when they are prompted to select their certificate

![image](https://user-images.githubusercontent.com/51786870/210730514-fed3b891-14d4-44a5-a8c3-877a0e74d575.png)

### Task 5: Logging in from a user’s perspective

1. Open an incognito window in the Chrome browser and access https://solution6.acme.com

2. You will be presented with three possible certificates. Choose **User1** and click **OK**

![image](https://user-images.githubusercontent.com/51786870/210730868-f7fc7fb6-cd2c-44cf-867e-74cace42d53e.png)

3. If successful the user is granted access to the application

![image](https://user-images.githubusercontent.com/51786870/210731022-b713fa06-ab42-4515-a0f6-fd03e935efbd.png)

### Task 5: Webtop Access Policy Build - Create a Webtop resource

In this lab, we will add a Webtop resource to the Access Policy created in the previous lab. A full webtop provides an access policy ending for an access policy branch to which you can optionally assign portal access resources, app tunnels, remote desktops, and webtop links, in addition to network access tunnels. Then, the full webtop provides your clients with a web page on which they can choose resources, including a network access connection to start.

1. Expand the **Access** tab from the main menu on the left and navigate to **Webtops > Webtop Lists**.

2. Click **Create** to create a new Webtop called **MyFullWebtop**, select Type **Full** , uncheck **Minimize To Tray** and click **Finished**.

![image](https://user-images.githubusercontent.com/51786870/210731896-91980d9f-8e40-4b77-9163-e24bec0b9363.png)

### Task 6: Create Webtop Item

1. Browse to **Access** > **Webtops** >** Webtop Link** and click create.

2. Complete the following entries.

   * Name: F5Rocks
   * Link Type Dropdown: Application URI
   * Applicatoin URI : https://www.f5.com
   * Application Caption : F5 Rocks.

![image](https://user-images.githubusercontent.com/51786870/210732012-99e8bfd6-4469-4502-8b0e-8bed669eac67.png)

![image](https://user-images.githubusercontent.com/51786870/210732344-c20c014b-392a-403d-8416-7222b662dd39.png)


### Task 7: Add Webtop resource to existing Access Policy

1. Browse to **Access** > **Profiles / Policies** > **Access Profiles (Per-Session Policies)**, click on **Edit** for **MyAccessPolicy**. A new tab should open to the Visual Policy Editor for **MyAccessPolicy**.

![image](https://user-images.githubusercontent.com/51786870/210732515-badfd6a6-101e-4ab5-9125-e80320b6d53d.png)

2. In between the AD Auth APM Item and the Allow APM item click the + option to add an item.

![image](https://user-images.githubusercontent.com/51786870/210734861-4d5a367e-582d-4d40-8382-e3abfcea0f4b.png)

3. Select the **Advanced Resource Assign** object. Click on the “**Assignment Tab**” and select the “**Advanced Resource Assign**” radio button. Click **Add Item**.

![image](https://user-images.githubusercontent.com/51786870/210734961-270af786-b7e9-4d37-9ea6-159bc2cd9318.png)

4. Then Click the “**Add New Entry**” button.

![image](https://user-images.githubusercontent.com/51786870/210735178-04f26f96-e2d4-43ea-a4d6-57b0a8323cb8.png)

5. Then under the “**Expression Section**” click the “**Add/Delete**” button

![image](https://user-images.githubusercontent.com/51786870/210735387-d1316aac-bee5-47eb-b18b-ff1eb6d13a23.png)

6. Click on the **Webtop** tab, select the radio button for **MyFullWebtop**. Click on the **Webop Links** tab, and select the radio button for **F5Rocks** then click the Update button at the bottom of the screen.

![image](https://user-images.githubusercontent.com/51786870/210735560-21bb1343-32e8-49ae-a5f5-a876a0b5e465.png)

![image](https://user-images.githubusercontent.com/51786870/210735609-107d9c5e-69dc-46f1-b724-8f1ce043af71.png)

7. Click **Save**.

8. At the top left of the browser window, click on **Apply Access Policy**, then close the tab. Replace the Access Profile on your app-https VIP with your myaccesspolicy Access profile and set the Per-Request Policy to None

![image](https://user-images.githubusercontent.com/51786870/210735659-921f6382-d2d4-43e3-b79f-34b10a79e032.png)

9. Navigate to **Local Traffic** –> **Virtual Servers** –> **Virtual Server List**

**Note**

```
`Make sure you are in the Common Partition`

![image](https://user-images.githubusercontent.com/51786870/210735880-e6eb387a-f708-4eaa-9751-e5d09dc4e6b9.png)
```

10. Open the **app-https** Virtual server, scroll down to the **Access Policy** section and ensure that **MyAccessPolicy** has been assigned to this virtual server.

![image](https://user-images.githubusercontent.com/51786870/210736203-be0a6e93-73e7-420c-8df9-95e8821c1a91.png)

### Task 8: Testing

1. Open a **New Incognito web browser** to the virtual server created in the previous lab by navigating to **https://app.acme.com**. You will be presented with a Logon page similar to the one from the last lab.

2. Enter the following credentials:

| Username:	| user1 |
| ---- | ---- |
| Password:	| user1 |

3. Click **Logon**.

This will open the APM Webtop landing page that shows the resources you are allowed to access. In this lab, we’ve only configured one resource:

**F5 Rocks**, but you can add as many as you want and they will appear on this Webtop page.

### Task 9: Lab Cleanup

1. Open a new tab and click on the Access: PORTAL bookmark then select **CLASSES**

2. Locate the **102 Access Building Blocks** Tile and click on the **Stop** button

![image](https://user-images.githubusercontent.com/51786870/210737541-9c29e057-2e27-46aa-94d4-b7b208d30e99.png)




![image](https://user-images.githubusercontent.com/51786870/210558798-88c9bb09-18d9-41a0-b1bf-0dfb934bc100.png)

3. Wait about 30 seconds for the processing to begin

![image](https://user-images.githubusercontent.com/51786870/210737783-4e05e71b-921e-4f20-889b-c061fb392bca.png)

4. This process will take up to 30 seconds. Scroll to the bottom of the script and verify no issues.


**Module 3 is now complete.**



## Module 4 Part 1
## Module 4: Part 1 - SAML SP Access Guided Configuration (AGC)

The purpose of this lab is to configure and test SAML Federation Services. This lab will be configured in two parts.

Students will leverage Access Guided Configuration (AGC) to configure the various aspects of a SAML Service Provider (SP), import and bind to a SAML Identity Provider (IdP) and test SP-Initiated SAML Federation.

### Objective:

   * Gain an understanding of SAML Federation configurations and their component parts through Access Guided Configuration (AGC)
   * Gain an understanding of the access flow for IDP & SP Initiated SAML

### Lab Requirements:

   * All Lab requirements will be noted in the tasks that follow
   * Estimated completion time: 25-30 minutes

### Task 1 - Setup Lab environment

1. Open Chrome browser and launch the site https://portal.f5lab.local.

2. Click the **Classes** tab at the top of the page.

![image](https://user-images.githubusercontent.com/51786870/210740642-0de495be-7143-429a-a09a-e67680771656.png)

3. Scroll down the page until you see **202 - Federation** on the left

![image](https://user-images.githubusercontent.com/51786870/210740741-b1e4df56-236e-4648-84fc-e18d29870490.png)

4. Hover over tile **SAML SP Access Guided Configuration(AGC) Lab**. A start and stop icon should appear within the tile. Click the **Play** Button to start the automation to build the environment

![image](https://user-images.githubusercontent.com/51786870/210740902-14c8e875-15e1-4caa-9ba8-0a94c4825e25.png)

10. The screen should refresh displaying the progress of the automation within 30 seconds. Scroll to the bottom of the automation workflow to ensure all requests succeeded.

![image](https://user-images.githubusercontent.com/51786870/210740987-b2973049-4956-4242-8e06-784c0b01f400.png)

### Task 2: Configure a SAML Service Provider (SP) via AGC

1. Navigate to **Access** -> **Guided Configuration** in the left-hand menu.

2. Once **Guided Configuration** loads, click on **Federation**.

![image](https://user-images.githubusercontent.com/51786870/210741143-51f3a953-6064-4fe8-9f63-540891a72f32.png)

3. In the resulting **Federation** sub-menu click, **SAML Service Provider**.

![image](https://user-images.githubusercontent.com/51786870/210741211-994a0c0b-6f6a-4df1-b64a-ad3d3724cfbb.png)

4. In the resulting **SAML Service Provider** window, review the (**SP-Initiated**) flow and then click the **right arrow**.

![image](https://user-images.githubusercontent.com/51786870/210741515-54dad002-bc55-4c18-9282-a2be7da005fc.png)

5. Review the **IdP-Initiated** flow and then scroll down to the bottom of the window.

![image](https://user-images.githubusercontent.com/51786870/210741590-b438cc33-bb38-4ad7-b619-507b26643483.png)

6. Review the configuration objects to be created and the click **Next**.

### Task 3: Configure the Service Provider

1. In the **Service Provider Properties** section, enter the following values in the fields provided:

   * In the **Configuration Name** field input **sp.acme.com**.
   * In the **Entity ID** field input **https://sp.acme.com**.

2. In the **Security Settings** section, check the checkbox next to **Sign Authentication Requests**.

3. In the updated **Security Settings** section, use the dropdowns to select the following:

   * For the **Message Signing Key** select **sp.acme.com**.
   * For the **Message Signing Certificate** select **sp.acme.com**.

4. Click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210742865-1a5a90b6-02b0-4d9d-b295-0185d337661e.png)

### Task 3: Configure the Virtual Server

1. In the **Virtual Server Properties** section, enter the following values in the fields provided:

   * In the **Destination Address** field input **10.1.10.103**.
   * In the **Service Port** field input **443 HTTPS**
   * In the **Redirect Port** field input **80 HTTP**

2. In the **Client SSL Profile** section, use the arrows to move only the **wildcard.acme.com** profile to the right-hand column as shown.

3. Click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210743314-0113e83d-9382-4bdb-ae8c-117bbf64ec01.png)

### Task 4: Configure External IdP Connector¶

1. In the **External Identity Provider Connector Settings** section, use the first dropdown **Select Method** to configure your **IdP Connector** to select **Metadata**.

2. In the updated window, click the **Choose File** button and then browse the **Jumphost desktop** and select the **file idp_acme_com.xml**.

3. In the **Name** field, input **idp.acme.com**

4. Click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210749659-204a8794-0d73-4d65-968b-0f592b89c1a5.png)

### Task 5: Configure Pool

1. Click **Show Advanced Setting** in the upper right of the **Guided Configuration**.

2. In the **Pool Properties** section, use the dropdown to select **Create New** for **Select a Pool**.

3. In the **Health Monitors** section, use the arrows to move only the **/Common/http** health monitor to the right-hand column as shown.

4. In the **Resource Properties** section, use the dropdown to select **Least Connections (member)** for Load Balancing Method.

5. For the **Pool Servers** section, use the first dropdown for **IP Address/Node Nam**e to select **/Common/10.1.20.6**. Ensure port **80** and **HTTP** are set for the **Port**.

6. Click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210750219-05d35ace-356e-4a37-86c9-4eb901a1da74.png)

# Task 6: Configure SSO

1. In the Single Sign-On Settings section, check the Enable Signle Sign-On checkbox.

2. Use the **Selected Single Sign-On Type** dropdown to select **HTTP header-based**.

3. In the **Header Valu**e field, ensure **session.saml.last.identit**y is present.

4. In the **SSO Headers** section, makes sure the following values are correct:
   * **Header Operation**: **replace**
   * **Header Name**: **Authorization**
   * **Header Value**: **%{session.saml.last.identity}**

5. Scroll to the bottom of the window and Click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210750915-aca62225-899e-4bb8-ba96-595ca1d055ee.png)

### Task 7: Configure Endpoint Checks

1. In the **Endpoints Checks Properties** window, click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210751035-3d778d44-9509-4d0f-ac38-6d53453ad26c.png)

### Task 8: Configure Session Management

1. Review the **Session Managment** settings, in the **Timeout Settings** section then scroll to the bottom of the window and click **Save & Next**.

![image](https://user-images.githubusercontent.com/51786870/210751213-0c8d2195-4adf-4c85-9a9a-0f79e79e95d2.png)

### Task 9: Review the Summary and Deploy

1. Review the Summary, then scroll to the bottom of the window and click **Deploy**.

![image](https://user-images.githubusercontent.com/51786870/210751315-0e7788c6-f1c2-428c-ac8a-a3b3037304d2.png)

2. The application is now deployed click **Finish**.

![image](https://user-images.githubusercontent.com/51786870/210751439-7720ebd5-c61f-425e-a49e-ff130b0ed387.png)

3. Review the Access Guided Confguration window, **Status** for **sp.acme.com** is **DEPLOYED**.

![image](https://user-images.githubusercontent.com/51786870/210751474-37e2b64e-89b4-4ab3-b8d9-dc42ed68c353.png)

### Task 10: Testing the SAML Service Provider (SP)

1. Open Firefox from the Jumphost desktop and navigate to https://sp.acme.com

**Note**

`If you have issues, open Firefox in a New Private Window (Incognito/Safe Mode)`

2. Once the page loads, enter **user1** for username and **user1** for password in the **Secure Logon** form and click the **Logon** button.

![image](https://user-images.githubusercontent.com/51786870/210752366-0e362293-7746-45a6-b921-4e532c8d69c7.png)

3. The **sp.acme.com** application will now open if successfully configured.

![image](https://user-images.githubusercontent.com/51786870/210752424-5e6d096f-cae8-40fe-921f-20497155cc70.png)

### Task 11: Lab CleanUp

1. Navigate to **Access** -> **Guided Configuration** in the left-hand menu.

![image](https://user-images.githubusercontent.com/51786870/210752668-b3b66d52-1f13-4ade-aff2-1242de0f2e02.png)

2. Click the **Undeploy** button

![image](https://user-images.githubusercontent.com/51786870/210752757-ff475ebc-9292-42b1-91a6-4d98e05488a9.png)

3. Click **OK** when asked, “Are you sure you want to undeploy this configuration?”

![image](https://user-images.githubusercontent.com/51786870/210752866-187e5a98-ce08-4317-b93d-312f5a7a5812.png)

4. Click the **Delete** button once the deployment is undeployed

![image](https://user-images.githubusercontent.com/51786870/210752939-a455001f-38e2-4d3e-8c14-117f3a18b62e.png)

5. Click **OK** when asked, “Are you sure you want to delete this configuration?”

![image](https://user-images.githubusercontent.com/51786870/210753018-a3742dd7-af68-4008-b920-2d4851bd494d.png)

6. The Configuration section should now be empty

![image](https://user-images.githubusercontent.com/51786870/210753098-ba002f8a-eeea-4f63-9a22-9ed5cf98e302.png)

7. From a browser on the jumphost navigate to **https://portal.f5lab.local**

8. Click the **Classes** tab at the top of the page.

![image](https://user-images.githubusercontent.com/51786870/210753199-810dcb24-c553-48b9-9882-286fc41d65d6.png)

9. Scroll down the page until you see **202 - Federation** on the left

![image](https://user-images.githubusercontent.com/51786870/210753285-0cef4841-f8f2-4c67-8594-2a6ace05bd80.png)

10. Hover over the tile **SAML SP Access Guided Configuration(AGC) Lab**. A start and stop icon should appear within the tile. Click the Stop Button to start the automation to delete any prebuilt objects

![image](https://user-images.githubusercontent.com/51786870/210753359-e29f27af-552d-44d2-9835-f119d508fc2d.png)

11. The screen should refresh displaying the progress of the automation within 30 seconds. Scroll to the bottom of the automation workflow to ensure all requests succeeded. 

![image](https://user-images.githubusercontent.com/51786870/210753433-0c6ec134-41db-40ad-835b-af76461a577b.png)

**This concludes Part 1**

## Module 4 Part 2
## Module 4: Part 2 - SAML Identity Provider (IdP) - Certificate Auth

To access your dedicated student lab environment, you will need a web browser and Remote Desktop Protocol (RDP) client software. The web browser will be used to access the Unified Demo Framework (UDF) Training Portal. The RDP client will be used to connect to the jumphost, where you will be able to access the BIG-IP management interfaces (HTTPS, SSH).


Estimated completion time: 25 minutes

### Task 1 - Setup Lab Environment

1. Open the site **https://portal.f5lab.local**. 

2. Click the **Classes** tab at the top of the page.

![image](https://user-images.githubusercontent.com/51786870/210754960-ee437be0-b984-4bd9-a6e6-6b44abe49e86.png)

3. Scroll down the page until you see **301 SAML Federation** on the left

![image](https://user-images.githubusercontent.com/51786870/210755014-b526c231-f2fb-4295-831c-b005ae619c15.png)

4. Hover over tile **SAML Identity Provider (IdP) - Cert Auth**. A start and stop icon should appear within the tile. Click the **Play** Button to start the automation to build the environment

![image](https://user-images.githubusercontent.com/51786870/210789876-25451bfd-bc86-49e8-a086-bef38a10695f.png)

![image](https://user-images.githubusercontent.com/51786870/210789894-c3c59a85-c7c4-48d0-9b04-b55f592a5ab6.png)

5. The screen should refresh displaying the progress of the automation within 30 seconds. Scroll to the bottom of the automation workflow to ensure all requests succeeded.

![image](https://user-images.githubusercontent.com/51786870/210789920-d1e7bb4e-d30d-4f88-afd7-05e71b0f6649.png)

### Task 2 ‑ Configure the SAML Identity Provider (IdP)

**IdP Service**

1. Begin by selecting: **Access** ‑> **Federation** ‑> **SAML Identity Provider** ‑> **Local IdP Services**

2. Click the **Create** button (far right)

![image](https://user-images.githubusercontent.com/51786870/210755673-3611be4a-c83a-4bfa-a6d1-0b168026c627.png)

3. In the** Create New SAML IdP Service** dialog box, click **General Settngs** in the left navigation pane and key in the following:

   * **IdP Service Name**:	**idp.acme.com**
   * **IdP Entity ID**:	**https://idp.acme.com**

![image](https://user-images.githubusercontent.com/51786870/210755903-11d7b439-5157-4ac9-b0d1-fb73f19c8386.png)

**Note**

`The yellow box on “Host” will disappear when the Entity ID is entered`

4. In the **Create New SAML IdP Service** dialog box, click **Assertion Settings** in the left navigation pane and key in the following:

   * **Assertion Subject Type**:	**Persistent Identifier** (drop down)
   * **Assertion Subject Value**:	**%{session.logon.last.username}** (drop down)
   * **Authentication Context Class Reference**	**urn:oasis:names:tc:SAML:2.0:ac:classes:SmartcardPKI**

![image](https://user-images.githubusercontent.com/51786870/210791034-b6aacce8-e6ae-4073-88bc-0a59e030a301.png)

5. In the **Create New SAML IdP Service** dialog box, click **Security Settings** in the left navigation pane and key in the following:

   * **Signing Key**:	**/Common/idp.acme.com (drop down)**
   * **Signing Certificate**:	**/Common/idp.acme.com (drop down)**

**Note**

`The certificate and key were previously imported`

6. Click **OK** to complete the creation of the IdP service

![image](https://user-images.githubusercontent.com/51786870/210756787-3ba7d8d3-ce53-4d21-968f-dc12bed2da1f.png)

**SP Connector**

1. Click on **External SP Connectors** (under the **SAML Identity Provider** tab) in the horizontal navigation menu

2. Click specifically on the **Down Arrow** next to the **Create** button (far right)

3. Select **From Metadata** from the drop down menu

![image](https://user-images.githubusercontent.com/51786870/210757141-da3b6c4d-ce05-47d7-be26-2a0d61230ba3.png)

4. In the **Create New SAML Service Provider** dialogue box, click **Browse** and select the **sp_acme_com.xml** file from the **Desktop** of your jump host

5. In the **Service Provider Name** field, enter the following: **sp.acme.com**

6. Click **OK** on the dialog box

![image](https://user-images.githubusercontent.com/51786870/210757358-3fbc049a-6561-4d18-99e5-2e3358037fcb.png)

**Note**

`The sp_acme_com.xml file was created previously. Oftentimes SP providers will have a metadata file representing their SP service. This can be imported to save object creation time as has been done in this lab.`

7. Click on **Local IdP Services** (under the **SAML Identity Provider** tab) in the horizontal navigation menu

![image](https://user-images.githubusercontent.com/51786870/210757480-7b45dac6-a4b3-48cd-9d22-ff038773e739.png)

8. Select the **Checkbox** next to the previously created **idp.acme.com** and click the **Bind/Unbind SP Connectors** button at the bottom of the GUI

![image](https://user-images.githubusercontent.com/51786870/210757545-c42f600c-1987-4fda-87ba-a2c81978968a.png)

9. In the **Edit SAML SP’s that use this IdP** dialog, select the **/Common/sp.acme.com** SAML SP Connection Name created previously

10. Click the **OK** button at the bottom of the dialog box

![image](https://user-images.githubusercontent.com/51786870/210757644-ac141c6a-430c-4cc2-bb8e-9bb985cc9d1c.png)

11. Under the **Access** ‑> **Federation** ‑> **SAML Identity Provider** ‑> **Local IdP Services** menu you should now see the following (as shown):

   * **Name**:	**idp.acme.com**
   * **SAML SP Connectors**:	**sp.acme.com**

![image](https://user-images.githubusercontent.com/51786870/210757786-fa36ea2b-0304-47ba-8e7a-94fdc93aad60.png)

### Task 3 - Create a SAML Resource

1. Begin by selecting **Access** ‑> **Federation** ‑> **SAML Resources** >> **+ (Plus Button)**

![image](https://user-images.githubusercontent.com/51786870/210776992-3f388a59-b9f5-4f4b-9cf6-ba0a59ba2f25.png)

2. In the **New SAML Resource** window, enter the following values:

    * **Name**:	**sp.acme.com**
    * **SSO Configuration**:	**idp.acmem.com**
    * **Caption**:	**sp.acme.com**

3. Click **Finished** at the bottom of the configuration window

![image](https://user-images.githubusercontent.com/51786870/210777152-f7404ec8-c332-427e-b425-33dd17deb6bf.png)

### Task 4 - Create a Webtop

1. Select **Access** ‑> **Webtops** ‑> **Webtop Lists** >> **+ (Plus Button)**

![image](https://user-images.githubusercontent.com/51786870/210777275-f569aa09-d661-4c92-9983-b8a0d12cbc87.png)

2. In the resulting window, enter the following values:

    * **Name**:	**full_webtop**
    * **Type**:	**Full** (drop down)
    * **Minimize To Tray**:	**uncheck**

3. Click **Finished** at the bottom of the GUI

![image](https://user-images.githubusercontent.com/51786870/210777481-65eec353-33e0-4ef1-a130-29527fea4781.png)

### Task 5 - Create an OCSP Responder

1. Navigate to Access >> Authentication >> OCSP Responder >> Click the Plus (+) Sign.

![image](https://user-images.githubusercontent.com/51786870/210792185-4f199c19-5cf2-4de7-a7f4-f129b1333fc2.png)

2. Enter the following information for the OCSP Responder configuration:

   * Name:	ocsp_servers
   * Configuration:	Advanced
   * URL:	http://dc1.f5lab.local
   * Certificate Authority File	ca.f5lab.local
   * Certificate Authority Path:	/ocsp
   * Options:	Uncheck Nonce

3. Click **Finished**

![image](https://user-images.githubusercontent.com/51786870/210792482-e7d2e86f-dabd-4304-94f1-ba824e9bc867.png)


### Task 6 - Create an AAA LDAP Server

1. Navigate to Access >> Authentication >> LDAP >> Click the Plus (+) Sign.

![image](https://user-images.githubusercontent.com/51786870/210792615-453d2500-c0d6-4857-8976-7b8981a41374.png)

2. Enter the following information for the LDAP Server configuration:

Name:	ldap_servers
Server Connection:	Use Pool
Server Pool Name:	ldap_pool
Server Addresses:	10.1.20.7
Admin DN:	CN=admin,CN=Users,DC=f5lab,DC=local
Admin Password:	admin

3. Click Finished

![image](https://user-images.githubusercontent.com/51786870/210792889-74bbe4f2-ed52-4f9f-acc7-0ba64480f49f.png)

### Task 7 - Create a SAML IdP Access Policy

1. Select **Access** ‑> **Profiles/Policies** ‑> **Access Profiles (Per-Session Policies)**

2. Click the **Create** button (far right)

![image](https://user-images.githubusercontent.com/51786870/210792998-1cfb7f02-6259-47fe-a999-c8adf05a2b9d.png)

3. In the New Profile window, enter the following information:

   * **Name**:	**idp.acme.com‑psp**
   * **Profile Type**:	**All** (drop down)
   * **Profile Scope**:	**Profile** (default)
   * **Customization Type**:	**modern** (default)

![image](https://user-images.githubusercontent.com/51786870/210778935-8f5a8d3d-c287-46cf-8715-56492cb4e95f.png)

4. Scroll to the bottom of the **New Profile** window to the **Language Settings** section

5. Select **English** from the **Factory Built‑in Languages** menu on the right and click the **Double Arrow (<<)**, then click the **Finished** button.

6. The **Default Language** should be automatically set

![image](https://user-images.githubusercontent.com/51786870/210779125-a66399a3-e266-4977-a050-fbacaca326b6.png)

7. From the **Access** ‑> **Profiles/Policies** ‑> **Access Profiles (Per-Session Policies)** screen, click the **Edit** link on the previously created **idp.acme.com-psp** line

![image](https://user-images.githubusercontent.com/51786870/210779164-72f27535-c213-4aa4-853a-63e45b47357f.png)

8. Click the **Plus (+)** Sign between **Start** and **Deny**

![image](https://user-images.githubusercontent.com/51786870/210793118-347555ce-3030-47ba-bc33-2edd543340e0.png)

9. In the pop-up dialog box, select the **Logon** tab and then select the **Radio** next to **On-Demand Cert Auth**, and click the **Add Item** button

![image](https://user-images.githubusercontent.com/51786870/210794502-40143390-9dd7-4878-8bf9-01d8b9956a96.png)

10. Click Save in the resulting Logon Page dialog box

![image](https://user-images.githubusercontent.com/51786870/210794604-89029ba3-1231-463d-9f9d-7b4c2c7e2620.png)

11. On the successful branch of the On-Demand Cert Auth Policy-Item click the Plus (+) Sign

![image](https://user-images.githubusercontent.com/51786870/210794710-d5396d01-655c-44fc-9585-d6eefd742017.png)

12. In the pop-up dialog box, select the Authentication tab and then select the Radio next to OCSP Auth, and click the Add Item button

![image](https://user-images.githubusercontent.com/51786870/210794863-be46f00d-ff7a-4bd9-b9ad-c6284894741e.png)



13. Select /Common/ocsp_servers from the OCSP Responder drop down menu.

14. Click Save at the bottom of the window

![image](https://user-images.githubusercontent.com/51786870/210795031-ae68f557-755c-4580-a24a-07d2aa054bef.png)

15. Click the Plus (+) Sign on the successful branch between OCSP Auth and Deny

![image](https://user-images.githubusercontent.com/51786870/210795087-cd8d0446-63c4-4912-a990-5d1c143b1562.png)

16. In the pop-up dialog box, select the Assignment tab and then select the Radio next to Variable Assign, and click the Add Item button

![image](https://user-images.githubusercontent.com/51786870/210795197-7a94059b-da42-4e6d-b0f4-411474777ee5.png)

17. Enter the Name upn_extract

18. Click Add new entry

19. Click Change

![image](https://user-images.githubusercontent.com/51786870/210795392-e383eb63-b6cf-40ab-ba4f-0972ce82a72e.png)

20. Enter the Custom Variable session.custom.upn

21. Select Custom Expresssion from the right drop down menu

22. Enter the text below for the custom expression.

```
set x509e_fields [split [mcget {session.ssl.cert.x509extension}] "\n"];
# For each element in the list:
foreach field $x509e_fields {
# If the element contains UPN:
if { $field contains "othername:UPN" } {
## set start of UPN variable
set start [expr {[string first "othername:UPN<" $field] +14}]
# UPN format is <user@domain>
# Return the UPN, by finding the index of opening and closing brackets, then use string range to get everything between.
return [string range $field $start [expr { [string first ">" $field $start] - 1 } ] ];  } }
#Otherwise return UPN Not Found:
return "UPN-NOT-FOUND";
```

23. Click Finished

![image](https://user-images.githubusercontent.com/51786870/210795726-9892d4a5-1566-4cfd-af68-f55877f51265.png)

24. Click Save

![image](https://user-images.githubusercontent.com/51786870/210795763-a4f713a4-7310-4e76-8caf-db9dce303815.png)


25. Click the Plus (+) Sign between upn_extract and Deny

![image](https://user-images.githubusercontent.com/51786870/210795815-63c9c4b2-a25b-4430-8a61-52443b3e7e3d.png)

26. Change the **Name** of the branch to **Successful**.

![image](https://user-images.githubusercontent.com/51786870/210795896-cbabb9eb-2cf4-4323-a4ce-436cf2afd740.png)

27. In the LDAP Query Properties window, enter the following information:

Server:	/Common/ldap_servers (drop down)
Search DN:	dc=f5lab,dc=local 
SearchFilter:	(userPrincipalName=%{session.custom.upn})

28. Click Add new entry

29. Add sAMAccountName to the list of Required Attributes

![image](https://user-images.githubusercontent.com/51786870/210796110-ede296df-b67b-481f-b579-85b99b4fc057.png)

30. Click the Branch Rules tab

31. Click the X on the User Group Membership line

![image](https://user-images.githubusercontent.com/51786870/210796219-08299017-7eb4-4f80-a8a8-15da07428942.png)

32. Click Add Branch Rule

![image](https://user-images.githubusercontent.com/51786870/210796491-c40bc475-6171-4001-91ab-779a76983c5c.png)

33. Enter the name **LDAP Query Passed**

34. Click **change**

![image](https://user-images.githubusercontent.com/51786870/210796701-0058fef9-c6dd-48ba-9ab3-3cb42fe21cd6.png)

35. Click Add Expression

![image](https://user-images.githubusercontent.com/51786870/210796753-105d7a9b-bf0e-4944-978c-48a755707171.png)

36. Select LDAP Query from the Context dropdown menu

37. Select LDAP Query Passed from the Condition dropdown menu

38. Click Add Expression

![image](https://user-images.githubusercontent.com/51786870/210796929-e5b68dec-f323-4cad-87e6-fab42eaf2435.png)

39. Click Finsished

![image](https://user-images.githubusercontent.com/51786870/210796957-abb2948c-14e0-43c5-a4f8-99979e1abb37.png)

40. Click Save

![image](https://user-images.githubusercontent.com/51786870/210797039-f4054d66-92b8-4507-9c80-a29c3953fb0b.png)

41. Click the Plus (+) Sign on the LDAP Query Passed branch between LDAP Query and Deny

![image](https://user-images.githubusercontent.com/51786870/210797077-8fba8452-7b56-451e-8cc0-f86bb58aa3fd.png)

42.In the pop-up dialog box, select the Assignment tab and then select the Radio next to Variable Assign, and click the Add Item button

![image](https://user-images.githubusercontent.com/51786870/210797232-1c3e4123-9bb3-4ebb-9b08-2044b4cd5582.png)

43. Enter the Name set_username

![image](https://user-images.githubusercontent.com/51786870/210782278-81871182-9f55-48d5-9c0f-b1469690f8a5.png)

44. Click Add new entry

45. Click Change

![image](https://user-images.githubusercontent.com/51786870/210797369-0b40f90c-4e14-4a64-bdbd-6e2a9d170b78.png)

46. Enter the Custom Variable session.logon.last.username

47. Select Session Variable from the right drop down menu

48. Enter the session variable name session.ldap.last.attr.sAMAccountName

49. Click Finished

![image](https://user-images.githubusercontent.com/51786870/210797853-5d690ec1-614f-437b-902b-56249c591c61.png)

50. Click Save

![image](https://user-images.githubusercontent.com/51786870/210798000-67c44035-2d1f-4780-9b88-77d9831f61c0.png)

51. Click the Plus (+) Sign between set_username and Deny

![image](https://user-images.githubusercontent.com/51786870/210798053-283c5c38-233e-4407-95c6-729bfb079e9e.png)

52. In the pop-up dialog box, select the Assignment tab and then select the Radio next to Advanced Resource Assign, and click the Add Item button

![image](https://user-images.githubusercontent.com/51786870/210798093-2fc5ce9e-0da4-4926-a8ce-de6544c8c1da.png)

53. In the new Resource Assignment entry, click the Add/Delete link

![image](https://user-images.githubusercontent.com/51786870/210798136-0ed9a1c8-5618-4d56-ba8a-41e40b596953.png)

54. In the resulting pop-up window, click the SAML tab, and select the Checkbox next to /Common/sp.acme.com

![image](https://user-images.githubusercontent.com/51786870/210798276-42dfc0c1-5fde-438c-a993-ccb505d7cdff.png)

55. Click the Webtop tab, and select the Checkbox next to /Common/full_webtop

56. Click the Update button at the bottom of the window to complete the Resource Assignment entry

![image](https://user-images.githubusercontent.com/51786870/210798352-91905033-2b0a-4dfe-b405-5fa8fd777248.png)

57. Click the Save button at the bottom of the Advanced Resource Assign window

![image](https://user-images.githubusercontent.com/51786870/210798477-a6379f57-ce1b-4e0f-9024-3b594b4390d6.png)

58. In the Visual Policy Editor, select the Deny ending on the fallback branch following Advanced Resource Assign

![image](https://user-images.githubusercontent.com/51786870/210798595-11820a96-5187-4f9e-a3e3-f5c3f440af73.png)

59. In the Select Ending dialog box, selet the Allow radio button and then click Save

![image](https://user-images.githubusercontent.com/51786870/210798636-b7e8ddcf-f4d2-4428-b4f3-281e07ee06fb.png)

60. In the Visual Policy Editor, click Apply Access Policy (top left), and close the Visual Policy Editor

![image](https://user-images.githubusercontent.com/51786870/210798693-95a1e272-db74-415e-b277-5f3ed9dfacf9.png)


### Task 8 - Create a Client-side SSL Profile

1. Navigate to Local Traffic ‑> Profile -> SSL -> Client. Click the Plus (+) Sign

![image](https://user-images.githubusercontent.com/51786870/210798992-763c6993-4042-4030-b935-739d37a9ee7b.png)

2. Enter the Name idp.acme.com-clientssl

3. Check the custom box on the Certificate Key Chain Line

4. Click Add

![image](https://user-images.githubusercontent.com/51786870/210799065-e42d2255-e263-4729-b902-e80ac44da398.png)

5. Select acme.com-wildcard from the Certificate dropdown menu

6. Select acme.com-wildcard from the Key dropdown menu

7. Click Add

![image](https://user-images.githubusercontent.com/51786870/210799121-e5bcfa17-d763-4a65-82ea-93fd048fe350.png)

8. Check the custom box on the Trusted Certificate Authorities Line

9. Select ca.f5lab.local from the Trusted Certificate Authorities dropdown menu

10. Check the custom box on the Advertised Certificate Authorities Line

11. Select ca.f5lab.local from the Advertised Certificate Authorities dropdown menu

![image](https://user-images.githubusercontent.com/51786870/210799193-d3aa0436-ae41-48c7-97cc-7eb265940326.png)

12. Click Finished



### Task 9 - Create the IdP Virtual Server

1. Begin by selecting **Local Traffic** ‑> **Virtual Servers**

2. Click the **Create** button (far right)

![image](https://user-images.githubusercontent.com/51786870/210782542-d28ae08a-9fb3-43bd-ac69-34528c261c40.png)

3. In the **New Virtual Server** window, enter the following information:

   * **General Properties**
   * **Name**:	**idp.acme.com**
   * **Destination Address/Mask**:	**10.1.10.102**
   * **Service Port**:	**443**

![image](https://user-images.githubusercontent.com/51786870/210783276-099bf127-cffc-4622-bca9-0c3cb98f9e79.png)

   * **Configuration**
   * **HTTP Profile**:	**http** (drop down)
   * **SSL Profile (Client)**:	**idp.acme.com-clientssl**

![image](https://user-images.githubusercontent.com/51786870/210799759-1d3a2e3f-f228-4a4c-a799-a4ba1e93fd75.png)

   * **Access Policy**
   * **Access Profile**:	**idp.acme.com-psp**

![image](https://user-images.githubusercontent.com/51786870/210783632-2d2e861c-8d72-4138-958e-d763acc6058b.png)

4. Scroll to the bottom of the configuration window and click **Finished**

### Task 10 - Test the Configuration

1. From the **jumphost**, navigate to the SAML IdP you previously configured at **https://idp.acme.com**.

2. Select the user1 certificate

3. Click OK

![image](https://user-images.githubusercontent.com/51786870/210799983-78dbb4eb-ad12-4979-a205-d0dc011b1078.png)

4. Click **sp.acme.com**

5. You are then successfully logged into https://sp.acme.com and presented a webpage.

![image](https://user-images.githubusercontent.com/51786870/210800066-9f2c7fe3-09a0-439f-9af5-1a99e984aed2.png)

6. Review your Active Sessions (Access ‑> Overview ‑> Active Sessions­­­)

7. Review your Access Report Logs (Access ‑> Overview ‑> Access Reports)

### Task 11 - Lab Cleanup

1. From a browser on the jumphost navigate to https://portal.f5lab.local

2. Click the **Classes** tab at the top of the page.

![image](https://user-images.githubusercontent.com/51786870/210806286-7f5e114f-0f2c-4550-a535-91cf1acee990.png)

3. Scroll down the page until you see 301 SAML Federation on the left

![image](https://user-images.githubusercontent.com/51786870/210806359-40348c50-bb16-4810-9a8e-a877645cae1b.png)

4. Hover over tile SAML Identity Provider (IdP) - Cert Auth. A start and stop icon should appear within the tile. Click the Stop Button to trigger the automation to remove any prebuilt objects from the environment

![image](https://user-images.githubusercontent.com/51786870/210806402-12332300-d350-43ec-92ca-6e1352128c8e.png)

![image](https://user-images.githubusercontent.com/51786870/210806418-dbf1d40f-06fe-4c9f-be24-52fe66d4a863.png)

5. The screen should refresh displaying the progress of the automation within 30 seconds. Scroll to the bottom of the automation workflow to ensure all requests succeeded.

![image](https://user-images.githubusercontent.com/51786870/210806480-4b4bf967-440b-42a9-954b-003c162b23ce.png)

6. **This concludes the lab.**


## Class 2
### Class 2 – API Protection
  * Module 1: API Security AWAF and APM modules

# Part 1 Deploy an API Protection Profile

Lab 1: Deploy an API Protection Profile¶
Section 1.1 - Setup Lab Environment¶
To access your dedicated student lab environment, you will need a web browser and Remote Desktop Protocol (RDP) client software. The web browser will be used to access the Unified Demo Framework (UDF) Training Portal. The RDP client will be used to connect to the jumphost, where you will be able to access the BIG-IP management interfaces (HTTPS, SSH).

Click DEPLOYMENT located on the top left corner to display the environment

Click ACCESS next to jumphost.f5lab.local

![image](https://user-images.githubusercontent.com/51786870/210808500-825a8300-49e2-4685-a0f0-410c6fd0a78d.png)

Select your RDP resolution.

The RDP client on your local host establishes a RDP connection to the Jumphost.

5. Login with the following credentials:

User: f5lab\user1
Password: user1

6. After successful logon the Chrome browser will auto launch opening the site https://portal.f5lab.local. This process usually takes 30 seconds after logon.

![image](https://user-images.githubusercontent.com/51786870/210808662-e67f0ea6-4582-4800-85cc-569c5561c72b.png)

7. Click the Classes tab at the top of the page.

8. Scroll down the page until you see 304 API Protection on the left

![image](https://user-images.githubusercontent.com/51786870/210808725-9b2f8e36-48db-410f-b31b-9dcf45bd319a.png)

9. Hover over tile Deploy an API Protection Profile. A start and stop icon should appear within the tile. Click the Play Button to start the automation to build the environment

![image](https://user-images.githubusercontent.com/51786870/210808769-b574604c-4fec-4aee-9a9b-90a41e50f498.png)

10. The screen should refresh displaying the progress of the automation within 30 seconds. Scroll to the bottom of the automation workflow to ensure all requests succeeded. If you you experience errors try running the automation a second time or open an issue on the Access Labs Repo.

![image](https://user-images.githubusercontent.com/51786870/210808804-9442470a-a735-4465-b8b1-0081bb2b0baf.png)

# Section 1.2 - Implement Course-Graing Access Control
# Task 1 - Create a Provider¶

The cornerstone of the API protection profile is the ability to authorize users using JWT. Unlike Guided Configuration that creates the JWT Provider for you based on a few defined parameters, you must create the provider manually.

1. Navigate to Access >> Federation >> OAuth Client/Resource Server >> Provider. Click the + (Plus Symbol)

![image](https://user-images.githubusercontent.com/51786870/210808884-f4940a4a-bafe-4fb0-a538-be9aa2888fd9.png)

2. Configure the following parameters:

Name: api-as-provider
Trusted Certificate Authorities: ca.acme.com.
OpenID URL: replace f5-oauth.local with as.acme.com

3. Click Discover

![image](https://user-images.githubusercontent.com/51786870/210808974-05111009-04ca-4fbf-9c56-b88ff9953439.png)

4. The Authentication URI, Token URI, Token Validation Scope URI, UserInfo URI and keys should be updated.

![image](https://user-images.githubusercontent.com/51786870/210809025-ef7cb933-5b0a-4c34-98f3-789144f0e3ad.png)

5. Click Save

### Task 2 - Create a JWT Provider

1. Navigate to Access >> Federation >> JSON Web Token >> Provider List. Click the + (Plus Symbol).

![image](https://user-images.githubusercontent.com/51786870/210809131-dc06daa8-89b0-42aa-b362-5ad53849f7fb.png)

2. Enter the name: as-jwt-provider

3. Click Add so api-as-provider is added to list of providers

4. Click Save

![image](https://user-images.githubusercontent.com/51786870/210809176-cc46cab3-10e7-409a-a581-2e16ccdc0f58.png)

### Task 3 - Create an API Protection Profile

1. Navigate to API Protection >> Profile. Click the + (plus symbol)

![image](https://user-images.githubusercontent.com/51786870/210809253-048fc95e-bc08-42e5-b6fb-c523452ab33a.png)

**Note**

`The JSON file is located on the jumpbox in c:\access-labs\class3\module4\student_files`

2. Enter the following parameters:

Name: api-protection
OpenAPI File: Active Directory OpenAPI.json
DNS Resolver: internal-dns-resolver
Authorization: OAuth 2.0

3. Click Add

4. Click Save

![image](https://user-images.githubusercontent.com/51786870/210809327-d74b6b2e-fa54-43e2-80d0-7079689f10c3.png)



### Task 4 - Explore the Path Configuration


1. Note the Spec file contained a single path of /user but it supports four different request methods.

2. The API server that all requests will be sent to is http://adapi.f5lab.local:81

![image](https://user-images.githubusercontent.com/51786870/210809478-e47b9b8a-4e40-4047-83e4-65e0bc4d4f1c.png)

### Task 5 - Associate a JWT Provider

1. Click Access Control from the top ribbon

2. Click Edit (Per Request Policy)

![image](https://user-images.githubusercontent.com/51786870/210809573-6e5d505a-cdc9-4fcd-844d-c4c4b79a7969.png)

3. Notice the same paths displayed in the API Protection profile appear here. Currently there is no fine-grained access control. We will implement it later in the lab.

4. Click the + (plus symbol) next the Subroutine OAuth Scope Check AuthZ to expand its properties:

![image](https://user-images.githubusercontent.com/51786870/210809637-61a89bf3-4119-47ad-801f-a7d2fe524eec.png)

**Note**

`The OAuth scope agent currently has a red asterisk since no provider is associated with it.`

5. Click OAuth Scope

![image](https://user-images.githubusercontent.com/51786870/210809703-a6812ded-a848-4089-b691-6a478c92dd1c.png)

6. Enter the following parameters:

Token Validation Mode: Internal
JWT Provider List: as-jwt-provider
Response: api-protection_auto_response1

7. Click Save

![image](https://user-images.githubusercontent.com/51786870/210809787-1b08a091-8cc7-475c-bd7f-759063de4764.png)

### Task 6 - Create a virtual server

1. Navigate to Local Traffic >> Virtual Servers >> Virtual Server List. Click the + (plus symbol)

![image](https://user-images.githubusercontent.com/51786870/210809881-ffca2cdd-3668-4db0-b75d-517a23e19412.png)

2. Enter the following parameters:

Name: api.acme.com
Destination Address/Mask: 10.1.10.102
Service Port: 443
HTTP Profile (Client): http
SSL Profile(Client): acme.com
Source Address Translation: Auto Map
API Protection: api-protection

3. Click Finished

![image](https://user-images.githubusercontent.com/51786870/210809985-65d6abc2-9f07-41d0-bc6c-1743b0a44e39.png)
![image](https://user-images.githubusercontent.com/51786870/210810026-cf28889c-0cf7-4678-8008-2dd87f4cf5b5.png)


### Task 7 - Import Postman Collections

1. From the Jumpbox, open Postman via the desktop shortcut or toolbar at the bottom

![image](https://user-images.githubusercontent.com/51786870/210810096-ff789a70-6769-45ca-91be-51937dee2dfa.png)

2. Click Yes if prompted for “Do you want to allow this app to make changes to your device?”

![image](https://user-images.githubusercontent.com/51786870/210810121-c5a22a81-6ad4-4aef-a7ea-0ae1d4404199.png)

3. Click Import located on the top left of the Postman application

![image](https://user-images.githubusercontent.com/51786870/210810189-20b2d97d-ff03-49c9-b7a7-67521ce6816c.png)

4. Click Upload Files

![image](https://user-images.githubusercontent.com/51786870/210810232-68cc9ee7-f4c9-4065-b019-2b697576ec96.png)

5. Navigate to C:\access-labs\class3\module4\student_files, select student-class3-module4-lab01.postman_collection.json, and click Open

![image](https://user-images.githubusercontent.com/51786870/210810265-91802266-dbeb-49f1-89bd-f8f58a72d736.png)

6. Click Import

![image](https://user-images.githubusercontent.com/51786870/210810300-df027855-7e45-41e7-a77c-4d601291a955.png)

7. A collection called student-class3-module4-lab01 will appear on the left side in Postman

### Task 8 - Retreive your OAuth clientID

1. Expand the student-class3-module4-lab01 Collection

2. Select the request Request1: Retrieve Postman ClientID

3. Click Send

![image](https://user-images.githubusercontent.com/51786870/210810449-7750fcd3-2dc1-4bd7-838a-c4e42f8ff4b6.png)

4. You receive a 200 OK with a response body. The clientID is now stored as a Postman Variable to be used in future requests. Your ClientID will not be the same as displayed in the screenshot below.

![image](https://user-images.githubusercontent.com/51786870/210810489-8e0d7ae1-ec8e-4c23-8ae3-1f71920c12eb.png)

### Task 9 - Attempt to Retrieve User1's Attributes without JWT

1. Select the request Request 2: Retrieve User Attributes without JWT

2. Click Send

3. You receive a 403 Forbidden response status code since you do not have a valid JWT

![image](https://user-images.githubusercontent.com/51786870/210810563-78b3507e-3fd2-4796-9564-df5db0162554.png)

### Task 10 - Retrieve User1's Attributes with a JWT

1. Select the request Request 3: Retrieve User Attributes with JWT

2. Select the Authorization tab

3. Click Get New Access Token

![image](https://user-images.githubusercontent.com/51786870/210810662-08c9d64a-2854-4d38-b38f-8c0aecc04420.png)

4. Login using Username: user1, Password: user1

![image](https://user-images.githubusercontent.com/51786870/210810705-bc410156-5385-4e43-b9d3-809b768b719e.png)

5. Click Use Token.

![image](https://user-images.githubusercontent.com/51786870/210810722-1dc55512-afc1-4866-a573-e1d34a5e4bed.png)

6. Notice the Access Token field is now populated

![image](https://user-images.githubusercontent.com/51786870/210810759-2aa9d49c-7f4d-414c-b489-b6f8e81b0b9e.png)

7. Click Send

8. You receive a 200 OK response status code with attributes for user1 in the body of the response

![image](https://user-images.githubusercontent.com/51786870/210810838-361146dc-db44-4fb7-a2cc-82facb7ae47d.png)

### Task 11 - Set a Valid User Attribute

1. Select the request Request 4: Update a Valid User Attribute

2. Select the Authorization tab

3. Select the previously created User1 token from the Available Tokens dropdown

![image](https://user-images.githubusercontent.com/51786870/210810955-3af82f5c-b737-4da6-b2b0-2bfdccad628e.png)

4. The Token field is now populated

![image](https://user-images.githubusercontent.com/51786870/210810987-6ff05b56-e188-4069-ae6c-dffc7b616564.png)

5. Click Send

**Note**

`If you receive a 403 response status code, request a new token. You can change the name of the token request prior to sending by setting the Token Name. You can delete expired tokens by clicking the Available Tokens dropdown, clicking Manage Tokens, and then clicking the trashcan next to the Token.`

6. You receive a 200 OK response status code with a response body that contains user1’s employeeNumber 123456

![image](https://user-images.githubusercontent.com/51786870/210811061-722e54c7-24be-4f4e-84ad-cfb492f6698a.png)

### Task 12 - Set an Nonexistent User’s Attribute

1. Select the request Request 5: Update a Nonexistent User's Attribute

2. Select the Authorization tab

3. Select the previously created User1 token from the Available Tokens dropdown

4. The Token field is now populated

5. Click Send

**Note**

`If you receive a 403 response status code, request a new token. You can change the name of the token request prior to sending by setting the Token Name. You can delete expired tokens by clicking the Available Tokens dropdown, clicking Manage Tokens, and then clicking the trashcan next to the Token.`

6. You receive a 2O0 OK response status code. The request successfully passed through the API Gateway, but the server failed to process the request.

![image](https://user-images.githubusercontent.com/51786870/210811165-fddf5cd4-55bb-45f2-840b-07bb06f8026c.png)

### Task 13 - Update a Valid User with PUT

1. Select the request Request 6: Update a Valid User’s Attribute with PUT

2. Select the Authorization tab

3. Select the previously created User1 token from the Available Tokens dropdown

4. The Token field is now populated

5. Click Send

6. You receive a 403 Forbidden response status code. This is expected because the PUT Method was not specified in the API Protection Profile for the path /user

![image](https://user-images.githubusercontent.com/51786870/210811275-d720a4a3-066e-412b-9daf-1fdfe413f508.png)

### Task 14 - Create a User

1. Select the request Request 7: Create a User

2. Select the Authorization tab

3. Select the previously created User1 token from the Available Tokens dropdown

![image](https://user-images.githubusercontent.com/51786870/210811336-4ccd7836-9b98-4e35-b3dc-e8a5b37fb20f.png)

4. Click Send

5. You receive a 200 OK response status code with a response body that contains Bob Smith’s user attributes

![image](https://user-images.githubusercontent.com/51786870/210811445-d8f63dae-716f-4044-b0f1-d7d9e92b3eb0.png)

### Task 15 - Request invalid endpoint

1. Select the request Request 8: Request Invalid Endpoint

2. Select the Authorization tab

3. Select the previously created User1 token from the Available Tokens dropdown

4. The Token field is now populated

5. Click Send

6. You receive a 403 Forbidden response status code. This is expected because the path /hacker/attack was not specified in the API Protection Profile

![image](https://user-images.githubusercontent.com/51786870/210811543-b18bed88-cdde-41eb-a66f-e6c0f86e77a7.png)

# Section 1.3 - Implement Fine-Grained Access Controls

Up to this point any authenticated user to the API is authorized to use them. In this section we will restrict user1’s ability to create users, but will still be able to modify a user’s employee number.

### Task 1 - Retrieve Group Membership Subsession Variable

**Note**

`In order to implement fine-grained control the session variables that contain the data must be known. This first session shows you how to display the session variables and their values.`

1. From the Jumpbox desktop click on the BIG-IP1 Putty icon

![image](https://user-images.githubusercontent.com/51786870/210811753-70c3351a-fede-4389-929c-63086105db8d.png)

2. Enter the command sessiondump –delete all to remove any existing APM sessions

![image](https://user-images.githubusercontent.com/51786870/210811814-d73c1ba8-ef0e-4943-8866-bd29fde98f5c.png)

3. Enter the command tailf /var/log/apm. Hit enter a few times to create some space on the screen

![image](https://user-images.githubusercontent.com/51786870/210811860-283fb6f7-42eb-4d00-ac50-de775bea9358.png)

4. From Postman, Select the request Request 3: Retrieve User Attributes with JWT. The Authorization field should already be populated with User1’s token.

5. Click Send

6. You receive a 200 OK response status code with attributes for user1 in the body of the response

![image](https://user-images.githubusercontent.com/51786870/210811936-e53fc002-3d55-4ad8-b106-02b247f741b6.png)

**Note**

`Your SessionID will be different`

7. Return to the CLI and examine the logs. You will see a message about a new subsession being created. Copy the subsession ID

![image](https://user-images.githubusercontent.com/51786870/210812019-e01526f7-6e68-4102-95b2-becf36380b0d.png)

8. Exit the logs using Ctrl+Z

9. Enter the command sessiondump -subkeys < subsessionID >

![image](https://user-images.githubusercontent.com/51786870/210812072-7df648ee-29e1-4d0a-a29c-b1ab59efd732.png)
 
10. Scroll through input until you find the session variable for subsession.oauth.scope.last.jwt.groups

![image](https://user-images.githubusercontent.com/51786870/210812107-621641c8-bb8c-4445-ad02-c1395a3eedf8.png)


 ### Task 2 - Edit the per-request policy

1. Return to BIG-IP1’s management interface and navigate to Access >> API Protection >> Profile. Click Profile to modify the previously created API protection Profile (not the + Plus symbol)

2. Click Edit Under Per-Request Policy

![image](https://user-images.githubusercontent.com/51786870/210812473-5c817eef-1464-42d2-8979-d66df431a53a.png)

3. Click the Allow terminal located at the end of the POST /user branch

![image](https://user-images.githubusercontent.com/51786870/210812448-e80179ed-1d4c-4145-9778-3903e8db71c9.png)

4. Select Reject

5. Click Save

![image](https://user-images.githubusercontent.com/51786870/210812529-3faa57d9-fd54-482d-97d8-a04450ef2296.png)

6. Click the + (Plus Symbol) on the POST /user branch

![image](https://user-images.githubusercontent.com/51786870/210812574-554eb306-f8a7-4eaa-9fbb-1288cc8017b5.png)

7. Click the General Purpose tab

8. Select Empty

9. Click Add Item

![image](https://user-images.githubusercontent.com/51786870/210812653-1484e8d9-0ec6-4268-9f5b-d4792aad8a94.png)

10. Enter the name Claim Check

![image](https://user-images.githubusercontent.com/51786870/210812693-c4c27e05-dfd4-4b78-ade3-19d3f848ebcf.png)

11. Click the Branch Rules tab

12. Click the Add Branch Rule

![image](https://user-images.githubusercontent.com/51786870/210812985-bdba7cb1-6f34-421b-bfe4-e6164e5d8d6a.png)

13. Enter Name CreateUser

14. Click Change

![image](https://user-images.githubusercontent.com/51786870/210813088-82db6fc3-66dd-49bc-be8f-ee72447895d2.png)

15. Click the Advanced tab

16. Enter the string in the notes section to restrict access to only members of the CreateUser Group. Make sure the ” characters are properly formatted after pasting. If they aren’t, simply delete and re-enter them manually.

17. Click Finished

**Note**

`expr {[mcget {subsession.oauth.scope.last.jwt.groups}] contains “CreateUser”}`

![image](https://user-images.githubusercontent.com/51786870/210813192-020afd46-c21e-433e-b31b-a224e5872963.png)

18. Click Save

![image](https://user-images.githubusercontent.com/51786870/210813285-8b034e75-5ef3-4aec-b1fc-8f5905f6fd0b.png)

19. Click Reject on the CreateUser Branch to permit access

![image](https://user-images.githubusercontent.com/51786870/210813342-860e35ca-19da-4028-a7f6-cf06f98f28fd.png)

20. Select Allow

21. Click Save

![image](https://user-images.githubusercontent.com/51786870/210813385-ce4f0624-1619-445b-a3a9-ebc21a955944.png)

22. Review the Policy Flow

![image](https://user-images.githubusercontent.com/51786870/210813443-d7151d18-c3e6-4ef4-a759-bac8430d7054.png)

### Task 3 - Test the Fine-Grained Access Control with user1
 
1. From Postman select the request Request 7: Create User

2. Select the Authorization Tab

3. Select the previously created User1 token from the Available Tokens dropdown

4. The Token field is now populated

5. Click Send

6. You receive a 403 Forbidden response status code when using user1. User1 does not contain the proper claim data.

![image](https://user-images.githubusercontent.com/51786870/210813556-681960d7-318c-452e-9ed7-e48e1539c119.png)

### Task 4 - Test the Fine-Grained Access Control with user2
 
1. Select the request Request 7: Create User

2. Select the Authorization tab

3. Ensure the Token Name is set to user2

4. Click Get New Access Token

![image](https://user-images.githubusercontent.com/51786870/210813638-335c42ff-30d1-47e3-adca-bd4cf14ac65a.png)

5. Login using Username: user2, Password: user2

![image](https://user-images.githubusercontent.com/51786870/210813662-4dc1c0ac-b088-4064-a762-4f3a999a312f.png)

6. Ensure the User2 Token is selected

7. Click Use Token at the top right.

![image](https://user-images.githubusercontent.com/51786870/210813703-2d7702a3-080e-46bf-bf45-3aa05091da4f.png)

8. The Token field is now populated

9. Click Send

10. You receive a 200 OK response status code when using user2. User2 does contain the proper claim data

![image](https://user-images.githubusercontent.com/51786870/210813757-cfefebc2-ad05-4718-bce2-731980d7f6fd.png)

# Section 1.4 - Implement Rate Limiting
The API Protection Profile allows a BIG-IP administrator to throttle the amount of connections to an API through the use of Key Names.

### Task 1 - Test pre-rate limiting Access

1. Click the arrow located to the right of the student-class3-module4-lab01 collection.

2. Click Run

![image](https://user-images.githubusercontent.com/51786870/210814346-33dcad62-1075-4cae-8ed4-5c5054df2b82.png)

3. Deselect all requests except Request 3: Retrieve User Attributes with JWT

4. Set the iterations to 100

5. Click the blue Run Student-class3-module4-la… button

![image](https://user-images.githubusercontent.com/51786870/210814397-09cf859a-d3a1-43fc-8a52-df34794a4420.png)

6. You receive a 200 OK for every request. Leave Runner open

![image](https://user-images.githubusercontent.com/51786870/210814431-2c1d9be2-77a8-40f0-af39-d4eb9a37b42b.png)

### Task 2 - Define the rate limiting keys

1. Navigate to API Protection >> Profile. Click Profile to modify the previously created API protection Profile. Not the + Plus symbol.

![image](https://user-images.githubusercontent.com/51786870/210814529-f4621d8d-8e56-4822-b006-025fae35a1d3.png)

2. Click api-protection

![image](https://user-images.githubusercontent.com/51786870/210814566-97c19314-ea05-4b3b-bf40-9ce349d350ff.png)

3. Click Rate Limiting from the top ribbon

![image](https://user-images.githubusercontent.com/51786870/210814656-0c14714e-bcea-460a-9a65-4fb42148adaa.png)

**Note**

`The API protection profile default settings contains five Key Names created, but their values are empty. Additional Keys can be created if necessary`

4. Click api-protection_auto_rate_limiting_key1

![image](https://user-images.githubusercontent.com/51786870/210814710-6ec68574-d932-4a57-a638-6c97c0bd9750.png)

5. Enter the Key Value %{subsession.oauth.scope.last.jwt.user}

6. Click Edit

![image](https://user-images.githubusercontent.com/51786870/210814761-872092c6-a10b-4120-bc0b-1b10f6255882.png)

7. Click api-protection_auto_rate_limiting_key2

8. Enter the Key Value %{subsession.oauth.scope.last.jwt.groupid}

9. Click Edit

![image](https://user-images.githubusercontent.com/51786870/210814834-2dcc66f5-cf6a-4e52-a580-63b5b5902646.png)

10. Click api-protection_auto_rate_limiting_key3

11. Enter the Key Value %{subsession.oauth.scope.last.jwt.client}

12. Click Edit

![image](https://user-images.githubusercontent.com/51786870/210814899-2465165c-821a-494a-a851-d15686c53bd1.png)

13. Click api-protection_auto_rate_limiting_key4

14. Enter the Key Value %{subsession.oauth.scope.last.jwt.tier}

15. Click Edit

![image](https://user-images.githubusercontent.com/51786870/210814941-5b96c37b-ebe3-495f-a65c-d985ace7dd4f.png)

16. Click api-protection_auto_rate_limiting_key5

17. Enter the Key Value %{subsession.oauth.scope.last.jwt.org}

18. Click Edit

![image](https://user-images.githubusercontent.com/51786870/210814996-d83714bc-6638-47bf-b163-5622e5a34010.png)

19. Click Save

![image](https://user-images.githubusercontent.com/51786870/210815026-aa85e854-0d15-4aef-b904-4c86527141ac.png)

### Task 3 - Create a Rate Limiting Policy

1. Click Create in the rate limiting section

![image](https://user-images.githubusercontent.com/51786870/210815069-66c75ce4-4c41-445f-a0ad-8f807eb2cd38.png)

2. Enter the Name acme-rate-limits

3. Move all five keys under Selected Keys

4. Enter 10 for the number of requests per minute

5. Enter 5 for the number requests per second

6. Click Add.

![image](https://user-images.githubusercontent.com/51786870/210815137-90e4d2bf-e9bf-4575-b527-ecf2d98b5c6a.png)

7. Click Save

![image](https://user-images.githubusercontent.com/51786870/210815181-a14518a0-15f7-4e1e-91a4-dae8f95f9213.png)

### Task 4 - Apply the Rate Limiting Policy

1. Click Access Control from the ribbon

![image](https://user-images.githubusercontent.com/51786870/210815257-fa43dc39-9016-4054-bf1d-f6266f056a2f.png)

2. Click Edit Per Request Policy

![image](https://user-images.githubusercontent.com/51786870/210815299-ffa49ab4-a579-495f-93c3-1a8d699fd437.png)

3. Click the + (Plus Symbol) on the Out branch of the OAuth Scope Check AuthZ Macro

![image](https://user-images.githubusercontent.com/51786870/210815330-669a557d-c437-4680-82a8-2e95798b5af1.png)

4. Click the Traffic Management tab

5. Select API Rate Limiting

6. Click Add Item

![image](https://user-images.githubusercontent.com/51786870/210815391-25d10b69-c8af-45cb-a578-c7e28cbc49b5.png)

7. Click Add new entry

8. Select acme-rate-limits

9. Click Save

![image](https://user-images.githubusercontent.com/51786870/210815446-1cd9c9cc-8213-4f38-8063-b549332de5a1.png)

10. Verify the Rate Limiting agent now appears in the appropriate location

![image](https://user-images.githubusercontent.com/51786870/210815496-25c8ea22-8fb5-486a-91cf-a5ae117b372d.png)

### Task 5 - Test Rate Limiting

1. Return to Postman

2. Click Run Again to rerun the request an additional 100 times.

![image](https://user-images.githubusercontent.com/51786870/210815563-c51cb0fd-55d3-4a79-8588-1eaf56d3cabc.png)

3. On the 6th request you begin to receive a 429 Too Many Requests response status code


Section 1.5 - Onboard a New API¶
Organizations change. With this change, new APIs are introduced requiring modifications to the API Gateway. In this section you will learn how to add additional paths.

Task 1 - Verify no access to API¶
From Postman, select the request Request 9: Create DNS Entry

Select the Authorization tab

Select the previously created User1 token from the Available Tokens dropdown

The Token field is now populated

Click Send

You receive a 403 Forbidden response status code because the the new API has not been published at the Gateway.

Task 2 - Add the new API path¶
From the browser, navigate to API Protection >> Profile. Click Profile to modify the previously created API protection Profile (not the + Plus symbol)

image48

Click api-protection

image64

Click Paths

image65

Click Create

image66

The URI /dns

Select the Method POST

Click Add

image67

Click Save

image68

Task 3 - Test Access to the new path¶
From Postman, select the request Request 9: Create DNS Entry
You receive a 200 OK that the endpoint is now published.
image102





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

![image](https://user-images.githubusercontent.com/51786870/210753550-c4afc7ab-9105-44db-aa49-8950b19ec101.png)

