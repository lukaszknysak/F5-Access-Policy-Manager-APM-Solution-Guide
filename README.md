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





















