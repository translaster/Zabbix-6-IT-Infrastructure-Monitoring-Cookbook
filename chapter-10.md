# 10 Extending Zabbix Functionality with Custom Scripts and the Zabbix API

Zabbix offers a lot of functionality out of the box. But where Zabbix really shines is customization, not only through the default frontend but especially with scripts and the Zabbix API.



In this chapter, I will go over the basics of using the Zabbix API. We will then see how a Python script can utilize the API to build something cool, such as a jumphost. After that, we'll use some scripts written by *Brian van Baekel* to schedule maintenance periods as a Zabbix user and to enable and disable hosts with limited permissions from a Zabbix map.



After following these recipes, you'll be more than ready to tackle the Zabbix API and you'll know how to use scripts to extend Zabbix functionality. This chapter will expand your possibilities with Zabbix to almost endless proportions and you'll be ready to become a professional Zabbix user yourself.

360	Extending Zabbix Functionality with Custom Scripts and the Zabbix API





In this chapter, we will cover the following topics:



Setting up and managing API tokens



Using the Zabbix API for extending functionality



Building a jumphost using the Zabbix API and Python



Creating maintenance periods as a Zabbix user



Enabling and disabling a host from Zabbix maps



**Technical requirements**



We are going to need a Zabbix server as well as some new Linux hosts. We will also need to have general knowledge of scripting and programming. We are going to use Python to extend some functionality of Zabbix, which we'll provide scripts for.



The code required for the chapter can be found at the following link:



[https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10)[Monitoring-Cookbook/tree/main/chapter10](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10)



Make sure to keep all of this ready and you'll be sure to nail these recipes.



**Setting up and managing API tokens**



Let's start off our chapter by doing some pre-configuration for working with APIs in Zabbix. If you've worked with the Zabbix API before you might know it can be quite



a hassle to use API calls to authenticate and get an API token for using it in your scripts.



This is no longer the case, as we can generate API tokens using the Zabbix frontend.



**Getting ready**



For this recipe, all we'll need is the Zabbix setup running. We'll be using the frontend to generate the API token. From here we can use the API token in any of our integrations further on in this chapter.



**How to do it…**



First, let's log in to the Zabbix frontend as a Super admin user.



Navigate to **Administration** | **User groups** and click the blue **Create user group** button in the top-right corner.

Setting up and managing API tokens	361





3.	Here we'll create a new user group. Fill in the **Group name** field as API users.



Switch to the **Permissions** tab and give your API user group permission to every user group by clicking the **Select** button and selecting every host group.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_455e97051de952fd.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 10.1 – Zabbix Administration | User groups, create user group page, API Users

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Click the blue **Select** button at the bottom of this popup and click on **Read-write** then the small dotted **Add** button. It should now look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_d0a7056ea523f5ab.jpg) 



























Figure 10.2 – Zabbix Administration | User groups, user group permissions page, API Users





**Tip**



Instead of creating the API user as Super admin, we can also limit the permissions by limiting the host group access on the **API Users** user group. This could be preferred in your environment, as you might want to limit API access. Use whatever fits your preference.



Click the blue **Add** button at the bottom of the page to add this new user group.



Now let's go to **Administration** | **Users** and click the blue **Create user** button in the top-right corner.

Here we will create a new user called the API user. Create the user as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_4c96fcee208b62fc.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 10.3 – Zabbix Administration | Users, user creation page, API user

Setting up and managing API tokens	363





Before adding the user, switch to the **Permissions** tab and add the **Super admin role**.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_5a9c3e1538e3d5a1.jpg) 



























Figure 10.4 – Zabbix Administration | Users, user permissions page, API user



We can now add the user by clicking the blue **Add** button at the bottom of the page.



Next up we need to create some API tokens for this user. Navigate to **Administration** | **General** | **API tokens**.

Let's click the blue **Create API token** button in the top-right corner and fill in our **User** as API and our **Name** field as API book key. Set **Expires at** to somewhere far in the future or whatever you think might be secure. It will look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_d504faab38dd75cf.jpg) 



































Figure 10.5 – Administration | General | API token, API token creation page

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Click the blue **Add** button at the bottom of the page to generate the new API token. This will bring us to the next page:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_676fa19684dba579.jpg) 



























Figure 10.6 – Zabbix API user API token generated page



Make sure to save the **Auth token** to a secure location, like a password vault. It will be important later in the labs.



We can click the **Close** button at the bottom of this page now. This will bring us back to the **API tokens** page where we can manage all of our created API tokens.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_792e58b00a395937.jpg) 































Figure 10.7 – Zabbix API user API tokens page



**How it works…**



Because Zabbix now comes with built-in API token management, it has become a lot easier to work with the Zabbix API. Using a dedicated API user, we can manage all of our tokens in a single location or we can set up private API tokens under our own user account.

Using the Zabbix API for extending functionality	365





In this case, we created a new API Users group. This is important because our API tokens still respect user permissions. If we create an API user under any other role than Super admin we can restrict our API access using the API Users group.



Make sure that you apply the role to the user and the permissions to the user group as you see fit in your environment. Also, make sure to set a reasonable expiration date for your API tokens, so we can make sure to regenerate them from time to time.



There's not much else to say about setting up and managing your API tokens, but let's see how we can apply what we have learned in this recipe in the next recipes.



**Using the Zabbix API for extending functionality**



An API is your gateway to getting started with extending the functionality of any piece of software. Luckily, Zabbix offers a solid working API that we can use to extend our functionality with ease.



In this recipe, we'll explore the use of the Zabbix API to do some tasks, creating a good basis to start working with the Zabbix API in your actual production environments.



**Getting ready**



We are going to need a Zabbix server with some hosts. I'll be using our host lar-book-centos from the previous chapters, but feel free to use any Zabbix server. I will also use another Linux host to do the API calls from, but this can be done from any Linux host.



We will need to install Python 3 on the Linux host, though, as we'll be using this to create our API calls.



Also, make sure you have an API user with an API token. It is recommended to use the one we created in the first recipe.



**How to do it…**



First, on our Linux CLI let's move to a new directory: **cd /home/**



Install Python 3 on the host with the following command: For RHEL-based systems:



​	
 	

**dnf install python3**

366	Extending Zabbix Functionality with Custom Scripts and the Zabbix API





For Ubuntu systems:





**apt install python3**



Python pip should've been installed with this package by default as well. If not, issue the following command:



For RHEL-based systems:



**dnf install python3-pip**



For Ubuntu systems:



**apt install python3-pip**



Now let's install our dependencies using Python pip. We'll need these dependencies as they'll be used in the script:





**pip3 install requests**



Download the start of our script from the Packt GitHub repo by issuing the following command:





**wget https://raw.githubusercontent.com/PacktPublishing/ Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/main/ chapter10/api_test.py**



If you can't use wget from your host, you can download the script at the following URL: [https://github.com/PacktPublishing/Zabbix-6-IT-](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10/api_test.py)[Infrastructure-Monitoring-Cookbook/tree/main/chapter10/](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10/api_test.py) [api_test.py](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10/api_test.py).



Next up, we are going to edit our newly downloaded script by executing the following command:





**vim api_test.py**



First, let's change the IP address 10.16.16.152 in the url variable to the IP

or DNS of your Zabbix server. Then make sure to edit the api_token variable by replacing PUT_YOUR_TOKEN_HERE with the API token we generated in the first recipe of this chapter:





**url = "http://10.16.16.152/zabbix/api_jsonrpc.php" api_token = "c01ce8726bfdbce02664ec8750f99 da1bbbcb3cb295d924932e2f2808846273"**



Using the Zabbix API for extending functionality	367





We will also add some lines of code to our script to retrieve our host ID, hostname, and the interfaces of all our Zabbix hosts. Make sure to add your new code between the comments shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_101bbfcb7b25c132.jpg) 





















Figure 10.8 – Comments showing where to put code



\10. Now add the following lines of code:





\#Function to retrieve the hosts and interfaces def get_hosts(api_token, url):





payload = {





"jsonrpc": "2.0",





"method": "host.get",





"params": {





"output": [





"hostid",





"host"





],





"selectInterfaces": [





"interfaceid",





"ip",





"main"





]





},





"id": 2,





"auth": api_token





}





resp = requests.post(url=url, json=payload )





out = resp.json()





return out['result']

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Then, we'll also add lines to write the requested information to a file so we can see what happens after execution:





\#Write the results to a file





def generate_host_file(hosts,host_file):





hostname = None





f = open(host_file, "w")





\#Write the host entries retrieved from Zabbix for host in hosts:



hostname = host['host']





for interface in host["interfaces"]:





if interface["main"] == "1":





f.write(hostname + " " + interface["ip"]



\+ "\n")





f.close()





return



You should be able to execute this now by executing the following: **python3 api_test.py**



This should run but it won't give you any output. If this doesn't work, make sure to retrace your steps.



Let's check out the file to see what happened by executing the following:





**cat /home/results**

Using the Zabbix API for extending functionality	369





\15. The output of the preceding command should look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_f7ebcd36ab76536c.jpg) 













































Figure 10.9 – The cat command with our results showing in the file



We've now written a short script in Python to use the Zabbix API.



**How it works…**



Coding with the Zabbix API can be done with Python, but it's definitely not our only option. We can use a wide variety of coding languages, including Perl, Go, C#, and Java.



In our example though, we've used Python, so let's see what we do here. If we look at the script, we have two main functions:



get_hosts



generate_host_file

370	Extending Zabbix Functionality with Custom Scripts and the Zabbix API





First, we filled in our api_token and url variables, which are used to authenticate against the Zabbix API. We then use these to call on the get_hosts function to retrieve information from the Zabbix API:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_3918a23418b9cf92.jpg) 











































Figure 10.10 – Python function Zabbix API payload



Looking at the code, we use a JSON payload to request information such as host for the hostname, hostid for the host ID, and ip for the interface's IP address.



Now, if we look at our last function, generate_host_file, we can see that we write the host with an interface IP to the /home/results file. This way we have a solid script for writing host information to a file.



If you're not familiar with Python or coding in general, working with the Zabbix API might be a big step to take. Let's take a look at how the API actually works:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_bd4db6ba5363ee78.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 10.11 – Python script Zabbix API functionality diagram

Building a jumphost using the Zabbix API and Python	371





In *step 1*, we make an API call, using our target URL and API token as specified in our variables for authentication. Next, in *step 2*, we receive the data as requested in our Python function from Zabbix to further use in our Python script.



*Step 3* is our data processing step. We can do anything we want with the data received from the Zabbix API, but in our case, we format the data and write it to a file. That's how we use the Zabbix API for extending functionality. This is the step where our file gets filled with hostnames and IP information.



**See also**



If you are interested in learning more about the Zabbix API, check out the Zabbix documentation at [https://www.zabbix.com/documentation/current/en/](https://www.zabbix.com/documentation/current/en/manual/api) [manual/api](https://www.zabbix.com/documentation/current/en/manual/api).



**Building a jumphost using the Zabbix API and Python**



A lot of organizations have a jumphost (sometimes referred to as a bastion host) to access servers, switches, and their other equipment from a host. A jumphost generally has all the firewall rules needed to access everything important. Now if we keep our monitoring up to date, we should have every single host in there as well.



My friend, ex-colleague, and fellow Zabbix geek, *Yadvir Singh*, had the amazing idea to create a Python script to export all Zabbix hosts with their IPs to the /etc/hosts file on another Linux host. Let's see how we can build a jumphost just like his.



**Getting ready**



We are going to need a new host for this recipe with Linux installed and ready.



We'll call this host lar-book-jump. We will also need our Zabbix server, for which I'll use lar-book-centos.



Also, it is important to navigate to *Yadvir's* GitHub account, drop him a follow, and star his repository if you think this is a cool script: [https://github.com/cheatas/](https://github.com/cheatas/zabbix_scripts) [zabbix_scripts](https://github.com/cheatas/zabbix_scripts).

372	Extending Zabbix Functionality with Custom Scripts and the Zabbix API





**Important Note**



Setting up this script will override your /etc/hosts file every time the script is executed. Only use this script when you understand what it's doing, make sure you use an empty host for this lab, and check the default /etc/ hosts settings.





**How to do it…**



If you haven't already created an API user with an API token, make sure to check out the first recipe of this chapter first.



Install Python 3 on the host CLI with the following command: For RHEL-based systems:

**dnf install python3**



For Ubuntu systems:



**apt-get install python3**



Python pip should've been installed with this package by default as well. If not, issue the following command:



For RHEL-based systems:



**dnf install python3-pip**





For Ubuntu systems:





**apt-get install python3-pip**



Now let's install our dependencies using Python pip. We'll need these dependencies as they'll be used in the script:





**pip3 install requests**



First things first, log in to our new Linux host, lar-book-jump, and download Yadvir's script to your Linux host with the following command:





**wget https://raw.githubusercontent.com/cheatas/zabbix_ scripts/main/host_pull_zabbix.py**

​	
 	

​	If you can't use wget from your host, you can download the script at the following URL: [https://github.com/cheatas/zabbix_scripts/blob/main/](https://github.com/cheatas/zabbix_scripts/blob/main/host_pull_zabbix.py) [host_pull_zabbix.py](https://github.com/cheatas/zabbix_scripts/blob/main/host_pull_zabbix.py).

Building a jumphost using the Zabbix API and Python	373





As a backup, we also provide this script in the Packt repository. You may download this version at [https://github.com/PacktPublishing/Zabbix-6-IT-](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10/host_pull_zabbix.py)[Infrastructure-Monitoring-Cookbook/tree/main/chapter10/](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10/host_pull_zabbix.py) [host_pull_zabbix.py](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter10/host_pull_zabbix.py).



Now let's edit the script by executing the following command:





**vim host_pull_zabbix.py**



First, let's edit the zabbix_url variable by replacing https://myzabbix. com/api_jsonrpc.php with the IP address or DNS name of our



Zabbix frontend:





**zabbix_url = "http://10.16.16.152/zabbix/api_jsonrpc.php"**



We do not need to fill out our username and password as that was only required on older Zabbix versions. Instead, we will need an API token, as generated in the first recipe in this chapter. Fill in the api_token variable in the script as follows:





**api_token = "c01ce8726bfdbce02664ec8750f99da1bbbcb3cb295 d924932e2f2808846273"**



\11. We also need to uncomment the following lines:





**zabbix_hosts = get_hosts(api_token,zabbix_url)**





**generate_host_file(zabbix_hosts,"/etc/hosts")**



\12. The end of the script should now look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_3ee411685d747392.jpg) 

























Figure 10.12 – End of the script after receiving the API token and with commenting removed

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Last but not least, make sure to comment and uncomment the right lines for your Linux distro. It will look like this:



Ubuntu:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_5e43b2f547a1cf94.jpg) 































Figure 10.13 – Print to file for Ubuntu systems



For RHEL-based systems:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_6dcf6f472763afa1.jpg) 































Figure 10.14 – Print to file for RHEL-based systems



That's all there is to do, so we can now execute the script again and start using it. Let's execute the script as follows:





**python3 host_pull_zabbix.py**

Building a jumphost using the Zabbix API and Python	375





\15. Test whether it worked by looking at the host file with the following command:





**cat /etc/hosts**



This should give us an output like that in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_e33c828ef9e31d2a.jpg) 









































Figure 10.15 – /etc/hosts filled with our script information



We can now try to SSH directly to the name of a host, instead of having to use the IP, by issuing the following command:





**ssh lar-book-agent_passive**



We can also use it to find hosts from the file with the following command: **cat /etc/hosts | grep agent**



Let's do one more thing. We want this script to be as up to date as possible. So, let's add a cronjob. Issue the following command to add a cronjob:





**crontab -e**



Then add the following line, making sure to fill in the right script location for your setup:





***/15 \* \* \* \* $(which python3) /home/host_pull_zabbix.py >> ~/cron.log 2>&1**

​	
 	

​	That's it – we will now have an up-to-date /etc/hosts file all the time with our new Python script and Zabbix.

376	Extending Zabbix Functionality with Custom Scripts and the Zabbix API





**How it works…**



If your organization uses Zabbix as the main monitoring system, you now have the skills and knowledge to create an organized, reliably up-to-date, and easy-to-use jumphost.



Jumphosts are super useful when set up correctly, but it's important to keep them clean so that they are easy to update.



By using this script, we only add Python 3 and a simple script as a requirement to the server, but the end result is a jumphost that knows about all hosts in the environment.



If you've followed along with the previous *Using the Zabbix API for extending functionality* recipe, then you might notice that it works in roughly the same way. We can see in the following diagram how we utilize the script:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_905f1412c793d2f1.jpg) 























Figure 10.16 – Jumphost using script functionality diagram



After editing, our script will start at *step 1* of the diagram to request data with an API call, where we use the API token to authenticate. We receive this data in *step 2*. In the script, we add our default values and then write all the hostnames and IP addresses to the /etc/ hosts file.



Now, because a Linux host uses the /etc/hosts file for hostname-to-IP translation,

we can use the real names of servers in Zabbix to SSH to the hosts. This makes it easier for us to use the jumphost, as we can use the same name as the hostname we know from the Zabbix frontend.



**See also**



*Yadvir* will keep updating the script after writing this recipe (we've been using version 1.0 so far). Make sure to follow his GitHub account and star his repository to get



the updates. Also, if you have some cool ideas for additions, you can always open a pull request.

Creating maintenance periods as a Zabbix user	377





The Zabbix community is all about sharing cool ideas and useful scripts like this one. As *Yadvir* has shown, we can get very valuable stuff from each other. Be like *Yadvir*– use the Zabbix community GitHub and support other Zabbix users by adding to their GitHub repositories. We can find the Zabbix community GitHub using the link here:



https://github.com/zabbix/community-templates



**Creating maintenance periods as a Zabbix user**



It used to not be possible to schedule maintenance periods as a Zabbix user, which is why we created these scripts. In Zabbix 6, we can actually make it possible by using user roles, but these scripts are still relevant to provide quick access to maintenance creation. For some companies, this may be very useful, to limit the number of navigation actions required to do things using the frontend. In this recipe, I will show you just how to work with this Python script.



**Getting ready**



For this recipe, all we are going to need is our Zabbix server, some knowledge of Python, and some knowledge of the Zabbix API.



**How to do it…**



First, let's log in to our Zabbix server CLI and create a new directory: **mkdir /etc/zabbix/frontendscripts**



Change to the new directory:





**cd /etc/zabbix/frontendscripts**



3.	Now download the Public script from the *Opensource ICT Solutions* GitHub:





**wget https://github.com/OpensourceICTSolutions/zabbix-maintenance-from-frontend/archive/v2.0.tar.gz**



If you can't use wget from your host, check out the script here: [https://](https://github.com/OpensourceICTSolutions/zabbix-maintenance-from-frontend/releases/tag/v2.0) [github.com/OpensourceICTSolutions/zabbix-maintenance-from-](https://github.com/OpensourceICTSolutions/zabbix-maintenance-from-frontend/releases/tag/v2.0)[frontend/releases/tag/v2.0](https://github.com/OpensourceICTSolutions/zabbix-maintenance-from-frontend/releases/tag/v2.0).



Unzip the file with the following command:



​	
 	

**tar -xvzf v2.0.tar.gz**

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Remove the tar file using the following command:





**rm v2.0.tar.gz**



Move the script over from the newly created folder with the following command: **mv zabbix-maintenance-from-frontend-2.0/maintenance.py ./**



We are going to need Python to use this script so let's install it as follows: For RHEL-based systems:

**dnf install python3 python3-pip**



For Ubuntu systems:



**apt-get install python python-pip**



We will also need the requests module from pip, so let's install it with the following command:





**pip3 install requests**



Now let's edit the script with the following command: **vim maintenance.py**



In this file, we will change the url and token variables. Change the url variable to match your own Zabbix frontend IP or DNS name. Then replace PUT_YOUR_ TOKEN_HERE with your Zabbix API token. I will fill in the following, but be sure to enter your own information:





**url = 'http://10.16.16.152/zabbix/api_jsonrpc.php?'**





**token = "**



**c01ce8726bfdbce02664ec8750f99da1bbbcb3cb295d924932**





**e2f2808846273 "**



Now we can move on to our Zabbix frontend to add a frontend script. Navigate to **Administration** | **Scripts**, then click the blue **Create script** button in the top-right corner.

Creating maintenance periods as a Zabbix user	379





\13. Now add the following script:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_2691cc319ba8a4f1.jpg) 

































































Figure 10.17 – Zabbix Administration | Scripts, create script page, 1 hour maintenance period



Click on the blue **Add** button and then, on the next page, click the blue **Create script** button in the top-right corner again.

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Add the second script as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_54217f62ffb254ac.jpg) 

































































Figure 10.18 – Zabbix Administration | Scripts, Create script page, 24 hour maintenance period



Click on the blue **Add** button and then, on the next page, click the blue **Create script** button in the top-right corner again.

Creating maintenance periods as a Zabbix user	381





\17. Add the third and final script as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_14b6212f188d59e9.jpg) 

































































Figure 10.19 – Zabbix Administration | Scripts, Create script page, delete maintenance period



Now click the blue **Add** button to finish adding the final script.



Let's test the script by navigating to **Monitoring** | **Hosts**. Here we can click on any hostname available.

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



This will then open a drop-down menu where we can select to schedule maintenance for this host:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_875e8cc9958f0e97.jpg) 

















































Figure 10.20 – Zabbix Monitoring | Hosts page with dropdown from underlined hostname



If we click on **1 hour maintenance period**, we get a confirmation window to execute the scheduling of our maintenance period:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_edf5105c021ce760.jpg) 

















Figure 10.21 – Zabbix confirmation window before execution



Click on **Execute** and then navigate to **Configuration** | **Maintenance**. We can now see our new maintenance period:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_ca2154adfe59a565.jpg) 

​	
 	


 	


 	

Figure 10.22 – Zabbix Configuration | Maintenance page showing host lar-book-agent

Creating maintenance periods as a Zabbix user	383





**How it works…**



The script we just used was built in Python utilizing the Zabbix API. With this script, we can now schedule maintenance periods from the Zabbix frontend as a Zabbix user.



This works because the **Monitoring** | **Hosts** option is available even to Zabbix users. Instead of using the user's frontend permissions, our script uses the API user for execution. Because our Zabbix API user has more user permissions, it can execute the script that gets host information from a Zabbix database and creates a maintenance period using the information. Looking at the following diagram, we can see the process of this script:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_40e5594d2e462980.jpg) 



























Figure 10.23 – Python script maintenance.py execution diagram



As we can see in the preceding diagram, our script follows roughly the same steps as other Zabbix API utilities. Since the Zabbix API is very flexible, we can pull data and write data to do almost anything we could do from the frontend.



We can now use this new function from anywhere in the Zabbix frontend where we see a dotted line with the hostname, even from Zabbix maps. We can see an example of when we clicked on such a dotted line in *Figure 10.20.*



**See also**



*Brian van Baekel* created the script used in this recipe for a customer at *Opensource ICT Solutions* and then open sourced it. Because Zabbix has a very cool community working to extend the possibilities of Zabbix even further, we too upload some of our scripts. Sharing is caring, so check out the other open-sourced scripts on the GitHub repo at [https://](https://github.com/OpensourceICTSolutions) [github.com/OpensourceICTSolutions](https://github.com/OpensourceICTSolutions).

384	Extending Zabbix Functionality with Custom Scripts and the Zabbix API





**Enabling and disabling a host from Zabbix maps**



We've noticed that it is not possible to enable and disable hosts as a Zabbix user. For some companies, this may be a requirement, so we've created an extension for it. In this recipe, I will show you just how to work with this Python script and execute it from a map.



**Getting ready**



For this recipe, all we are going to need is our Zabbix server, some knowledge of Python, and some knowledge of the Zabbix API.



**How to do it…**



First, let's log in to our Zabbix server CLI and create a new directory: **mkdir /etc/zabbix/frontendscripts**



Change to the new directory:





**cd /etc/zabbix/frontendscripts**



3.	Now download the public script from the *Opensource ICT Solutions* GitHub:





**wget https://github.com/OpensourceICTSolutions/zabbix-toggle-hosts-from-frontend/archive/v2.0.tar.gz**



If you can't use wget from your host, you can check out the script here: [https://](https://github.com/OpensourceICTSolutions/zabbix-toggle-hosts-from-frontend/releases/tag/v2.0) [github.com/OpensourceICTSolutions/zabbix-toggle-hosts-](https://github.com/OpensourceICTSolutions/zabbix-toggle-hosts-from-frontend/releases/tag/v2.0)[from-frontend/releases/tag/v2.0](https://github.com/OpensourceICTSolutions/zabbix-toggle-hosts-from-frontend/releases/tag/v2.0).



Unzip the file with the following command:





**tar -xvzf v2.0.tar.gz**



Remove the tar file using the following command: **rm v2.0.tar.gz**



Enabling and disabling a host from Zabbix maps	385





7.	Move the script over from the newly created folder with the following command:





**mv zabbix-toggle-hosts-from-frontend-2.0/enable_disable-host.py ./**



We are going to need Python to use this script, so let's install it as follows: For RHEL-based systems:

**dnf install python3 python3-pip**



For Ubuntu systems:



**apt-get install python python-pip**



We will also need the requests module from pip. Install it as follows: **pip3 install requests**



Now let's edit the script with the following command:





**vim enable_disable-host.py**



In this file, we will change the url and token variables. Change the url variable to match your own Zabbix frontend IP or DNS name. Then replace PUT_YOUR_ TOKEN_HERE with your Zabbix API token. I will fill in the following, but be sure to enter your own information:





**url = 'http://10.16.16.152/zabbix/api_jsonrpc.php?'**





**token = " c01ce8726bfdbce02664ec8750f99da1bbbcb3cb295d 924932e2f2808846273 "**



Now, we can move on to our Zabbix frontend to add a frontend script. Navigate to **Administration** | **Scripts**, then click the blue **Create script** button at the top right.

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Add the following script:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_a32681e01fce7896.jpg) 



































































Figure 10.24 – Zabbix Administration | Scripts, Create script page, Enable



Click on the blue **Add** button and then, on the next page, click the blue **Create script** button in the top-right corner again.

Enabling and disabling a host from Zabbix maps	387





\15. Now add the second and final script as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_9f160fb59eaab949.jpg) 

































































Figure 10.25 – Zabbix Administration | Scripts, Create script page, Disable



Now navigate to **Monitoring** | **Maps**, and you should see a map called **Local network** here, as it's included with Zabbix by default. Click this map (or any other map with hosts in it).

Extending Zabbix Functionality with Custom Scripts and the Zabbix API



Now, if you click on a host on the map, you will see a drop-down menu like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_9f190fdac827ea42.jpg) 









































































Figure 10.26 – Zabbix Monitoring | Maps, Local network map drop-down menu



\18. If we click on **Disable** here, we will get a pop-up message as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_54114686f2223e2e.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 10.27 – Zabbix script confirmation window

Enabling and disabling a host from Zabbix maps	389





Click on the blue **Execute** button and this host will be disabled. Navigate to **Monitoring** | **Hosts** to confirm if this worked. You should see that the host is **Disabled**.



Back at **Monitoring** | **Maps**, you can enable the host again with the same drop-down menu. This time, select **Enable**.



**How it works…**



The script we just used was built in Python utilizing the Zabbix API. With this script, we can now enable and disable hosts from the Zabbix frontend as a Zabbix user.



This works because the **Monitoring** | **Maps**option is available even to Zabbix users. This script uses the API user for execution though. Since our Zabbix API user has more user permissions, it can execute the script that gets host information from a Zabbix database and creates a maintenance period using the information. As we can see in the following diagram, our script follows roughly the same steps as the other Zabbix API utilities:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc1o9_tmp_31bd0a178bc32144.jpg) 





























Figure 10.28 – Python script maintenance.py execution diagram



Because the Zabbix API is very flexible, we can pull data and write data to do almost anything we could do from the frontend.



We can now use this cool function from anywhere in the Zabbix frontend where we see a dotted line with the hostname, even from **Monitoring** | **Hosts**.



**See also**



*Brian van Baekel* created this script for a customer at *Opensource ICT Solutions* and then open sourced it. Because Zabbix has a very cool community that continues to extend the possibilities of Zabbix even further, we too upload some of our scripts. Sharing



is caring, so check out the other open-sourced scripts at [https://github.com/](https://github.com/OpensourceICTSolutions) [OpensourceICTSolutions](https://github.com/OpensourceICTSolutions).