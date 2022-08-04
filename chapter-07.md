# 7 Using Discovery for Automatic Creation

This chapter is going to be all about making sure that you as a Zabbix administrator are doing as little work as possible on host and item creation. We are going to learn how to perform (or perfect, maybe) automatic host creation. Check out the recipes featured here to see just what we are going to discover.

In this chapter, we will first learn how to set up Zabbix discovery with Zabbix agent and **Simple Network Management Protocol** (**SNMP**). We will then set up active agent autoregistration. Later, we will also cover the automatic creation of Windows performance counters and **Java Managements Extension** (**JMX**) items.



In this chapter, we will cover the following recipes:



Setting up Zabbix agent discovery



Setting up Zabbix SNMP discovery



Working with active agent autoregistration



Using the new Windows performance counter discovery



Discovering JMX objects

258	Using Discovery for Automatic Creation





**Technical requirements**



As this chapter is all about host and item discovery, besides our Zabbix server we will need one new Linux host and a Windows host. Both these hosts will need Zabbix agent 2 installed, but not configured just yet.



Furthermore, we are going to need our JMX host, as configured in *Chapter 3*, *Setting Up Zabbix Monitoring* , and a new host with SNMP set up. To learn more about setting up an SNMP-monitored host, check out the *Working with SNMP monitoring* recipe in *Chapter 3*, *Setting Up Zabbix Monitoring*.



**Setting up Zabbix agent discovery**



A lot of Zabbix administrators use Zabbix agent extensively and thus spend a lot of time creating Zabbix agent hosts by hand. Maybe they don't know how to set up Zabbix agent discovery, maybe they didn't have time to set it up yet, or maybe they just prefer it this way. If you are ready to get started with Zabbix agent discovery, in this recipe we will learn just how easy it is to set it up.



**Getting ready**



Besides our Zabbix server, in this chapter's introduction I mentioned that we will need two (empty) hosts with Zabbix agent 2 installed: one Windows host and one Linux host. If you don't know how to install Zabbix agent 2, check out *Chapter 3*, *Setting Up Zabbix Monitoring*, or see the Zabbix documentation at [https://www.zabbix.com/](https://www.zabbix.com/documentation/current/en/manual/concepts/agent2)



[documentation/current/en/manual/concepts/agent2](https://www.zabbix.com/documentation/current/en/manual/concepts/agent2).



Let's give the servers the following hostnames:



lar-book-disc-lnx: For the Linux host (use Zabbix agent 2)



lar-book-disc-win: For the Windows host (use Zabbix agent 2)



**How to do it…**



Let's get started by logging in to our lar-book-disc-lnx Linux host and editing the following file:





**vim /etc/zabbix/zabbix_agent2.conf**



Now, make sure your Zabbix agent 2 configuration file contains at least the following line:



​	
 	

Hostname=lar-book-disc-lnx

Setting up Zabbix agent discovery	259





For your Windows Zabbix agent, it's important to do the same. Edit the following file:





C:\Program Files\Zabbix Agent 2\zabbix_agent2.conf



Now, change the hostname by editing the following line:



Hostname=lar-book-disc-win



Next up, in our Zabbix frontend, navigate to **Configuration** | **Discovery**, and on this page, we are going to create a rule with the following settings:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_a054f5740f4ba98f.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 7.1 – Discovery rules page for Zabbix agent hosts

260	Using Discovery for Automatic Creation



​	
 	

**Important note**


 	

​	We are using an **Update interval** of 1 minute in this example. As this might take up a lot of resources on your server, make sure to adjust this value for your production environment. For example, 1 hour is a much better production value.


 	

​	Click the blue **Add** button to move on.


 	

​	After setting up the discovery rule, we will also need to set up an action to actually create the host with the right template. Navigate to **Configuration** | **Actions** | **Discovery actions**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_3c2894d4840171c0.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 7.2 – Main-menu at Configuration | Actions


 	

​	Here, we will click the **Create action** button in the top-right corner and fill out the next page with the following information:

Setting up Zabbix agent discovery	261

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_a49e72edc544275b.png) 













































Figure 7.3 – Create Discovery action page for Zabbix agent hosts





**Tip**



When creating Zabbix actions, it's important to keep the order of creation for **Conditions** in mind. The labels seen in the preceding screenshot will be added in order of creation. This means that it's easier to keep track of your Zabbix actions if you keep the order of creation the same for all actions.



9.	Next up, click the **Operations** tab. This is where we will add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_c06ab01206b737d9.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.4 – Create Discovery action | Operations page for Zabbix agent hosts

262	Using Discovery for Automatic Creation



​	
 	

​	That's it for the Linux agent. Click the blue **Add** button, and let's continue to our Windows discovery rule.


 	

​	Navigate to **Configuration** | **Host groups**. Create a host group for our Windows hosts by clicking **Create host group** in the top-right corner and filling out the group name:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_b43925ce80ffba17.jpg) 
 	


 	


 	


 	


 	


 	


 	

​	Figure 7.5 – Create host group page for Windows server hosts


 	

​	Click the blue **Add** button and navigate to **Configuration** | **Actions**.


 	

​	Go to **Discovery actions** again and click **Create action**. We will fill out the same thing but for our Windows hosts this time:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_7948838f3b2d4d7b.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.6 – Create Discovery action page for Windows Zabbix agents

Setting up Zabbix agent discovery	263





Before clicking **Add**, let's also fill out the **Operations** page with the **Operations** shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_147f41df9300bf14.jpg) 

































Figure 7.7 – Create Discovery action Operations page for Windows Zabbix agents



Now, we can click the blue **Add** button, and our second discovery action is present.



Move on to **Monitoring** | **Discovery**. This is where we can see if and when our hosts are discovered:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_7b2c60dde423934.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 7.8 – Monitoring | Discovery page

264	Using Discovery for Automatic Creation





**Tip**



Use the **Monitoring** | **Discovery** page to keep a close eye on the hosts you



expect to show up in your Zabbix setup. It's very useful to track new hosts



coming in and see which Zabbix discovery rule was used to create the host.





**How it works…**



Network discovery might not be very hard to set up initially, but there are loads of options to configure. For this example, we made the choice to use the agent.hostname key as our check. We create the Zabbix hostname based on what's configured in the Zabbix agent config file.



What happens is that Zabbix network discovery finds our hosts and performs our check. In this case, the check is: *What is the hostname used by the Zabbix agent?* This information, plus our IP address, is then triggering the action. Our action then performs our configured checks:



Does the hostname contain lnx or win?



Is the discovery status UP?



Is the service type Zabbix Agent?



If all of those checks are true, our action will then create our newly discovered host with the following:



Our configured host group plus the default Discovered hosts host group



Our template as configured in our action



We will end up with two newly created hosts, with all the right settings:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_1127335ef129fc94.jpg) 

















Figure 7.9 – Configuration | Hosts page with our new hosts, Windows and Linux

Setting up Zabbix SNMP discovery	265





**There's more…**



Creating the host by using the configuration file settings isn't always the right way to go, but it's a solid start to working with network discovery.



If you want a more flexible environment where you don't have to even touch the Zabbix agent configuration file, then you might want to use different checks on the discovery rule. Check out which keys we can use to build different discovery rules in the Zabbix



documentation at [https://www.zabbix.com/documentation/current/en/](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix_agent) [manual/config/items/itemtypes/zabbix_agent](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix_agent).



**Setting up Zabbix SNMP discovery**



If you work with a lot of SNMP devices but don't always want to set up monitoring manually, network discovery is the way to go. Zabbix network discovery uses the same functionality as Zabbix agent discovery, but with a different configuration approach.



**Getting ready**



To get started with network discovery, we are going to need a host that we can monitor with SNMP. If you don't know how to set up a host such as this, check out the *Working with SNMP monitoring* recipe in *Chapter 3*, *Setting Up Zabbix Monitoring*. We'll also need our Zabbix server.



**How to do it…**



First, log in to your new SNMP-monitored host and change the hostname to the following:





**hostnamectl set-hostname lar-book-disc-snmp**



2.	Then, restart the SNMP daemon using the following command:





**exec bash**





**systemctl restart snmpd**



Now, navigate to **Configuration** | **Discovery** and click on **Create discovery rule** in the top-right corner.

266	Using Discovery for Automatic Creation



​	
 	

​	We are going to create a new SNMP discovery rule, with an SNMP **Object Identifier** (**OID**) check type. Fill out the **Name** and **IP range** fields first, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_674999b075f8067b.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.10 – Configuration | Discovery, discovery rule creation page for SNMPv2 agents


 	

​	Make sure to fill out your own IP range in the **IP range** field.


 	

​	Now, we are going to create our SNMP check. Click on **Add** next to **Checks**, and you'll see the following pop-up screen:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_7ec76384d7b877f0.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.11 – Configuration | Discovery, discovery check creation pop-up window


 	

​	We want our **Check type** to be **SNMPv2 agent** and we want to fill it with our community and a useful OID, which in this case will be the OID for the system name. Fill it out like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_e123f79f8814b33e.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 7.12 – Configuration | Discovery, discovery check creation pop-up


 	

​	window filled for SNMPv2 agents

Setting up Zabbix SNMP discovery	267





**Important note**



Please note that our check type is *not* SNMP version-independent. We have three SNMP versions and thus three different check types to choose from, unlike with our new SNMP interface selection on the Zabbix 6 host screen.



8.	After clicking **Add** again, fill out the rest of the page, as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_9c3dabb108001241.jpg) 



































































Figure 7.13 – Configuration | Discovery page for SNMPv2 agents



Last, but not least, click the **Add** button at the bottom of the page. This concludes creating our Zabbix discovery rule.



We will also need an action for creating our hosts from the discovery rule. Navigate to **Configuration** | **Actions**, and after using the dropdown to select **Discovery actions**, click on **Create action**.

268	Using Discovery for Automatic Creation



​	
 	

\11. We will fill out the page with the following information:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_cfa3c7925b1e2a26.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.14 – Configuration | Actions, action creation page for SNMPv2 agents


 	

​	Before clicking **Add**, navigate to **Operations** and fill out this page with the following details:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_decf933c7fc76e33.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.15 – Configuration | Actions, action creation Operations tab for SNMPv2 agents

Setting up Zabbix SNMP discovery	269





Now, click **Add** and navigate to **Monitoring** | **Discovery** to see if our host gets created:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_c443654d2f3f5775.jpg) 







































Figure 7.16 – Monitoring | Discovery page for SNMPv2 agents



**How it works…**



In this recipe, we've created another discovery rule, but this time for SNMP. As you've noticed, the principle remains the same, but the application is a bit different.



When we created this Zabbix discovery rule, we gave it two checks instead of the one check we did in the previous recipe. We created one check on SNMP OID



.1.3.6.1.2.1.1.5.0 to retrieve the hostname of the device through SNMP. We then put the hostname retrieved from the system into Zabbix as the Zabbix hostname of the system.



We also created a check on SNMP OID .1.3.6.1.2.1.25.1.4.0. This check will retrieve the following string, if present:





"BOOT_IMAGE=(hd0,gpt2)/vmlinuz-4.18.0-193.6.3.el8_2.x86_64 root=/dev/mapper/cl-root ro crashkernel=auto resume=/dev/ mapper/cl-swa"



If the string is present, it means that the boot image is Linux on this host. This is a perfect example of how we can retrieve multiple OIDs to do multiple checks in our Zabbix discovery rules. If we'd been monitoring a networking device, for instance, we could have picked an OID to see if it was a Cisco or a Juniper device. We would replace



.1.3.6.1.2.1.25.1.4.0 with any OID and poll it. Then, we would create our action based on what we received (Juniper or Cisco) and add our templates accordingly.

270	Using Discovery for Automatic Creation





**Important note**



General knowledge of SNMP structure is very important when creating Zabbix discovery rules. We want to make sure we use the right SNMP OIDs as checks. Make sure to do your research well, utilize SNMP walks, and plan out what OIDs you want to use to discover SNMP agents. This way, you'll end up with a solid monitoring infrastructure.





**Working with active agent autoregistration**



Using discovery to set up your Zabbix agents is a very useful method to automate your host creation. But what if we want to be even more upfront with our environment and automate further? That's when we use a Zabbix feature called **active agent autoregistration**.



**Getting ready**



For this recipe, we will need a new Linux host. We will call this host lar-book-lnx-agent-auto. Make sure to install the Zabbix agent 2 to this host. Besides this new host, we'll also need our Zabbix server.



**How to do it…**



Let's start by logging in to our new lar-book-lnx-agent-auto host and change the following file:





**vim /etc/zabbix/zabbix_agent2.conf**



We will then edit the following line in the file. Make sure to enter your Zabbix server IP on this line:





ServerActive=10.16.16.152



We can also change the following line in the file if we want to set our hostname in the file manually:





Hostname=lar-book—lnx-agent-auto



This is not a requirement though, as Zabbix agent will use the system hostname if it is not filled out.

Working with active agent autoregistration	271





Next up, we will navigate to our Zabbix frontend, where we'll go to **Configuration** | **Actions**.



Use the drop-down menu to go to **Autoregistration actions**, as in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_ae72c5454a150d8c.jpg) 

























Figure 7.17 – Configuration | Actions page drop-down menu



Now, we will click the blue **Create action** button in the top-right corner to create a new action.



Fill out **Name** and then click on the **Add** text link:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_b0f51bcc7ffe1b05.jpg) 



































Figure 7.18 – Configuration | Actions, create new action page

272	Using Discovery for Automatic Creation



​	
 	

​	We can set up a condition here to only register hosts with a certain hostname. Let's do this by filling out the window like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_add41e5034d7f501.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.19 – Create action page, New condition window for host lar-book-lnx


 	

Your page should now look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_d4773953f5c77430.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.20 – Create action page, filled with our information for host lar-book-lnx


 	


 	

**Tip**


 	

​	We can set up conditions for different types of hosts. For instance, if we want to add Windows hosts, we set up a new action with a different hostname filter. This way, it is easy to maintain the right groups and templates, even with autoregistration.

​	
 	

9.	Before clicking the blue **Add** button, let's go to the **Operations** tab.

Working with active agent autoregistration	273





\10. Click on the **Add** text link, and you will see the following window:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_b08bbe91ecc3a92a.jpg) 





















































Figure 7.21 – Create action operations page, new operation window, Send message for host lar-book-lnx



\11. Create an action to add the host to the **Linux servers** host group:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_91734b8c068de3a2.jpg) 





























Figure 7.22 – Create action operations page, new operation window, Add to host group, lar-book-lnx

Using Discovery for Automatic Creation



Create an action to add the host to the Template OS Linux by Zabbix agent active template:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_15591ab9833fe06.jpg) 

























Figure 7.23 – Create action operations page, new operation window, Link to template, lar-book-lnx



Your finalized **Operations** page should now look like this, and we can click the blue **Add** button:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_60a01565230e63a6.jpg) 

































Figure 7.24 – Create action operations page, filled with our information, lar-book-lnx



Navigate to **Configuration** | **Hosts**, and we can see our new active autoregistered host:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_a714893edb1f2083.jpg) 









Figure 7.25 – Configuration | Hosts page with host lar-book-lnx-agent-auto

Working with active agent autoregistration	275





**How it works…**



Active agent autoregistration is a solid method to let a host register itself. Once the ServerActive= line is set up with the Zabbix server IP, the host agent will start requesting configuration data from the Zabbix server. The Zabbix server will receive these requests, and if there is an action set up in Zabbix (as we just did in this recipe), the host autoregisters to Zabbix:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_384ef0dd88a0272a.jpg) 





































Figure 7.26 – Host autoregistration process



We can do a bunch of cool automations with this functionality. We could create a script to fill up our Zabbix agent configuration file with the right ServerActive= line on our hosts in a certain IP pool.



It would also be super-easy to set up new hosts with Ansible. We can automate the Zabbix agent installation with Ansible and we can add the ServerActive= line in the /etc/zabbix/zabbix_agent2.conf file using Ansible as well. Our Zabbix server autoregistration action will take care of the rest from here.



Zabbix agent autoregistration is a perfect way to get a zero-touch monitoring environment that's always up to date with our latest new hosts.



**There's more…**



Not every company uses hostnames that reflect the machine's OS or other attributes. This is when Zabbix HostMetadata can come in very useful. We can add this field to the active Zabbix agent configuration to reflect the attributes of the machine.



Afterward, we can use this HostMetadata in our Zabbix discovery action to do the same kind of filtering we did on the hostname.

276	Using Discovery for Automatic Creation





We also have the **HostInterface** and **HostInterfaceItem** parameters in the Zabbix agent configuration file, which are used for autoregistration. The host will use the specified IP or DNS name as its Zabbix agent interface IP or DNS, as seen in the Zabbix frontend. We can also use this functionality to enable passive agent monitoring while using autoregistration to create the host.



Check out this link for more information:



[https://www.zabbix.com/documentation/current/manual/discovery/](https://www.zabbix.com/documentation/current/manual/discovery/auto_registration#using_host_metadata) [auto_registration#using_host_metadata](https://www.zabbix.com/documentation/current/manual/discovery/auto_registration#using_host_metadata)



**Using the new Windows performance counter discovery**



In Zabbix 6, it is possible to discover Windows performance counters. In this recipe, we will go over the process of discovering Windows performance counters to use in our environments.



Discovering Windows performance counters might seem to be a little tricky at first, as it uses both Windows- and Zabbix-specific concepts. But once we finish this recipe you'll know exactly how to set it up.



**Getting ready**



In this chapter, we added the lar-book-disc-win host to our setup, which is the host used in our Zabbix agent discovery process. We can reuse this host to discover Windows performance counters easily.



Of course, we'll also need our Zabbix server.



**How to do it…**



Let's start by navigating to **Configuration** | **Templates** and creating a new template by clicking **Create template** in the top-right corner.



Create the following template:

Using the new Windows performance counter discovery	277

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_fb207d9c5b5c485d.png) 

















































Figure 7.27 – Configuration | Templates, Create template page, Windows performance by Zabbix agent



Click on the blue **Add** button, which will bring you back to **Configuration** | **Templates**. Select the new template.



Now, before continuing with our template, navigate to your Windows frontend and open perfmon.exe:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_5f763e5924ed1882.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 7.28 – Windows search bar, perfmon.exe

278	Using Discovery for Automatic Creation



​	
 	

5.	This will open the following window:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_3e77a784644e101d.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 7.29 – Windows perfmon.exe


 	

​	Let's click on **Performance Monitor** and then on the green + icon. This will show you all the available Windows performance counters.


 	

​	Let's start by using the **Processor** counter.


 	

​	Go back to the **Configuration** | **Template** page in Zabbix and edit our new **Template Module Windows performance by Zabbix agent** template.


 	

​	When you are at the **Edit template** page, click on **Discovery rules** in the bar next to your template name.

Using the new Windows performance counter discovery	279





Click on **Create new discovery rule** in the top-right corner and add the following rule:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_e6e94dd03c398020.jpg) 



















































Figure 7.30 – Create Low-Level Discovery (LLD) rule page, Discover counter Processor





**Important note**



We are using an **Update interval** of 1 minute in this example. As this might take up a lot of resources on your server, make sure to adjust this value to your production environment. For example, 1 hour is a much better production value.



Click the blue **Add** button at the bottom and click our new Discover counter Processor discovery rule.

280	Using Discovery for Automatic Creation



​	
 	

​	Click on **Item prototypes**, and in the top-right corner click on **Create item prototype**. We will then create the following item prototype:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_6a573d0eb39331d4.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.31 – Create LLD item prototype page, CPU instance C1 time


 	

\13. On the **Tags** tab, do not forget to add some new tags as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_9638b8008db5c75e.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.32 – Create LLD item prototype page tag tab, CPU instance C1 time


 	

​	Save the new **Item prototype**, go to **Configuration** | **Hosts**, and click on lar-book-disc-win.

Using the new Windows performance counter discovery	281





Add our **Windows performance by Zabbix agent** template:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_fc8f807912fbdd2d.jpg) 



































Figure 7.33 – Create LLD item prototype page, add Templates for lar-book-disc-win



After clicking on the blue **Update** button, we can navigate to **Monitoring** | **Latest data**. Add the following filters:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_f95502b9cb557dcf.jpg) 

































Figure 7.34 – Latest data filter on host lar-book-disc-win

Using Discovery for Automatic Creation



We can now see our three newly created items:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_272384af88d6aff8.jpg) 





















Figure 7.35 – Monitoring | Latest data page for our host lar-book-disc-win



**How it works…**



Windows performance counters have been around for a long time and they are very important to anyone who wants to monitor Windows machines with Zabbix. Using **Low-Level Discovery** (**LLD**) in combination with Windows performance counters makes it a lot easier and more flexible to build solid Windows monitoring.



In this recipe, we created a very simple but effective Windows performance counter discovery rule by adding the discovery rule with the perf_instance. discovery[Processor] item key. The [Processor] part of this item key directly correlates to the perfmon.exe window we saw. If we look at the following screenshot, we already see **Processor** listed:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_864c547d963259b0.jpg) 

































Figure 7.36 – perfmon.exe | Add Counters, Processor



When our discovery rule polls this item key, Zabbix agent will return the following value for our host:



​	
 	

​	[{"{#INSTANCE}":"0"},{"{#INSTANCE}":"1"},{"{#INSTANCE}":"_ Total"}]

Using the new Windows performance counter discovery	283





This value means that Zabbix will fill the{#INSTANCE} macro with three values:



0



1



_Total



We can then use these three values by using the {#INSTANCE} macro in our **Item prototype**, like we did here:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_458ffc09cd2ae081.jpg) 











Figure 7.37 – Our created Item prototype, CPU C1 time



It will then create three items with our macro values, with the right keys to monitor the second part of our counter—% C1 time. If you expand the window in your perfmon. exe file, you can see all the different counters we could add to our item prototypes to monitor more Windows performance counters:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_8abae0a0a30c224d.jpg) 











































Figure 7.38 – Perfmon.exe | Add Counters, Processor expanded

284	Using Discovery for Automatic Creation





**Discovering JMX objects**



In *Chapter 3*, *Setting Up Zabbix Monitoring*, we went over setting up JMX monitoring in the recipe titled *Setting up JMX monitoring*. What we didn't cover yet though was discovering JMX objects.



In this recipe, we will go over how to set up JMX objects with LLD, and after you've finished this recipe, you'll know just how to set it up.



**Getting ready**



For this recipe, we will need the JMX host that you set up for the *Setting up JMX monitoring* recipe in *Chapter 3*, *Setting Up Zabbix Monitoring*. Make sure to finish that recipe before working on this one.



We will also need our Zabbix server with our Zabbix JMX host titled lar-book-jmx.



**How to do it…**



Let's start this recipe off by logging in to our Zabbix frontend and navigating to **Configuration** | **Templates**.

Create a new template by clicking on **Create template** in the top-right corner. Fill in the following fields:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_f48ef8465c668c39.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.39 – Configuration | Templates, Create new template page, Apache Tomcat JMX discovery

Discovering JMX objects	285





After clicking the blue **Add** button, you will be taken back to **Configuration** | **Templates**. Click on your new **Template App Apache Tomcat JMX discovery** template.



We will now add our JMX discovery rule. Click on **Discovery rules** next to our template name.



Now, click on **Create discovery rule** and fill in the following fields:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_76aad27a7be55d9.jpg) 





















































Figure 7.40 – Configuration | Templates, create new template page, Discover JMX object MemoryPool



Click on the blue **Add** button at the bottom of the page. Then, click on **Item prototypes** next to your newly created **Discover JMX object MemoryPool** discovery rule.

286	Using Discovery for Automatic Creation



​	
 	

​	We will now click on the **Create item prototype** button in the top-right corner and create the following item prototype:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_d5bb2728dc1f0f7a.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.41 – Item prototype creation page, MemoryPool Memory type


 	

​	Also make sure that on the **Tags** tab you add a new tag as component | memory pool:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_51d73b8244f05aa2.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.42 – Item prototype creation page tag tab, MemoryPool Memory type

​	
 	

9.	Let's click on the blue **Add** button and move on.

Discovering JMX objects	287





Go to **Configuration** | **Hosts** and click on lar-book-jmx. We will add our template to this host.

Click on **Templates** and add the template, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_b68cc67bafd4c194.jpg) 































Figure 7.43 – Configuration | Hosts, add template to host lar-book-jmx



Click on the blue **Update** button.



When we navigate to **Monitoring** | **Latest data** now, we will filter on **Hosts** | lar-book-jmx and **Tag** | component with **Value** | memory pool, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_90bd67b70e97c73c.jpg) 





















Figure 7.44 – Monitoring | Latest data page filters, host lar-book-jmx



\14. We will then see the following results:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_41bff12e3b8d5cc6.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 7.45 – Monitoring | Latest data page for host lar-book-jmx with our results

288	Using Discovery for Automatic Creation





**How it works…**



Monitoring JMX applications can be quite daunting at first, as there is a lot of work to figure out while building your own LLD rules. But now that you've built your first LLD rule for JMX, there is a clear structure in it.



First, for our discovery rule, we've picked the item key:





jmx.discovery[beans,"*:type=MemoryPool,name=*"]



MemoryPool is what we call an **MBean object** in Java. We poll this MBean object for several JMX objects and fill the macros accordingly.



We picked the name=* object to fill the {#JMXNAME} macro in this discovery rule.

Our macro is then used in our item prototype to create our items.



Our items are then created, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc082_tmp_a55ab4230ebefc76.jpg) 





















Figure 7.46 – Items on our JMX-monitored host



If we look at the keys of the items, we can see that we poll the **Type** JMX attribute on every MemoryPool with different names.



That's how we create JMX LLD rules with ease.



**There's more…**



If you are not familiar with MBeans objects, then make sure to check out the Java documentation. This will explain to you a lot about what MBeans are and how they can be used for monitoring JMX attributes: [https://docs.oracle.com/javase/](https://docs.oracle.com/javase/tutorial/jmx/mbeans/index.html)

[tutorial/jmx/mbeans/index.html](https://docs.oracle.com/javase/tutorial/jmx/mbeans/index.html)





**Tip**



Before diving deeper into using JMX object discovery, dive deeper into the preceding JMX object documentation. There's a lot of information in it and it will greatly improve your skills in creating these LLD rules.