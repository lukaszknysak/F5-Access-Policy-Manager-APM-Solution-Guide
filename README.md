# THIS IS UNDER CONSTRUCTION

# F5-Access-Policy-Manager-APM-Solution-Guide


# Welcome to the Access Policy Manager (APM) lab guide.

The following labs and exercises will instruct you on how to configure and troubleshoot various APM use cases. This guide is intended to complement lecture material provided during the course as well as a reference guide that can be referred to after the class as a basis for configuring Access solutions in your own environment.

[Original Lab Guide](https://github.com/f5devcentral/f5-agility-labs-iam)



### Class 1 – Access Policy Manager Solution (F5 APM)
  * Module 1: APM GUI Overview
  * Module 2: Building a Basic Access Policy
  * Module 3: Server-Side Single Sign-On
  * Module 4: Identity Federation sample use case

### Class 2 – API Protection
  * Module 1: API Security AWAF and APM modules

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
 
# Module 1: APM GUI Overview
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

### Task 9: Lab Cleanup¶

1. Open a new tab and click on the Access: PORTAL bookmark then select **CLASSES**

2. Locate the **APM GUI Overview** Tile and click on the **Stop** button

![image](https://user-images.githubusercontent.com/51786870/210558784-6de5fe68-f5f3-46ed-9401-a16194bf0271.png)
![image](https://user-images.githubusercontent.com/51786870/210558798-88c9bb09-18d9-41a0-b1bf-0dfb934bc100.png)

3. Wait about 30 seconds for the processing to begin

![image](https://user-images.githubusercontent.com/51786870/210558838-4479bda4-bb15-4359-8486-8f5ff23da8ca.png)

4. This process will take up to 30 seconds. Scroll to the bottom of the script and verify no issues.

**Module 1 is now complete.**



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
