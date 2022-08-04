# 8 Setting Up Zabbix Proxies

You can't preach about Zabbix without actually preaching about the use of Zabbix proxies—a nice addition at first, but a no-brainer by now. Anyone that's expecting to set up a Zabbix environment of a medium/larger size will need proxies. The main reason to use proxies is scalability, as Zabbix proxies offload the data collection and preprocessing load from the Zabbix server. This way we can scale up our Zabbix environment further and with greater ease.



In this chapter, we will first learn how to set up a Zabbix proxy. We will then learn how to work with passive and active Zabbix proxies, and also how to monitor hosts with either form of the Zabbix proxy. We will also cover some Zabbix network discovery using the proxies, and we'll learn how to monitor Zabbix proxies to keep them healthy. After these recipes, you'll have no more excuses for not setting up the proxies, as we'll cover most of the possible forms of proxy use in this chapter.



So, let's go through the following recipes and check out how to work with Zabbix proxies:



Setting up a Zabbix proxy



Working with passive Zabbix proxies



Working with active Zabbix proxies

Setting Up Zabbix Proxies



Monitoring hosts with a Zabbix proxy



Using discovery with Zabbix proxies



Monitoring your Zabbix proxies





**Technical requirements**



We are going to need several new Linux hosts for this chapter, as we'll be building them as Zabbix proxies.



Set up two proxies by installing CentOS (or your preferred Linux distribution) on the following two new hosts:



lar-book-proxy-passive



lar-book-proxy-active



You'll also need the Zabbix server, with at least one monitored host. We'll be using the following new host with a Zabbix agent installed:



lar-book-agent-by-proxy



**Setting up a Zabbix proxy**



Setting up a Zabbix proxy can be quite daunting if you don't have a lot of experience with Linux, but the task is quite simple once you get the hang of it. We will install a Zabbix proxy on our lar-book-proxy-passive server; you can repeat the task on



lar-book-proxy-active.



**Getting ready**



Make sure to have your new Linux host ready and installed. We won't need our Zabbix server in this recipe yet.



You'll need a Zabbix repository for this recipe as well. Check out the following link to find the latest version: https://www.zabbix.com/download.

Setting up a Zabbix proxy	291





**How to do it…**



Let's start by logging in to the **command-line interface** (**CLI**) of our new lar-book-proxy-passive host.



Now, execute the following command to add the Zabbix repository for CentOS 8:





**rpm -Uvh https://repo.zabbix.com/zabbix/6.0/rhel/8/ x86_64/zabbix-release-6.0-1.el8.noarch.rpm**





**dnf clean all**



For Ubuntu, execute this command:





**wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/ main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_ all.deb**





**dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb apt update**





Now, install Zabbix proxy by executing the following command. RHEL-based systems:

**dnf install zabbix-proxy-sqlite3**



Ubuntu systems:



**apt install zabbix-proxy-sqlite3**





**Tip**



On RHEL-based servers, don't forget to set **Security-Enhanced Linux**



(**SELinux**) to permissive or allow Zabbix proxy in SELinux for production. For lab environments it is fine to set SELinux to permissive, but in production I would recommend leaving it enabled. For Ubuntu systems, in a lab environment, we can disable AppArmor.



Now, edit the Zabbix proxy configuration by executing the following command: **vim /etc/zabbix/zabbix_proxy.conf**



Let's start by setting the proxy mode on the passive proxy. The mode will be 1 on this proxy. On the active proxy, this will be 0:



​	
 	

**ProxyMode=1**

Setting Up Zabbix Proxies



Change the following line to your Zabbix server address:



**Server=10.16.16.152**





**Important note**



When working with a Zabbix server in **High Availability** (**HA**), make sure to add the Zabbix server IP addresses here for every single node in your cluster. The proxy will only be sending data to the active node. Keep in mind that HA nodes are delimited by a semi-colon (;) instead of a comma (,).



Change the following line to your proxy hostname:



**Hostname=lar-book-proxy-passive**



As we'll be using the sqlite version of the proxy for the example, change the DBName parameter to the following:





**DBName=/tmp/zabbix_proxy.db**



9.	You can now enable Zabbix proxy and start it with the following two commands:





**systemctl enable zabbix-proxy**





**systemctl start zabbix-proxy**



You might want to check that the Zabbix proxy logs will restart, with the following command:





**tail -f /var/log/zabbix/zabbix_proxy.log**



**How it works…**



There are three versions of Zabbix proxy to work with:



zabbix-proxy-mysql



zabbix-proxy-pgsql



zabbix-proxy-sqlite3



We've just done the setup for the zabbix-proxy-sqlite3 package, which is the easiest method, if you ask me. The sqlite3 version of Zabbix proxy makes it possible for us to set up a Zabbix proxy with great ease as we don't actually need to worry too much about database setup.

Working with passive Zabbix proxies	293





Please do note that the sqlite3 versions might not be suited to Zabbix proxies with a lot of hosts and items. You get more options to scale a mysql or pgsql version of Zabbix proxy by the fine-tuning mechanisms installed with those database types.





**Tip**



Always pick the right type of Zabbix proxy for what you expect to need in the future. Although it is easy to switch proxies later, don't go too easy on this choice as you might save yourself hours in the future.



The amazing part about the sqlite3 version is that if we run into database issues, it's very easy to just run the following:



**rm -rf /tmp/zabbix_proxy.db**



Zabbix proxy then creates a new database on startup, and we're all ready to start collecting again. Do note that we might lose some information that is in the proxy database and that has not yet been sent to the Zabbix server.



**There's more…**



More information about installing Zabbix proxies can be found here:



[https://www.zabbix.com/documentation/current/manual/](https://www.zabbix.com/documentation/current/manual/installation/install_from_packages) [installation/install_from_packages](https://www.zabbix.com/documentation/current/manual/installation/install_from_packages)



Choose the distribution you are using, and you can find the guides for all the different variants of proxy installations.



**Working with passive Zabbix proxies**



Now that we have installed our Zabbix proxy in the previous recipe, we can start working with it. Let's start by setting up our passive Zabbix proxy in the frontend and see what we can do with it from the start.



**Getting ready**

​	
 	

​	You will need the lar-book-proxy-passive host for this recipe ready and installed with Zabbix proxy. We will also be using our Zabbix server in this recipe again.

294	Setting Up Zabbix Proxies





**How to do it…**



Let's start by logging in to our Zabbix frontend and navigating to **Administration** | **Proxies**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_f81aba824c58105f.jpg) 





























Figure 8.1 – Administration | Proxies page, no passive proxies



Our **Proxies** page is where we do all proxy-related configuration.



Let's add a new proxy with the blue **Create proxy** button in the top-right corner.



This will take us to the **Create proxy**page, where we will fill out the following information:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_5a4ff6eaa1b94b54.jpg) 

































Figure 8.2 – Administration | Proxies, Create proxy page, lar-book-proxy-passive

Working with passive Zabbix proxies	295





4.	Before clicking the blue **Add** button, let's take a look at the **Encryption** tab:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_6a0eefd194f2dc26.jpg) 































Figure 8.3 – Administration | Proxies, Create proxy Encryption page, lar-book-proxy-passive



5.	By default, **No encryption** is checked here, which we'll leave be for this recipe.





**Important note**



A lot of valuable information is exchanged between Zabbix servers and Zabbix proxies. If you are working with insecure networks or just need an extra layer of security, use Zabbix proxy encryption. You can find more

information on Zabbix encryption here: [https://www.zabbix.com/](https://www.zabbix.com/documentation/current/en/manual/encryption)



[documentation/current/en/manual/encryption](https://www.zabbix.com/documentation/current/en/manual/encryption)



Now, click the blue **Add** button, which will take us back to our proxy overview page.



The **Last seen (age)** part of your newly added proxy should now show a time value, instead of **Never**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_97b22a1f04008a66.jpg) 









Figure 8.4 – Administration | Proxies page, Last seen (age)



**How it works…**

​	
 	

​	Adding proxies isn't the hardest task after we've already done the installation part. After the steps we took in this recipe, we are ready to start monitoring with this proxy.

296	Setting Up Zabbix Proxies





The proxy we just added is a passive proxy. These proxies work by receiving configuration from the Zabbix server, which the Zabbix server sends to the Zabbix proxy on port 10051:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_2cfbef09cfa238a8.jpg) 

















Figure 8.5 – Diagram showing active proxy connection



Once the passive proxy knows what to monitor, every time the Zabbix server polls for data, data is sent back in the same TCP stream. This means that the connection is always initiated from the Zabbix server side. Once it's set up, the Zabbix server will keep sending configuration changes and it will keep polling for new data.



**Working with active Zabbix proxies**



We now know how to install and add proxies. Let's set up our active proxy, like we did with the passive proxy in the previous recipe, and see how it works.



**Getting ready**



You will need the lar-book-proxy-active host for this recipe, ready and installed with Zabbix proxy. We will also be using our Zabbix server in this recipe.



**How to do it…**



Let's start by logging in to our Zabbix frontend and navigating to **Administration** | **Proxies**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_a13a15ae6c1dcfc.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 8.6 – Administration | Proxies page, no active proxies

Working with active Zabbix proxies	297





Our **Proxies** page is where we do all configuration that's proxy-related.



Let's add a new proxy by clicking the blue **Create proxy** button in the top-right corner.



This will take us to the **Create proxy**page, where we will fill out the following information:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_6b19aac420695d64.jpg) 



































Figure 8.7 – Administration | Proxies, Create proxy page, lar-book-proxy-active





**Tip**



The **Proxy address** is actually optional for our active proxy. You do not have to add this for the Zabbix proxy to function, but it is recommended to keep things clear. Adding the Proxy address also functions as a sort of whitelist in this case, as only the IP address listed will be allowed to connect.



5.	Before clicking the blue **Add** button, let's take a look at the **Encryption** tab:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_de39c54d9bf984d1.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 8.8 – Administration | Proxies, Create proxy Encryption page, lar-book-proxy-active

Setting Up Zabbix Proxies



By default, **No encryption** is checked here, which we'll leave be for this recipe.



Now, click the blue **Add** button, which will take us back to the proxy overview page.



Log in to the CLI and check the configuration with the following command:





**vim /etc/zabbix/zabbix_proxy.conf**



By default, the frequency at which the proxy requests configuration changes is 3,600 seconds (that is, 1 hour):

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_40402ff6f465d8b2.jpg) 





















Figure 8.9 – Zabbix proxy configuration file, ConfigFrequency





**Tip**



For active Zabbix proxies, it is also possible to force the Zabbix proxy to request configuration data from the Zabbix server with zabbix_proxy -R config_cache_reload. Keep in mind that this will not work on a passive Zabbix proxy!



The **Last seen (age)** part of your newly added proxy should now show a time value, instead of **Never**.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_e5e33ba0a027a550.jpg) 











Figure 8.10 – Administration | proxies page, Last seen (age)



Depending on your setting in the proxy configuration file, the **Last seen (age)** part may take a while to change.





**Important note**



Polling the Zabbix server for configuration too often might increase load but polling too infrequently will leave your Zabbix proxy waiting for a new configuration. Choose a frequency that is best suited to your environment.

Monitoring hosts with Zabbix proxy	299





**How it works…**



If you followed the *Working with passive Zabbix proxies* recipe from this chapter, the steps are about the same, except for the part where we add the proxy mode and the part where we checked the ConfigFrequency value.



The proxy we just added is an active proxy that works by requesting a configuration from the Zabbix server on port 10051.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_64fe274ba3aa3022.jpg) 

















Figure 8.11 – Diagram showing active proxy connection



The Zabbix proxy keeps requesting configuration changes, and it keeps sending any new collected data to the Zabbix server every second or it sends out a heartbeat if no data is available.





**Important note**



It is recommended to use active Zabbix proxies, as we can use them to reduce the load on our Zabbix server. Use the passive proxy only when you have a good reason to do so.





**Monitoring hosts with Zabbix proxy**



We have our active and passive Zabbix proxies ready to use, so it's now time to add some hosts to them. Setting up the Zabbix frontend to monitor hosts with Zabbix proxies works in about the same way as monitoring directly from the Zabbix server. The backend and design change completely though, which I'll explain in the *How it works…* section of this recipe.



**Getting ready**

​	
 	

​	Make sure you have your lar-book-proxy-passive passive proxy and your lar-book-proxy-active active proxy ready by following all of the previous recipes in this chapter.

300	Setting Up Zabbix Proxies





You will also need your Zabbix server and at least two hosts to monitor. We will be using lar-book-agent_snmp and lar-book-agent in the example, but any host with an active and passive Zabbix agent will work.



**How to do it…**



We'll configure a host on both our active and our passive proxies to show you what the difference is between these two. Let's start with the passive proxy.



**Passive proxy**



Let's start off this recipe by logging in to our Zabbix frontend and navigating to **Configuration** | **Hosts**.



Let's add the host with the passive agent to our passive proxy. In my case, this is the lar-book-agent_snmp host.



Click on the lar-book-agent_snmp host and change the **Monitored by proxy** field to lar-book-proxy-passive, as in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_95301086868e32d1.jpg) 





























Figure 8.12– Configuration | Hosts, Edit host page for host lar-book-agent_snmp



Now, click on the blue **Update** button. Our host will now be monitored by the Zabbix proxy.





**Important note**



Due to the configuration update interval of 1 hour, we can see the SNMP icon turn gray temporarily, and it may take up to 1 hour before monitoring continues. Be patient or force a configuration cache reload on both the Zabbix server and Zabbix proxy.

Monitoring hosts with Zabbix proxy	301





**Active proxy**



Let's do the same for our other lar-book-agent host by navigating back to **Configuration** | **Hosts**.



Click on the lar-book-agent host and edit the **Monitored by proxy** field to lar-book-proxy-active, as in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_5323d0ff7c534a39.jpg) 

























Figure 8.13 – Configuration | Hosts, Edit host page for host lar-book-agent



Now, click on the blue **Update** button.



On the CLI of our monitored Linux host lar-book-agent host, execute the following command:





**vim /etc/zabbix/zabbix_agent2.conf**



When working with an active Zabbix agent we need to make sure to add the proxy IP address to the following line:





ServerActive=



Our host will now be monitored by the Zabbix proxy. Once again, this might take up to an hour.



**How it works…**



Monitoring hosts with a Zabbix proxy in passive or active mode works in the same way from the frontend. We merely configure which host is monitored by which proxy in our Zabbix frontend, and it will be done.

302	Setting Up Zabbix Proxies





Let's take a look at how our **Simple Network Management Protocol** (**SNMP**) agent is now monitored by the passive proxy:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_25d4273a7a5c598a.jpg) 









Figure 8.14 – A completely passive Zabbix setup with proxy



Our passive Zabbix proxy now collects data from our SNMP agent, and after this is collected, Zabbix server collects this data from our Zabbix proxy. Sounds like a whole process already, right?



Let's look at our active Zabbix proxy setup:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_3d6dcb5f54e58c1e.jpg) 









Figure 8.15 – A completely active Zabbix setup with proxy



Our active Zabbix proxy receives data from our active Zabbix agent and then sends this data to our Zabbix server. We've eliminated all the timers in this proxy setup altogether and are now receiving all of our data in the Zabbix server as soon as it's available.



This is why I would always recommend working with active proxies—and even active agents—as much as possible. If we look at the following screenshot, we can see a setup that you might see at a company:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_adf5144a7486a4b5.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 8.16 – An active Zabbix proxy setup with different monitored types

Monitoring hosts with Zabbix proxy	303





Fortunately, we have the option of using a lot of different combinations of setups. It is perfectly possible—and even logical—to combine your checks from a proxy, just as much as it would be with a Zabbix server. We can monitor all types from our proxy, whether it's a Zabbix agent, SNMP, or even **Java Management Extensions** (**JMX**) and the **Intelligent Platform Management Interface** (**IPMI**).





**Tip**



When designing a new Zabbix hosting infrastructure, start with adding proxies if possible. This way, you don't have to change a lot later. It's easy to add and change proxies, but it's harder to go from just using Zabbix server to using Zabbix proxies in your design.



**There's more…**



We now have a solid setup with some proxies up and running. We've figured out the difference between active and passive proxies and how they affect monitoring. But why would we build a setup like this? Well, Zabbix proxies are great for many environments— not just the big ones, but even sometimes in the smallest ones.



We can use Zabbix proxies to offload polling and preprocessing from our main Zabbix server, thus keeping the server clear for handling data like when writing to the Zabbix database.



We can use Zabbix proxies to monitor offsite locations, such as when you're a **Managed Service Provider** (**MSP**) and want to monitor a big customer network. We simply place a proxy on-site and monitor it. We can use industry-standard techniques like monitoring through a **virtual private network** (**VPN**) or simply set up a connection using built-in Zabbix proxy encryption.



When the VPN goes down, our proxy will keep collecting data on-site and send this to our Zabbix server when the VPN comes back up.



We can also use the Zabbix proxy to bypass firewall complications. When we place a proxy behind a firewall in a monitored network, we only need one firewall rule between the Zabbix server and the Zabbix proxy. Our Zabbix proxy then monitors the different hosts and sends the collected data in one stream to the Zabbix server.



With this, you have loads of options to use Zabbix proxies already.

304	Setting Up Zabbix Proxies





**See also**



Check out this interesting blog post by Dmitry Lambert about some more cool hidden benefits of Zabbix proxies: [https://blog.zabbix.com/hidden-benefits-of-](https://blog.zabbix.com/hidden-benefits-of-zabbix-proxy/9359/)[zabbix-proxy/9359/](https://blog.zabbix.com/hidden-benefits-of-zabbix-proxy/9359/).



Dmitry is an experienced Zabbix engineer and head of Zabbix Customer Support. His blog posts are easy to understand and provide some new angles to look at Zabbix from.



**Using discovery with Zabbix proxies**



In *Chapter 7*, *Using Discovery for Automatic Creation*, we talked about Zabbix and discovery. It is a very good idea to edit your discovery rules if you followed along with that chapter. Let's see how this would work in this recipe.



**Getting ready**



You'll need to have finished *Chapter 7*, *Using Discovery for Automatic Creation*, or have some discovery rules and active agent autoregistration set up.



I'll be using lar-book-lnx-agent-auto, lar-book-disc-lnx, and lar-book-disc-win hosts in this example. We will also need our Zabbix server.



**How to do it…**



Let's start with editing our discovery rule and then move on to editing our active agent to autoregister to the proxy.



**Discovery rules**



Starting with Zabbix discovery rules, let's look at how to make sure we do this from the



Zabbix proxy:



Log in to the CLI of lar-book-disc-lnx and edit the /etc/zabbix/ zabbix_agent2.conf file. Edit the following lines to include our Zabbix proxy address:





Server=127.0.0.1,10.16.16.152,10.16.16.160,10.16.16.161 ServerActive=10.16.16.160





2.	Restart your Zabbix agent by executing the following command:



​	
 	

**systemctl restart zabbix-agent2**

Using discovery with Zabbix proxies	305





Now, make sure to log in to lar-book-disc-win and edit the C:\Program Files\Zabbix agent\zabbix_agent2 file. Edit the following lines to include our Zabbix proxy address:





Server=127.0.0.1,10.16.16.152,10.16.16.160,10.16.16.161 ServerActive=10.16.16.160





**Important note**



On the ServerActive lines in our configuration files, make sure to only include the Zabbix proxy we want to send data to. The Zabbix agent will actively try to send data to all our Zabbix proxies or Zabbix servers listed here, so we should only list the one we want to use.



Restart your Zabbix agent by executing the following commands in the Windows command line:





**zabbix_agent2.exe --stop**





**zabbix_agent2.exe --start**



5.	Next, navigate to **Configuration** | **Hosts** and delete the discovered hosts:





lar-book-disc-lnx





lar-book-disc-win



We do this to prevent duplicate hosts.



Now, navigate to **Configuration** | **Discovery**.



Click on **Discover Zabbix Agent hosts** and change the **Discovered by proxy** field, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_a5fe7c0e84076caf.jpg) 





















Figure 8.17 – Configuration | Actions, drop-down menu for discovery by proxy lar-book-proxy-active

​	
 	

​	Click on the blue **Update** button, and that's all there is to editing your discovery rule to be monitored by a proxy.

Setting Up Zabbix Proxies



You can now check out your newly discovered hosts under **Configuration** | **Hosts** and see that they are monitored by the proxy:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_f36027c85fb67f26.jpg) 























Figure 8.18 – Configuration | Hosts screen for discovered hosts



**Active agent autoregistration**



Moving on to active agent autoregistration, let's see how we can do this from our



Zabbix proxy:



Start by navigating to **Configuration** | **Hosts** and deleting lar-book-lnx-agent-auto.



To do active agent autoregistration to a proxy, we have to log in to our lar-book-lnx-agent-auto host CLI.



Edit the Zabbix agent configuration file with the following command:





**vim /etc/zabbix/zabbix_agent2.conf**



Make sure to edit the following line to the Zabbix proxy address instead of the Zabbix server address:





ServerActive=10.16.16.160



5.	Restart the Zabbix agent:





**systemctl restart zabbix-agent2**

Monitoring your Zabbix proxies	307





We can now see our host autoregister to the Zabbix proxy instead of the Zabbix server:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_f36027c85fb67f26.jpg) 























Figure 8.19 – Configuration | Hosts screen for our two auto registered hosts



**How it works…**



Discovery with a Zabbix proxy works exactly the same as discovery with a Zabbix server. The only thing that changes is the location of where we are registering to or discovering from.



If you want to learn more about the process of discovery and auto registration, check out *Chapter 7*, *Using Discovery for Automatic Creation*, if you haven't already.



**Monitoring your Zabbix proxies**



A lot of Zabbix users—or even monitoring users in general—forget a very important part of their monitoring. They forget to monitor the monitoring infrastructure. I want to make sure that when you set up Zabbix proxies, you also know how to monitor the health of these proxies.



Let's check out how to do so in this recipe.



**Getting ready**



For this recipe, we will need our new lar-book-proxy-active Zabbix proxy. We will also need our Zabbix server to monitor the Zabbix proxy.

308	Setting Up Zabbix Proxies





**How to do it…**



We are going to build some monitoring in our Zabbix frontend, but we'll also check the integrated monitoring options for Zabbix proxies. Let's start by building our own.



**Monitoring the proxy with Zabbix**



We can monitor our Zabbix proxy with Zabbix proxy itself to make sure we know exactly what's going on:



Let's start by logging in to our lar-book-proxy-active Zabbix proxy CLI.



Issue the following command to install Zabbix agent 2 for RHEL-based systems:





**dnf install zabbix-agent2**



For Ubuntu, issue this command:





**apt install zabbix-agent2**



Edit the Zabbix agent configuration file by issuing the following command: **vim /etc/zabbix/zabbix_agent2.conf**



Edit the following lines to point toward localhost:





Server=127.0.0.1





ServerActive=127.0.0.1



Also, make sure to add the hostname to the Zabbix agent file:



**Hostname=lar-book-proxy-active**



Now, log in to the Zabbix frontend and navigate to **Configuration** | **Hosts**.



Monitoring your Zabbix proxies	309





Click on the blue **Create host** button in the top-right corner and add the following host:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_d87844f4c38d641.jpg) 











































Figure 8.20 – Configuration | Hosts, Create host page, lar-book-proxy-active



Take extra care at the **Monitored by proxy** field—we want to monitor from the proxy because we are doing Zabbix internal checks, which need to be handled by the Zabbix daemon that received this configuration.



Before clicking the blue **Add** button, make sure to add the **Templates**. Add the following templates to the host:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_d82688299459c7bd.jpg) 























Figure 8.21 – Configuration | Hosts, Create host page Templates tab for host lar-book-proxy-active

​	
 	

\10. We can now click the blue **Add** button to create the host.

Setting Up Zabbix Proxies



Now, navigate to **Monitoring** | **Latest data** and add the following filters:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_8b640b78da5a7900.jpg) 



















Figure 8.22 – Monitoring | Latest data page with filters, host lar-book-proxy-active



We can now see our Zabbix proxy statistics, such as **Number of processed values per second** and **Utilization of configuration syncer internal processes**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_abc81719d96f9d78.jpg) 







































Figure 8.23 – Monitoring | Latest data page with data from our Zabbix proxy



**Monitoring the proxy remotely from our Zabbix server**



We can also monitor our Zabbix proxy remotely from our Zabbix server, so let's see how that works:



Let's start by logging in to our lar-book-proxy-active host CLI and editing the following file:



​	
 	

**vim /etc/zabbix/zabbix_agent2.conf**

Monitoring your Zabbix proxies	311





Edit the following lines to match your Zabbix server address (every node in an HA cluster):





Server=127.0.0.1,10.16.16.152 ServerActive=127.0.0.1,10.16.16.152





3.	Also, edit the following file:





**vim /etc/zabbix/zabbix_proxy.conf**



Edit the following line to match your Zabbix server address:



StatsAllowedIP=127.0.0.1,10.16.16.152



Now, navigate to the Zabbix frontend and go to **Configuration** | **Hosts**.



Click on the blue **Create host** button in the top-right corner and add the following host:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_58e9508a838afdcd.jpg) 









































Figure 8.24 – Configuration | Hosts, Create host page, lar-book-proxy-active_remotely

312	Setting Up Zabbix Proxies



​	
 	

​	Before clicking the blue **Add** button, make sure to add the right **Templates**. Add the following templates to the host:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_613c6b1f612e97a8.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 8.25 – Configuration | Hosts, create new host page, Templates tab, lar-book-proxy-active_ remotely


 	

​	We can now click the blue **Add** button to create the host.


 	

​	Back at **Configuration** | **Hosts**, click on your new lar-book-proxy-active_ remotely host.


 	

​	Go to **Macros** and add the following two macros:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_89dbff61c255cdb4.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 8.26 – Configuration | Hosts, Edit host page, Macros tab, lar-book-proxy-active_remotely


 	

​	Now, click on the blue **Update** button, and that's it for this host.


 	

​	If we navigate to **Monitoring** | **Latest data** and check the items for this host, we can see data coming in:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_2dc4e79a09f4fcaa.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 8.27 – Monitoring | Latest data page for host lar-book-proxy-active_remotely

Monitoring your Zabbix proxies	313





**Monitoring the proxy from the Zabbix frontend**



Let's start this off by navigating to **Administration** | **Queue**.



Use the drop-down menu to go to **Queue overview by proxy**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_a1518ffed02c847b.jpg) 





















Figure 8.28 – Administration | Queue page drop-down menu



This will bring us to the page shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_47e81688e9af4b47.jpg) 















Figure 8.29 – Administration | Queue page



**How it works…**



Monitoring your Zabbix proxies is an important task, thus we need to make sure that whenever we add a new Zabbix proxy, we are taking care of it like we would any other host.



**Monitoring the proxy with Zabbix**



By adding the Zabbix proxy as a host, we can make sure the Linux system that is running our Zabbix proxy is healthy. We also make sure that the Zabbix proxy applications running on this server are in good health.



Besides having the right triggers in these templates, we also get a load of options to troubleshoot issues with Zabbix proxy.



Zabbix proxy works just like Zabbix server when it comes to monitoring. This means that just as with Zabbix server, we need to keep the proxies in great health by tweaking the Zabbix proxy configuration file to our needs.

314	Setting Up Zabbix Proxies





Scaling your proxies becomes a lot easier once you figure out what's going on with them. So, this is why we make sure to always monitor them. We monitor them from the proxy itself to make sure that we get the right information with the Zabbix internal checks.



**Monitoring the proxy remotely from our Zabbix server**



Now, when we monitor with the **Remote Zabbix proxy health**, things go a little differently. Instead of doing our checks from the Zabbix proxy itself, we do them remotely from the Zabbix server by defining the Zabbix proxy address and port in the macros. The Zabbix internal check type will still be used for this, executing the checks from the Zabbix server to the Zabbix proxy remotely in this case.



On top of that, it is of course still recommended that we also keep our Zabbix agent running in either passive or active mode.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc0p7_tmp_112748d22a51c9bb.jpg) 













Figure 8.30 – Zabbix agent running on Zabbix proxy monitored by Zabbix server



This way, our Zabbix server is the one requesting and receiving information. Even when the proxy is having issues, the checks will still be done by Zabbix server.



We can use this template as a way to keep a closer eye on our proxy if we suspect issues with internal checks being performed locally, or we can use this template to bypass certain firewall setups. Both are valid reasons.



**Monitoring the proxy from the Zabbix frontend**



From the frontend, we can use the **Administration** | **Queue** page to monitor our proxies. The Zabbix **Queue** page is an important page, but a lot of new users neither know nor fully utilize this page.



When a part of Zabbix starts performing poorly, such as our example Zabbix proxy here, that's when we can see stuff happening in the queue. There are six options in the Zabbix **Queue**:



**5 seconds**



**10 seconds**



**30 seconds**

Monitoring your Zabbix proxies	315





**1 minute**



**5 minutes**



**More than 10 minutes**



What the options in the **Queue** mean is that the Zabbix proxy has been waiting on receiving a value that's configured more than expected. I would state that anything up to 1 minute doesn't necessarily have to be an issue, but this depends on the type of check. The **5 minutes** or **More than 10 minutes** options can mean serious performance issues with your Zabbix proxy, and you would have to troubleshoot this issue. Make sure to keep a good eye on the Zabbix queue when you suspect issues, which are also included as triggers in the templates we added to monitor our Zabbix proxies.