# Setting Up Zabbix Monitoring


Zabbix is built to be flexible and should be able to monitor just about anything you could ever require. In this chapter, we will learn more about working with Zabbix to build a lot of different options for monitoring. We'll go over them recipe by recipe, so you'll end up with a solid understanding of how they work. We'll cover the following recipes on the different monitoring types:

     Setting up Zabbix agent monitoring
    
     Working with SNMP monitoring
    
     Creating Zabbix simple checks and Zabbix trapper
    
     Working with calculated and dependent items
    
     Creating external checks
    
     Setting up JMX monitoring
    
     Setting up database monitoring
    
     Setting up HTTP agent monitoring
    
     Using Zabbix preprocessing to alter item values
70	Setting Up Zabbix Monitoring


Technical requirements

We will need a Zabbix server capable of performing monitoring, with the following requirements:

     A server with Zabbix server installed on a Linux distribution of your choice, such as CentOS or Ubuntu, but a distribution such as Debian, Rocky Linux, or anything else will suit you just as well
    
     A MariaDB server to monitor

I'll be using the same server as we used in the previous chapter, but any Zabbix server should do.

Setting up Zabbix agent 2 monitoring

Starting from the release of Zabbix 5, Zabbix also officially started support for the new Zabbix agent 2. Zabbix agent 2 brings some major improvements and is even written in another coding language, which is Golang instead of C. In this recipe, we will be exploring how to work with Zabbix agent 2 and explore some of the new features introduced by it.

Getting ready

To get started with Zabbix agent 2, all we need to do is install it on a host that we want to monitor. Make sure you have an empty Red Hat Enterprise Linux (RHEL)-based or Ubuntu Linux host ready to monitor.

How to do it…

Let's see how to install Zabbix agent 2 and then move on to actually working with it.

Installing Zabbix agent 2

Let's start by installing Zabbix agent 2 on the Linux host we want to monitor. I'll be showing you how to do the steps on both RHEL and Ubuntu systems:

     Issue the following command to add the repository: For RHEL-based systems:


rpm -Uvh https://repo.zabbix.com/zabbix/6.0/rhel/8/ x86_64/zabbix-release-6.0-1.el8.noarch.rpm
Setting up Zabbix agent 2 monitoring	71


For Ubuntu systems:


wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/ main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_ all.deb


dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb

     Then issue the following command to install Zabbix agent 2: For RHEL-based systems:
dnf -y install zabbix-agent2

For Ubuntu systems:

apt install zabbix-agent2


Congratulations, Zabbix agent 2 is now installed and ready to use.


Important note

When adding new repositories to your system, always check out the Zabbix download page. We can find the right up-to-date repository for our system here:

https://www.zabbix.com/download


Using a Zabbix agent in passive mode

Let's start by building a Zabbix agent with passive checks:

     After installing Zabbix agent 2, let's open the Zabbix agent configuration file for editing:


vim /etc/zabbix/zabbix_agent2.conf

In this file, we edit all the Zabbix agent configuration values we could need from the server side.

2.	Let's start by editing the following parameters:


Server=127.0.0.1


Hostname=Zabbix server
     Setting Up Zabbix Monitoring

         Change the value of Server to the IP of the Zabbix server that will monitor this passive agent. Change the value for Hostname to the hostname of the monitored server. We can get the IP address of our server with the following command:


ip addr

4.	Now restart the Zabbix agent 2 process:


systemctl enable zabbix-agent2


systemctl restart zabbix-agent2

     Next, move to the frontend of your Zabbix server and add this host for monitoring.
    
     Go to Configuration | Hosts in your Zabbix frontend and click Create host in the top-right corner.
    
     To create this host in our Zabbix server, we need to fill in the values as seen in the following screenshot:
































Figure 3.1 – The Zabbix host creation page for host lar-book-agent
Setting up Zabbix agent 2 monitoring	73


It's important to add the following:

         Host name: To identify this host.
    
         Groups: To logically group hosts.
    
         Interfaces: To monitor this host on a specific interface. No interface means no communication. It's possible to have a host without an interface in Zabbix 6, if we don't need it. In the case of a Zabbix agent monitored host, we need an agent interface.
    
     Make sure to add the correct IP address to the Agent interface configuration.
    
     It is also important to add a template to this host, which with Zabbix 6 must be done on the same tab. As this is a Linux server monitored by a Zabbix agent, let's add the correct out-of-the-box template, as shown in the following screenshot:

















Figure 3.2 – The Zabbix host template page for host lar-book-agent

     Click the blue Add button and you're done creating this agent host. Now that you've got this host, make sure the ZBX icon turns green, indicating that this host is up and being monitored by the passive Zabbix agent:







Figure 3.3 – The Zabbix configuration hosts page, host lar-book-agent
     Setting Up Zabbix Monitoring

         Because we configured our host and added a template with items, we can now see the values received on items for this host by going to Monitoring | Hosts and checking the Latest data button. Please note that the values could take a while to show up:
























Figure 3.4 – The Zabbix Latest data page for host lar-book-agent

Using a Zabbix agent in active mode

Now let's check out how to configure the Zabbix agent with active checks. We need to change some values on the monitored Linux server host side:

1.	Start by executing the following command:


vim /etc/zabbix/zabbix_agent2.conf

     Now let's edit the following value to change this host to an active agent:

ServerActive=127.0.0.1

     Change the value for ServerActive to the IP of the Zabbix server that will monitor this passive agent and also change the value of Hostname to lar-book-agent:


Hostname=lar-book-agent
Setting up Zabbix agent 2 monitoring	75


Important note

Keep in mind that if you are working with multiple Zabbix servers or Zabbix proxies, for example when you are running Zabbix server in high availability, that you need to fill in all the Zabbix servers' or Zabbix proxies' their IP addresses at the ServerActive parameter. High Availability (HA) nodes are delimited by a semicolon (;) and different Zabbix environment IPs by a comma (,).

     Now restart the Zabbix agent 2 process: systemctl restart zabbix-agent2
    
     Then move to the frontend of your Zabbix server and let's add another host with a template to do active checks instead of passive ones.
     First, let's rename our passive host. To do that, go to Configuration | Hosts in your Zabbix frontend and click the host we just created. Change Host name as follows:










Figure 3.5 – The Zabbix host configuration page for host lar-book-agent_passive

We are doing this because for an active Zabbix agent, the hostname in the Zabbix agent configuration file needs to match the configuration of our host as seen on the Zabbix frontend.

     Click on the blue Update button to save the changes.
    
     Go to Configuration | Hosts in your Zabbix frontend and click Create host in the top-right corner.
76	Setting Up Zabbix Monitoring


9.	Now let's create the host as follows:




































Figure 3.6 – The Zabbix host configuration page for host lar-book-agent

     Also, make sure to add the correct template, named Linux by Zabbix agent active:












Figure 3.7 – The Zabbix host template page for host lar-book-agent
Setting up Zabbix agent 2 monitoring	77


Please note that the ZBX icon won't turn green for an active agent. But when we navigate to Monitoring | Hosts and check Latest data, we can see our active data coming in.


Tip

As you might have noticed just now, a Zabbix agent can run in both passive and active mode at the same time. Keep this in mind when creating your own Zabbix agent templates, as sometimes you might want to combine the check types.

How it works…

Now that we have configured our Zabbix agents and know how they should be set up, let's see how the different modes work.

Passive agent

The passive agent works by collecting data from our host with the Zabbix agent. Every time an item on our host reaches its update interval, the Zabbix server asks the Zabbix agent what the value is now:









Figure 3.8 – Communication diagram between server and passive agent

Passive agents are great when working in environments where you want to keep communication initiated from the Zabbix server or Zabbix proxy side, for example, when there is a firewall that is only allowing outgoing traffic, as seen from the Zabbix server or proxy side.
78	Setting Up Zabbix Monitoring


Active agent

The active agent works by sending data from the Zabbix agent to a Zabbix server or Zabbix proxy. Every time an item on our agent reaches its update interval, the agent will send the value to our server.









Figure 3.9 – Communication diagram between server and active agent

The active agent is great when working with an environment where there is a firewall that is only accepting outgoing connections, as seen from the Zabbix agent side. This is the case in a lot of environments, and it can mitigate one of the main security concerns that is mostly associated with monitoring hosts.

On the other hand, the Zabbix agent working in active mode can also be a lot more efficient. Most of the load that comes from getting data to your Zabbix server is now on the Zabbix agent side. Because there are more Zabbix agents out there than you have Zabbix servers or proxies, offloading load like this is a great idea.

As mentioned, we can use both types of checks at the same time, giving us the freedom to configure every type of check we could possibly need. Our setup would then look like this:










Figure 3.10 – Communication diagram between server and both agent types

This might be the case in situations where we want to mainly monitor passively, but, for example, log file monitoring with the Zabbix agent must be done with an active Zabbix agent. We can then combine our modes and make sure to use the full scale of our features provided in the Zabbix agent.
Working with SNMP monitoring	79


See also

There's a lot of stuff going on under the hood of Zabbix agent 2; if you're interested in learning more about the core of Zabbix agent 2, check out this cool blog post by Alexey Petrov: https://blog.zabbix.com/magic-of-new-zabbix-agent/8460/.

Working with SNMP monitoring

Now let's do something I enjoy most when working with Zabbix: build SNMP monitoring. My professional roots lie in network engineering, and I have worked a lot with SNMP monitoring to monitor all these different network devices.

Getting ready

To get started, we need the two Linux hosts we used in the previous recipes:

     Our Zabbix server host
    
     The host we used in the previous recipe to monitor via the Zabbix active agent

How to do it…

Monitoring via SNMP polling is easy and very powerful. We will start by configuring

SNMPv3 on our monitored Linux host:

     Let's start by issuing the following commands to install SNMP on our host. For RHEL-based systems:
dnf install net-snmp net-snmp-utils

For Ubuntu systems:

apt install snmp snmpd libsnmp-dev

     Now, let's create the new SNMPv3 user that we will use to monitor our host. Please note that we'll be using insecure passwords, but make sure to use secure passwords for your production environments. Issue the following command:


net-snmp-create-v3-user -ro -a my_authpass -x my_privpass -A SHA -X AES snmpv3user

This will create an SNMPv3 user with the usernamesnmpv3user, the authentication password my_authpass, and the privilege password my_ privpass.
     Setting Up Zabbix Monitoring

         Make sure to edit the SNMP configuration file to allow us to read all SNMP objects: vim /etc/snmp/snmpd.conf
    
         Add the following line at the rest of the view systemview lines:


view	systemview	included	.1

5.	Now start and enable the snmpd daemon to allow us to start monitoring this server:


systemctl enable snmpd


systemctl start snmpd

This is all we need to do on the Linux host side; we can now go to the Zabbix frontend to configure our host. Go to Configuration | Hosts in your Zabbix frontend and click Create host in the top-right corner.

6.	Now fill in the host configuration page:

































Figure 3.11 – Zabbix host configuration page for host lar-book-agent_snmp
Working with SNMP monitoring	81


     Don't forget to change the IP address of the SNMP interface to your own value.
    
     Make sure to add the right out-of-the-box template as shown in the following screenshot:















Figure 3.12 – Adding the template Linux SNMP to the host


Tip

While upgrading from an earlier Zabbix version to Zabbix 6, you won't get all the new out-of-the-box templates. If you feel like you are missing some templates, you can download them at the Zabbix Git repository: https://

git.zabbix.com/projects/ZBX/repos/zabbix/browse/ templates.

     We are using some macros in our configuration here for the username and password. We can use these macros to actually add a bunch of hosts with the same credentials. This is very useful, for instance, if you have a bunch of switches with the same SNMPv3 credentials.
82	Setting Up Zabbix Monitoring


Let's fill in the macros under Administration | General and use the dropdown to select Macros. Fill in the macros like this:















Figure 3.13 – Zabbix global macro page with SNMP macros


Tip

A cool feature in Zabbix 6 is the ability to hide macros in the frontend by using the macro type Secret text. Do keep in mind that macros of the type Secret text are still unencrypted in the Zabbix database, and for fully encrypted macros we would need something like HashiCorp Vault. Check out the documentation for more information: https://www.zabbix.com/ documentation/current/en/manual/config/secrets.

     Use the dropdown to change {$SNMPV3_AUTH} and {$SNMPV3_PRIV} to Secret text:










Figure 3.14 – Zabbix Secret text used for hiding sensitive (authentication) data
Working with SNMP monitoring	83


     Now, after applying these changes by clicking Update, we should be able to monitor our Linux server via SNMPv3. Let's go to Monitoring | Hosts and check the Latest data page for our new host:






























Figure 3.15 – SNMP Latest data for host lar-book-agent_snmp

Do note that it might take some time for your data to show up here.


Tip

When working with macros, there are three levels in cascading order: Global-, Template-, and Host-level macros. When working with Global level macros, keep in mind that they are not exported with templates or hosts. You want to use Template level and Host level for most cases, to keep your exports independent of Zabbix global settings.
84	Setting Up Zabbix Monitoring


How it works…

When we create a host as we did in step 4, Zabbix polls the host using SNMP. Polling SNMP like this works with SNMP OIDs. For instance, when we poll the item called Free memory, we ask the SNMP agent running on our Linux host to provide us the value for 1.3.6.1.4.1.2021.4.6.0. That value is then returned to us on the Zabbix server:










Figure 3.16 – Diagram showing communication between Zabbix server and SNMP host

SNMPv3 adds authentication and encryption to this process, making sure that when our Zabbix server requests information, that request is first encrypted and the data is sent back encrypted as well.

We also included the option to use Bulk requests when configuring our host. Bulk requests request several OIDs in the same stream, making this the preferred method of doing SNMP requests as it is more efficient. Only disable it for hosts that do not support bulk requests.

Lastly, let's take a look at SNMP OIDs, the most important part of our SNMP request. OIDs work in a tree-like structure, meaning that every number behind the dot can contain another value. For example, let's look at this OID for our host:


1.3.6.1.4.1.2021.4 = UCD-SNMP-MIB::memory

If we poll that OID with either the SnmpWalk CLI tool or our Zabbix server, we will get several OIDs back:


.1.3.6.1.4.1.2021.4.1.0 = INTEGER: 0


.1.3.6.1.4.1.2021.4.2.0 = STRING: swap


.1.3.6.1.4.1.2021.4.3.0 = INTEGER: 1679356 kB


.1.3.6.1.4.1.2021.4.4.0 = INTEGER: 1674464 kB


.1.3.6.1.4.1.2021.4.5.0 = INTEGER: 1872872 kB


.1.3.6.1.4.1.2021.4.6.0 = INTEGER: 184068 kB
Creating Zabbix simple checks and the Zabbix trapper	85


That includes our 1.3.6.1.4.1.2021.4.6.0 OID with the value that contains our free memory. This is how SNMP is built, like a tree.

Creating Zabbix simple checks and the Zabbix trapper

In this recipe, we will go over two checks that can help you build some more customized setups. The Zabbix simple checks provide you with an easy way to monitor some specific data. The Zabbix trapper combines with Zabbix sender to get data from your hosts into the server, allowing for some scripting options. Let's get started.

Getting ready

To create these checks, we will need a Zabbix server and a Linux host to monitor. We can use the host with a Zabbix agent and SNMP monitoring from the previous recipes.

Do note for these checks that we do not actually need the Zabbix agent.

How to do it…

Working with simple checks is quite simple, as the name suggests, so let's start.
86	Setting Up Zabbix Monitoring


Creating simple checks

We will create a simple check to monitor whether a service is running and accepting TCP connections on a certain port:

     To get this done, we will need to create a new host in the Zabbix frontend. Go to Configuration | Hosts in your Zabbix frontend and click Create host in the top-right corner.
    
     Create a host with the following settings:





















Figure 3.17 – Zabbix host configuration page for host lar-book-agent_simple

     Now go to Configuration | Hosts, click the newly created host, and go to Items. We want to create a new item here by clicking the Create item button.

We will create a new item with the following values, and after doing so, we will click the Add button at the bottom of the page:
Creating Zabbix simple checks and the Zabbix trapper	87











































Figure 3.18 – The Zabbix item configuration page for the port 22 check on the lar-book-agent_simple host
88	Setting Up Zabbix Monitoring


     Make sure to add a tag to the item as well, as we need this in several places to filter and find our item when working with Zabbix. Set it up like this:














Figure 3.19 – Zabbix SSH port item, Tag tab


Important note

We are adding the item key net.tcp.services[ssh,,22] here. The port in this case is optional, as we can specify the service SSH with a different port if we want to.

     Now we should be able to see whether our server is accepting SSH connections on port 22 on our Latest data screen. Navigate to Monitoring | Hosts and check the Latest data screen for our new value:






















Figure 3.20 – Zabbix Latest data page for host lar-book-agent_simple, item port 22 check
Creating Zabbix simple checks and the Zabbix trapper	89


     There is one more thing wrong here. As you can see, we do not currently have a value mapping setup. The Last value is just displaying a 1 or 0. To change this, navigate back to Configuration | Hosts and edit the lar-book-agent_simple host.
     Click on the Value mapping tab and click the small Add button to add a value mapping. Create the following:


















Figure 3.21 – Host lar-book-agent_simple, value mapping window

     Click on the blue Add button and click on the blue Update button.
    
     Then, back at the full Configuration | Hosts list, navigate to our lar-book-agent_simple host and click on Items for this host.
    
     Edit the Check if port 22 is available item and add the value mapping as follows:

















Figure 3.22 – Host lar-book-agent_simple, edit item window
90	Setting Up Zabbix Monitoring


That's all there is to creating your simple checks in Zabbix. The latest data page will now look like this:




















Figure 3.23 – Latest data page for our port 22 check item

As you can see, there is a human-readable value now displaying either Up or Down, giving us a human-readable entry, which is easier to understand. Now let's move on to the Zabbix trapper item.

Creating a trapper

We can do some cool stuff  with Zabbix trapper items once we get more advanced setups.

But for now, let's create an item on our lar-book-agent_simple host:

     Go to Configuration | Hosts and click the host, then go to Items. We want to create a new item here again by clicking the Create item button.
Creating Zabbix simple checks and the Zabbix trapper	91


Now let's create the following item and click the Add button:


































Figure 3.24 – Zabbix item trap receiver configuration screen for host lar-book-agent_simple

     Make sure to also navigate to the Tags tab and add a tag that we will use later for filtering:













Figure 3.25 – Zabbix item trap receiver tag configuration screen for host lar-book-agent_simple
     Setting Up Zabbix Monitoring

         If we go to the CLI of our monitored server, we can now execute the following to install Zabbix sender:


RHEL-based systems:


dnf -y install zabbix-sender


Ubuntu systems:


apt install zabbix-sender

4.	After installation, we can use Zabbix sender to send some information to our server:


zabbix_sender -z 10.16.16.152 -s "lar-book-agent_simple"

-k trap -o "Let's test this book trapper"

Now we should be able to see whether our monitored host has sent out the Zabbix trap and the Zabbix server has received this trap for processing.
5.	Navigate to Monitoring | Hosts and check the Latest data screen for our new value:








Figure 3.26 – Zabbix Latest data page for host lar-book-agent_simple, item trap receiver

There it is, our Zabbix trap in our Zabbix frontend.

How it works…

Now that we have built our new items, let's see how they work by diving into the theoretical side of Zabbix simple checks and trappers.

Simple checks

Zabbix simple checks are basically a list of built-in checks made for monitoring certain values. There is a list and description available on the Zabbix documentation for all the simple checks that are available here: https://www.zabbix.com/documentation/ current/manual/config/items/itemtypes/simple_checks.

All of these checks are performed by the Zabbix server to collect data from a monitored host. For example, when we do the Zabbix simple check to check whether a port is open, our Zabbix server requests whether it can reach that port and turns that into a status we can then see in our Zabbix frontend.
Creating Zabbix simple checks and the Zabbix trapper	93


This means that if your monitored host's firewall is blocking port22 from Zabbix server, we'll get a service is down value. However, this does not necessarily mean that SSH isn't running on the server; it simply means SSH is down as seen from the side of the Zabbix server or proxy.








Figure 3.27 – Zabbix server-to-host communication diagram


Tip

Keep in mind that working with simple checks is dependent on external factors such as the firewall settings on the monitored host. When you build your own simple checks, make sure to check these factors as well.

Trappers

When working with Zabbix sender, we are doing exactly the opposite of most checks. We are building an item on our Zabbix server, which allows us to capture trap items. This allows us to build some custom checks to send data to our Zabbix server from a monitored host:








Figure 3.28 – Zabbix server trap receiver diagram

Let's say, for instance, that you want to build a custom Python script that, at the end of running the scripts, sends output to the Zabbix server. We could ask Python to send this data using the Zabbix sender utility, and then you'd have this data available for processing on the Zabbix server.

For example, it is used by some companies who write their own software to completely integrate their software into Zabbix. As you can see, we can greatly extend our options with Zabbix trapper and customize our Zabbix server even further.
94	Setting Up Zabbix Monitoring


Working with calculated and dependent items

Calculated and dependent items are used in Zabbix to produce additional values from existing values. Sometimes, we have already collected a value and we need to do more with the values created by that item. We can do exactly that by using calculated and dependent items.

Getting ready

To work with calculated items and dependent items, we are going to need the Zabbix server and monitored hosts from the previous recipes. We will add the items on the lar-book-agent_passive host and our Zabbix server (or any MySQL server) host, so we already have some items available to calculate and make dependent.

How to do it…

Let's see how we can extend our items by getting started with the calculated items.

Working with calculated items

     Let's navigate to our host configuration by going to Configuration | Hosts and clicking on our lar-book-agent_passive host's items. In the filter field named Name, enter memory and you will get the following output:








Figure 3.29 – Zabbix item page for host lar-book-agent_passive
Working with calculated and dependent items	95


     What we are going to do now is create a calculated item that is going to show us the average memory utilization over a period of 15 minutes. We can use this value to determine how busy our host was during that period, without having to look at the graphs.
    
     Let's click the Create item button and start creating our new calculated item. We want our item to have the following values:
























Figure 3.30 – Zabbix item configuration page, average memory used
     Setting Up Zabbix Monitoring

         Make sure to also navigate to the Tags tab and add a tag that we will use later for filtering:














Figure 3.31 – Calculated item Tags tab

     Now if we go to check our Monitoring | Hosts page and select Latest data, we can check out our value. Make sure to filter in the Name field for memory, so we see the correct values:





Figure 3.32 – Zabbix Latest data page for host lar-book-agent_passive, memory items

Now we can clearly see that we are calculating the 15-minute average of the memory utilization on our newly created item.

Working with dependent items

Time to make our first dependent item. I'll use the lar-book-agent_centos host or our (as it's called by default) Zabbix server , but any MySQL database server should work. Let's say we want to request some variables from our MySQL database in one big batch. We can then create dependent items on top of the first item to further process the data:

     Let's start by creating the main check. Navigate to Configuration | Hosts, select our host, and then let's click the Create item button to start creating our first new item. We want an item with the following variables:
Working with calculated and dependent items	97






















































Figure 3.33 – Zabbix item configuration page, database status
98	Setting Up Zabbix Monitoring


Now, this item is an SSH check that logs in to our Zabbix server host using SSH and then executes the code as entered in the Executed script field. This code will then log in to our MariaDB database and it will print the status. Make sure to enter your credentials correctly.


Tip

Instead of using plain text credentials in the MySQL command, which is not recommended. Use macros in your Executed script field. This way, you can use the macro type Secret text to make sure no one can read your password from the frontend.

2.	Before saving this new item, make sure to also add a tag like this:














Figure 3.34 – Zabbix master item configuration page, Tags tab

     Now click the blue Add button to save this new item.
    
     Back at the list of items, let's click on this host's hostname and then Macros. Create a new {$USERNAME} and {$PASSWORD} macro with your SSH username and password as the Value.
    
     Next, go to Monitoring | Latest data and check out the data for our new check. There should be a long list of MariaDB values. If so, we can continue with our next step of creating the dependent item.
    
     To create the dependent item, navigate to Configuration | Hosts, select our host, and let's click the Create item button. We want this item to get the following variables:
Working with calculated and dependent items	99





































Figure 3.35 – Zabbix item configuration page, MariaDB aborted clients

7.	Make sure to add the tag, as follows:













Figure 3.36 – Zabbix dependent item configuration page, Tags tab
     Setting Up Zabbix Monitoring

         It's very important to add preprocessing to this item as well, otherwise, we will simply get the same data as our master item. So, let's add the following:










Figure 3.37 – Zabbix item Preprocessing page, MariaDB aborted clients

With the preprocessing added, the result will be the number of aborted clients for our MariaDB instance:







Figure 3.38 – Zabbix Latest data page, MariaDB aborted clients

As you can see, using a dependent item, we can use already available information from other Zabbix items and split them up into dependent items.

How it works…

The types we've discussed in this How to do it… section can be quite complicated; let's go over how the items actually work.

Calculated items

Working with calculated items can be a great way to get even more statistics out of your existing data. Sometimes you just need to combine multiple items into one specific value.

What we did just now works by taking several values of 1 item in a period of 15 minutes and calculating the average, as follows:
Working with calculated and dependent items	101

























Figure 3.39 – Zabbix dependent item diagram

We're taking those values and calculating the average every 15 minutes. It gives us a nice indication of what we are doing over a set period of time.

Dependent items

Dependent items work simply by taking the data from a master item and processing that data into other data. This way, we can structure our data and keep our check interval for all these items the same:



















Figure 3.40 – Dependent item diagram
102	Setting Up Zabbix Monitoring


As seen here, dependent items basically work as duplicators, with additional preprocessing options. Do note that preprocessing must be used to extract data from the master item. Without preprocessing, our data will be exactly the same as the master item.


Tip

Sometimes we do not require our master item to be saved in our database; we already have the information in our dependent items. When we don't want the master item to be saved, we simply select the Do not keep history option on that master item.


Creating external checks

To further extend Zabbix functionality, we can use our own custom scripts, which are used by Zabbix external checks. Not everything that we want to monitor will always be standard in Zabbix, although a lot is. There's always something that could be missing, and external checks are just a way to bypass some of these.

Getting ready

For this recipe, we are going to need just our Zabbix server. We can create an item on our lar-book-centos host, which is our Zabbix server monitored host.

How to do it…

     First, let's make sure our Zabbix server configuration is set up correctly. Execute the following on the Zabbix server CLI:

cat /etc/zabbix/zabbix_server.conf | grep ExternalScripts=

     This should show us the path where we will place the script that is used by the Zabbix external check. By default, this is /usr/lib/zabbix/ externalscripts/, and we will create a new script called test_external in this folder with the following command:


vim /usr/lib/zabbix/externalscripts/test_external

Add the following code to this file and save:


#!/bin/bash


echo $1
Creating external checks	103


     Make sure our Zabbix server can execute the script by adding the right permissions to the file. The zabbix user on your Linux server needs to be able to access and execute the file:

chmod +x /usr/lib/zabbix/externalscripts/test_external

chown zabbix:zabbix /usr/lib/zabbix/externalscripts/test_ external

     Now we are ready to go to our host to create a new item. Navigate to Configuration | Hosts, select our host, lar-book-centos, and click the Create item button. We want this item to get the following variables:



























Figure 3.41 – Zabbix item configuration page

     After adding this new item, let's navigate to Monitoring | Hosts and check the Latest data page for our host. We should get our Test variable returned by our script as the Last value in Zabbix, as shown in the following screenshot:







Figure 3.42 – Zabbix Latest data page
104	Setting Up Zabbix Monitoring


Tip

Use the macros in the frontend as variables to send data from your frontend to your scripts. You can further automate your checks with this to enhance your external checks.

How it works…

External checks seem like they have a steep learning curve, but they are actually quite simple from the Zabbix side. All we do is send a command to an external script and expect a result output:






Figure 3.43 – Zabbix server external script communication diagram

Like in our example, we sent the value Test to our script, which the script then, in turn, echoed back to us as $1.

When you have good knowledge of a programming language such as Python, you can use this function to build a lot more expansions on top of the current existing Zabbix feature set – a simple yet powerful tool to work with.

Setting up JMX monitoring

Built into Zabbix is JMX monitoring, so we can monitor our Java applications. In this recipe, we'll check out how to monitor Apache Tomcat with Zabbix JMX so we can get a feel for what this monitoring option is all about.

Getting ready

To get ready for this recipe, we are going to need our Zabbix server to monitor our JMX application.

I used a CentOS 7 machine for the example in this recipe, with Tomcat installed. It can be quite tricky to use Tomcat on later CentOS versions due to package dependencies, so I recommend sticking with CentOS 7 for the example. You can add the following to your Tomcat configuration after installation to get it working in our recipe:


JAVA_OPTS="-Djava.rmi.server.hostname=10.16.16.155 dcom.sun.management.jmxremote

Setting up JMX monitoring	105


dcom.sun.management.jmxremote.port=12345 dcom.sun.management.jmxremote.authenticate=false dcom.sun.management.jmxremote.ssl=false"


If you want to set up JMX monitoring in your production environment, you can use the settings you have probably already set up there. Simply change the port and IP address accordingly.

How to do it…

To set up JMX monitoring, we are going to add a host to our Zabbix server that will monitor our Apache Tomcat installation. But first, we will need to add some settings to our /etc/zabbix/zabbix_server.conf file:

     Let's edit the zabbix_server.conf file by logging in to our Zabbix server and executing the following command:


vim /etc/zabbix/zabbix_server.conf

2.	We will then need to add the following lines to this file:


JavaGateway=127.0.0.1


StartJavaPollers=5


Tip

It's possible to install your Java gateway to a separate host from your Zabbix.

This way we can spread the load and scale more easily. Simply install it

to a separate host and add the IP address of that host to the JavaGateway parameter. We won't be doing this in the example, but keep the Java gateway setup on the Zabbix server host itself.

     We will also need to install the zabbix-java-gateway application on our Zabbix server with the following command.

RHEL-based systems:


dnf install zabbix-java-gateway systemctl enable zabbix-java-gateway systemctl start zabbix-java-gateway systemctl restart zabbix-server

106	Setting Up Zabbix Monitoring


Ubuntu systems:


apt install zabbix-java-gateway systemctl enable zabbix-java-gateway systemctl start zabbix-java-gateway systemctl restart zabbix-server


That is all we need to do on the server side of things to get JMX monitoring to work. Zabbix doesn't include these settings by default, so therefore we need to add the text to our file and install the application.

     To get started with monitoring our JMX host, go to Configuration | Hosts in your Zabbix frontend and click Create host in the top-right corner.

We will then add a host with the following settings:



































Figure 3.44 – Zabbix item configuration page
Setting up JMX monitoring	107


     After this, our JMX icon should turn green; let's check this under Monitoring | Hosts. It should look like this:




Figure 3.45 – Zabbix Monitoring | Hosts page

     If we click on Latest data for our new JMX monitored host, we should also see our incoming data. Check it out; it should return stats like these:







Figure 3.46 – Zabbix Latest data page

How it works…

Zabbix utilizes a Java gateway either hosted on the Zabbix server itself or hosted on another server (proxy) to monitor JMX applications:







Figure 3.47 – Communication diagram between Zabbix server and Java

Zabbix polls the Java gateway and the Java gateway, in turn, communicates with our JMX application, as it does with Tomcat in our example. The data in turn is then returned through the same path and we can see our data in our Zabbix server.

See also

There are loads of applications that can be monitored through Zabbix JMX. Check out the Zabbix monitoring and integrations page for more uses of Zabbix JMX monitoring:

https://www.zabbix.com/integrations/jmx.
108	Setting Up Zabbix Monitoring


Setting up database monitoring

Databases are a black hole to a lot of engineers; there's data being written to them and there's something being done with this data. But what if you want to know more about the health of your database? That's where Zabbix database monitoring comes in – we can use it to monitor the health of our database.

Getting ready

We'll be monitoring our Zabbix database, for convenience. This means that all we are going to need is our installed Zabbix server with our database on it. We'll be using MariaDB in this example, so if you have a PostgreSQL setup, make sure to install a MariaDB instance on a Linux host.

How to do it…

Before getting started with the item configuration, we'll have to do some stuff on the CLI side of the server:

     Let's start by installing the required modules on our server. RHEL-based systems:
dnf install unixODBC mariadb-connector-odbc

Ubuntu systems:

apt install odbc-mariadb unixodbc unixodbc-dev odbcinst

     Now let's verify whether our Open Database Connectivity (ODBC) configuration files exist:


odbcinst -j

Your output should look as follows:


unixODBC 2.3.7

DRIVERS............
:
/etc/odbcinst.ini


SYSTEM DATA SOURCES:
/etc/odbc.ini


FILE DATA SOURCES..:
/etc/ODBCDataSources


USER DATA SOURCES..:
/root/.odbc.ini



SQLULEN Size.......
:
8



SQLLEN Size........
:
8


SQLSETPOSIROW Size.:
8



Setting up database monitoring	109


     If the output is correct, we can go to the Linux CLI and continue by editing odbc. ini to connect to our database:


vim /etc/odbc.ini

Now fill in your Zabbix database information. It will look like this:


[book]

Description
= MySQL book test database


Driver
= mariadb


Server
= 127.0.0.1



Port
=
3306



Database
=
zabbix




4.	Now let's test whether our connection is working as expected by executing this:


isql -v book

You should get a message saying Connected; if you don't, then check your configuration files and try again.

     Now let's move to the Zabbix frontend to configure our first database check. Navigate to Configuration | Hosts and click the host called lar-book-centos, or it might still be called Zabbix server. Now go to Items; we want to create a new item here by clicking the Create item button.


Tip

If you haven't already, a great way to keep your Zabbix structured is to keep all hostnames in Zabbix equal to the real server hostname. Rename your default Zabbix server host in the frontend to what you've actually called your server.
110	Setting Up Zabbix Monitoring


We want to add an item with the following parameters:
































Figure 3.48 – Zabbix item configuration page, items in Zabbix database

6.	Make sure to also add a tag to the item:














Figure 3.49 – Zabbix item configuration page, items in Zabbix database, Tags tab
Setting up database monitoring	111


     Now click the Add button and click on the name of the host to add the macros, as follows:

















Figure 3.50 – Zabbix host macro configuration page

     Now if you go to Monitoring | Hosts and click on Latest data for our host, you'll get to see this:





Figure 3.51 – Zabbix Latest data page for host lar-book-centos, items in Zabbix database

We can now see directly from the database how many items are written to it.

How it works…

The Zabbix database monitoring works by connecting to your database with the ODBC middleware API. Any database supported by ODBC can be queried with Zabbix database monitoring:






Figure 3.52 – A diagram showing communication between the Zabbix server and ODBC
112	Setting Up Zabbix Monitoring


Basically, your Zabbix server sends a command with, for instance, your MySQL query to the ODBC connector. Your ODBC connecter sends this query to the database through the ODBC API, which in turn returns a value to ODBC. ODBC then forwards the value to the Zabbix server and hey presto: we have a value under our item.

There's more…

You can do loads of queries to your databases with Zabbix database monitoring, but keep in mind that you are working with actual queries. Querying a database takes time and processing power, so keep your database monitoring structured and define the right execution times.

Alternatively, we can use Zabbix agent 2 to monitor most databases natively. This can improve security and performance and keep complexity lower.

Setting up HTTP agent monitoring

With the Zabbix HTTP agent, we can monitor a web page or API by retrieving data from it. For instance, if there's a counter on a web page and we want to keep an eye on that counter value, we can do so with the Zabbix HTTP monitor.

Getting ready

We are going to need a web page to monitor and we will need our Zabbix server. For your convenience, we've added a page to our own website to retrieve a value from. Here's the page: https://oicts.com/book-page/.

Please also note that your Zabbix server will need an active internet connection for this recipe.

How to do it…

Let's poll the special web page we've created for you showing a visitor count that's currently configured on it. This is a real counter for the number of times (unique) visitors have opened the URL:

     Navigate to your Zabbix frontend and navigate to Configuration | Hosts, then click the host called lar-book-agent_simple.
Setting up HTTP agent monitoring	113


     Now go to Items; we want to create a new item here by clicking the Create item button. We are going to need to create an HTTP agent item as shown in the following screenshot:














































Figure 3.53 – Zabbix Item configuration page, visitor count on the oicts.com page
114	Setting Up Zabbix Monitoring


3.	Do not forget to edit Type of information.










Figure 3.54 – Zabbix Item configuration page, visitor count on oicts.com page, lower part of the page

4.	We'll also need to add a tag to this item.














Figure 3.55 – Zabbix Item configuration page, visitor count on oicts.com page, Tags tab

5.	Use the following preprocessing steps:












Figure 3.56 – Zabbix Item configuration page, visitor count on oicts.com page, Preprocessing tab

     Now navigate to Monitoring | Hosts and open the Latest data page for our lar-book-agent_simple host. If everything is working as it should, we should now be requesting this page's visitor count every 15 minutes as follows:







Figure 3.57 – Zabbix Latest data page
Using Zabbix preprocessing to alter item values	115


How it works…

What we do here is request the complete web page from Zabbix by navigating to the page with the HTTP agent and downloading it. When we have the complete content of the page, in this case, an HTML/PHP page, we can process the data:













Figure 3.58 – Diagram showing Zabbix HTTP agent communication

We ask our preprocessor via a regex to go through the requested code and only show the number behind where it says Total Page Visits:.

All that's left is the number, ready for us to use in graphs and other types of data visualization.

Using Zabbix preprocessing to alter item values

Preprocessing item values is an important functionality in Zabbix; we can use it to create all kinds of checks. We've already done some preprocessing in this chapter, but let's take a deeper dive into it and what it does.

Getting started

We are going to need a Zabbix server to create our check for. We will also need a passive Zabbix agent on a Linux host to get our values from and preprocess them. We can use the agent that is running on our Zabbix server for this; in my case, this is lar-book-centos.
116	Setting Up Zabbix Monitoring


How to do it…

     Let's start by logging in to our Zabbix frontend and going to Configuration | Hosts.
    
     Click on your Zabbix server host; in my case, it's called lar-book-centos.
    
     Now go to Items and click on the blue Create item button in the top-right corner. Let's create a new item with the following information:




























Figure 3.59 – New item creation screen, Get traffic statistics from CLI

4.	Do not forget to add your tag:













Figure 3.60 – New item creation screen, Get traffic statistics from CLI, Tags tab
Using Zabbix preprocessing to alter item values	117


     Make sure to change ens192 to your own primary network interface. You can find your primary network interface by logging in to the Linux CLI and executing the following:


Ifconfig

     Back at the create item screen, click on the blue Add button. This item will use the Zabbix agent to execute a remote command on the Linux CLI.
    
     When we navigate to this new item, we can see that the item becomes unsupported. This is because when we use thesystem.run key, we need to allow it in the Zabbix agent configuration:










Figure 3.61 – Unsupported item information, Unknown metric system.run

     Log in to the Linux CLI of the monitored host and edit the Zabbix agent configuration with this:


vim /etc/zabbix/zabbix_agent2.conf

     Go to the Option: AllowKey line and add AllowKey=system.run[*] as shown here:















Figure 3.62 – Zabbix agent configuration file, AllowKey=system.run[*]
     Setting Up Zabbix Monitoring

         Save the file and restart the Zabbix agent with this: systemctl restart zabbix-agent2
    
         Back at the Zabbix frontend, the error we noticed in step 7 should be gone after a few minutes.
    
         Navigate to Monitoring | Latest data and filter on your Zabbix server host lar-book-centos and the name of the new Get traffic statistics from CLI item.
    
         The value should now be pulled from the host. If we click on History, we can see the full value; it should look as follows:












Figure 3.63 – Zabbix agent system.run command executing ifconfig ens192 results

     The information seen in the image is way too much for just one item. We need to split this up. Let's use preprocessing to get the number of RX bytes from the information.
    
     Go back to Configuration | Hosts and click on your Zabbix server host. Go to Items on this host.
    
     Click on the Get traffic statistics from CLI item to edit it. Change the name to

Total RX traffic in bytes for ens192 and add B to Units, where B stands for bytes. It will look like this:
Using Zabbix preprocessing to alter item values	119



























Figure 3.64 – Zabbix agent system.run item

17. Add your tag:














Figure 3.65 – New item creation screen, Get traffic statistics from CLI, Tags tab

18. Now click on Preprocessing and click on the underlined Add button.
     Setting Up Zabbix Monitoring

         A Regular expression (regex) field will be added, which we are going to fill to match the total number of bytes for your interface. Fill in the following:










Figure 3.66 – Zabbix agent system.run item preprocessing

     Make sure to also select the box under Custom on fail.
    
     Let's click on the underlined Add button again and use the drop-down menu for this new step to select Discard unchanged. The end result will look like this:












Figure 3.67 – Zabbix agent system.run item preprocessing

     We can now click the blue Update button to finish editing this item.
    
     Navigate back to Monitoring | Latest data and filter on your host and the new item name, Total RX traffic in bytes for ens192. Make sure to use your own interface name.
    
     We can now see our value coming in, and we have an item displaying our total RX traffic for our main interface:






Figure 3.68 – Zabbix Total RX traffic item latest data
Using Zabbix preprocessing to alter item values	121


How it works…

We've already done some preprocessing in the Working with calculated and dependent items recipe to get data from a master item. We also used preprocessing in the Setting up HTTP agent monitoring recipe to get a specific value from a web page. We didn't go over the preprocessing concepts used in those recipes, though, so let's go over them.

When working with preprocessing, it's important to know the basic setup. Let's take a look at the incoming data before we used preprocessing:












Figure 3.69 – Zabbix agent system.run command executing ifconfig ens192 results

This is a lot of information. When we look at how Zabbix items are used, we try to put graspable information in a single item. Luckily, we can preprocess this item before we store the value in Zabbix. In the following figure, we can see the preprocessing steps we added to our item:











Figure 3.70 – Zabbix agent system.run item preprocessing with two steps

Our first step is a regex. This step will make sure to only use the numbers we need. We match on the word RX, then the word bytes, and a sequence of numbers after them. This way, we end up with the total number of RX bytes in capture group 2. This is why we fill in \2 in the output field. We also specify Custom on fail, which will discard any value if the regex doesn't match.
122	Setting Up Zabbix Monitoring


Our second step is to discard any values that are the same as the value received before.

Instead of storing duplicate values, we simply discard them and save some space in our

Zabbix database.


Tip

It's a lot easier to build a regex when using an online tool such as https:// regex101.com/. You can see what number your capture groups will get, and there's a lot of valuable information in the tools as well.

It's important to note here that steps are executed in the sequence they are defined in the frontend. If the first step fails, the item becomes unsupported unless Custom on fail is set to do something else.

By adding preprocessing to Zabbix, we open up a whole range of options for our items, and we are able to alter our data in almost any way required. These two steps are just the beginning of the options opened up when diving into the world of Zabbix preprocessing.

See also

Preprocessing in Zabbix is an important subject, and it's impossible to cover every aspect of it in a single recipe. The two preprocessing steps in this recipe's example are just two of the many options we can use. Check out the official Zabbix documentation to see the other options we can use:

https://www.zabbix.com/documentation/current/en/manual/config/ items/preprocessing