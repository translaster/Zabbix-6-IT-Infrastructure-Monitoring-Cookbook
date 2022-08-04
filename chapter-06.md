# 6 Visualizing Data, Inventory, and Reporting

When working with Zabbix, it is important that collected data is put to good use. After all, the data is of no use if we do not have a place to easily access it. Zabbix already puts our data to good use with the **Latest data** page and with problems created from triggers, but we can also put our data to good use by building some stuff ourselves, such as graphs, maps, and inventory, and by using reporting.

After working through these recipes, you'll be able to set up the most important parts of Zabbix data visualization. You'll also be able to make good use of your inventory and reporting systems, to get the most out of their useful features.

In this chapter, we'll go over these subjects to show you how to achieve good results in these recipes:

Creating graphs to access visual data

Creating maps to keep an eye on infrastructure

Creating dashboards to get the right overview

Visualizing Data, Inventory, and Reporting

Setting up Zabbix inventory

Working through Zabbix reporting

Setting up scheduled PDF reports

Setting up improved business service monitoring

**Technical requirements**



For this chapter, we will need our Zabbix server, our **System Network Management Protocol** (**SNMP**)-monitored host from *Chapter 5*, *Building Your Own Structured Templates*. We'll be doing most of our work in the frontend of Zabbix, so have your mouse at the ready.



The code files can also be accessed from the GitHub repository here:



[https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter06)[Monitoring-Cookbook/tree/main/chapter06](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter06)



**Creating graphs to access visual data**



Graphs in Zabbix are a powerful tool to show what's going on with your collected data. You might have already found some graphs by using the **Latest data** page, but we can also create our own predefined graphs. In this recipe, we will go over doing just that.



**Getting ready**



Make sure to get your Zabbix server ready, along with a Linux host that we can monitor (with SNMP). If you have followed along with the recipes in *Chapter 5*, *Building Your Own Structured Templates*, you should already have created your own personal template.



Alternatively, download the templates here:



[https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter6)[Monitoring-Cookbook/tree/main/chapter6](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook/tree/main/chapter6)



If you are using the downloaded templates, download and import **Template OS Linux uptime by SNMP** first, and then **Linux by SNMP**. Importing a template can be done at **Configuration** | **Templates** by clicking the blue **Import** button in the top-right corner.



Make sure to put the template on a host and monitor it.

Creating graphs to access visual data	197





**How to do it…**



Let's start by navigating to our templates by going to **Configuration** | **Templates** and selecting the template. For me, it is still called **Custom Linux by SNMP**.



Go to **Items** and create the following item on the template:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_bd83637a0e0da80d.jpg) 













































Figure 6.1 – ICMP Item creation page



3.	Make sure to go to the **Tags** tab and add a tag as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_9720b93fa979179e.jpg) 

























Figure 6.2 – ICMP Item creation page, Tags tab



Click the blue **Add** button to save this item.



Now, back at the template configuration page, go to **Graphs**. This is where we can see all of our configured graphs in this template; at the moment, there are none.

198	Visualizing Data, Inventory, and Reporting



​	
 	

​	Click **Create graph** in the top-right corner. This will take you to the graph creation page:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_51fc6142284dd850.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.3 – Graph creation page


 	

​	This is where we can create graphs for standalone items. Let's create a graph to see our uptime.


 	

7.	Fill in the graph creation page with the following information:

Creating graphs to access visual data	199

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a8318a887f600793.png) 





















































Figure 6.4 – Graph creation page filled with our information





**Tip**



When working with graphs, it's a good idea to keep colorblind people in mind. Worldwide, about 8% of all males and 0.5% of all females are affected by this condition. There are great sources online that explain which colors to use for your production environment. You can find one such source here: [https://](https://www.tableau.com/about/blog/2016/4/examining-data-viz-rules-dont-use-red-green-together-53463)



[www.tableau.com/about/blog/2016/4/examining-data-](https://www.tableau.com/about/blog/2016/4/examining-data-viz-rules-dont-use-red-green-together-53463)[viz-rules-dont-use-red-green-together-53463](https://www.tableau.com/about/blog/2016/4/examining-data-viz-rules-dont-use-red-green-together-53463)



Now, ping your SNMP-monitored host for a while. Do this from your Zabbix server **command-line interface** (**CLI**):





**ping 10.16.16.153**



Afterward, navigate to **Monitoring** | **Hosts** and click the **Graphs** button next to your host. In my case, the host is still called lar-book-templated_snmp.

Visualizing Data, Inventory, and Reporting



Now, this will immediately take us to an overview of graphs for this host, where we can see our new **Incoming ICMP messages** graph:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_da51ede3061c287e.jpg) 



































Figure 6.5 – Monitoring | Hosts graph page with our graph



We can also make graphs for discovery items; this is called a graph prototype. They work in about the same way as our item prototypes. Let's create one of these as well:



Navigate to **Configuration** | **Templates** and select our **Linux by SNMP** template.



Go to **Discovery rules**, and then, for the **Discover Network Interfaces** discovery rule, click on **Item prototypes**. In the top-right corner, click **Create item prototype** and create the following item prototype:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_97f2b296e289efe8.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.6 – Item prototype Incoming bits page filled with our information

Creating graphs to access visual data	201





3.	Then let's add a tag on the **Tags** tab:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_1c629ff038f511db.jpg) 

























Figure 6.7 – Item prototype Incoming bits, Tags tab



4.	Lastly, make sure to add the following on the **Preprocessing** page:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e9dfad591de236c4.jpg) 

































Figure 6.8 – Item prototype Incoming bits Preprocessing tab filled with our information





**Important note**



Preprocessing is quite an extensive topic. In short, the preprocessing in this step will make sure that 1: Our data is calculated as change per second, with the mathematical formula **(value-prev_value)/(time-prev_time)**, and 2: Our data is multiplied by 8 to change from bytes to bits.

​	
 	

​	Click the blue **Add** button to finish creating this item prototype.


 	

​	Now, back at our discovery rule **Discover Network interfaces**, click the **Graph prototypes** button.

202	Visualizing Data, Inventory, and Reporting



​	
 	

​	In the top-right corner, click **Create graph prototype** and fill the next page with the following information:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_69aba464d13b6873.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.9 – Graph prototype Incoming bits page filled with our information


 	

​	Now, if we go back to **Monitoring** | **Hosts** and we click the **Graphs** button, we can see two new graphs:

Creating graphs to access visual data	203

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_4bf1a2c410b839d5.png) 











































































Figure 6.10 – Graphs page for our host



It might take some time for the graph to fill up with data, as we just added the item. Give it some time and you will start to see this graph fill up.



**How it works…**



Now, graphs work simply by putting your collected values in a visual form. We collect our data from our host—through SNMP, for example—and we put that data in our database. Our graphs in turn collect this data from the database and put it in this visual form. For us, this is a lot better to read, and we can interpret the data easily.

204	Visualizing Data, Inventory, and Reporting





The graph prototype works in almost the same way as our item prototype. For every single discovered interface, we create a graph using a name containing the {#IFNAME} **Low-Level Discovery** (**LLD**) macro. This way, we get a versatile structured environment because when a new interface is created (or deleted), a new graph is also created (or deleted).



**Creating maps to keep an eye on infrastructure**



Maps in Zabbix are a great way to get an overview of infrastructure—for instance,



they're amazing for following traffic flows or seeing where something is going off in your environment. They're not only super-useful for network overviews but also for server management overviews, and even for a lot of cool customization.



**Getting ready**



We will need our Zabbix server, our SNMP-monitored host, and the templates from the previous recipe.



**How to do it…**



Let's start this recipe off by navigating to **Configuration** | **Template**and selecting our **Linux by SNMP** template.



Go to **Discovery rules** and then **Item prototypes**. Create the following item prototype by filling in the fields in the **Item prototype** creation page:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_f62e459c87c0c1bf.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.11 – Item prototype creation page

Creating maps to keep an eye on infrastructure	205





We'll also need to go to **Tags** to add a new tag, like the one in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_eeeab265385ba2ff.jpg) 























Figure 6.12 – Item prototype creation page, Tags tab



4.	Lastly, do not forget to add preprocessing by going to the **Preprocessing** tab.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_1bbc09b67db57861.jpg) 

































Figure 6.13 – Item prototype Preprocessing creation page



5.	Click on the blue **Add** button to finish.

206	Visualizing Data, Inventory, and Reporting



​	
 	

​	Next up, navigate to **Monitoring** | **Maps**. There's already a default map here included in all Zabbix server installs, titled **Local network**. Feel free to check it out:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_1102e3d39a752ccd.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.14 – The default Local network map


 	

​	There's not much to see here, besides your local Zabbix server host and if it is in a problem state or not. So, let's click on **All maps**.

​	We are going to create our own map, so click the **Create map** button in the top-right corner. Create the map by filling in the following fields:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_82cfbc058835c58c.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.15 – Map creation page

Creating maps to keep an eye on infrastructure	207





After clicking the blue **Add** button, the frontend will take you back to the **Map** overview page. Click the newly created Templated SNMP host map here.



Click **Edit map** in the top-right corner to start editing the map.



Now, what we want to do here is select the **Add** button next to **Map element**, which is in the horizontal menu at the top of the map. This will add the following element:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_d05488330c94128e.jpg) 





























Figure 6.16 – The added element



\12. Click the newly added element—this will open the following screen:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_4a57ebb2d3d685a5.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.17 – New Map element edit window

Visualizing Data, Inventory, and Reporting



Now, here we can fill out our host information. Let's add the following information to the fields:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a75b2f189ae4e243.jpg) 















































































Figure 6.18 – Map element lar-book-templated_snmp edit window



Click **Apply** and move the element by dragging it to **X**:400 and **Y**:200 (see *Figure 6.20*).



Now, add another element by clicking the **Add** button next to **Map element**. Edit the new element and add the following information.

Creating maps to keep an eye on infrastructure	209

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_17815eb64c85c690.png) 













































Figure 6.19 – Map element Vswitch edit window filled with information



After creating both elements, let's move the new switch element to **X**:150 and



**Y**:200, as seen in *Figure 6.20*.



Now, select both elements by holding the *Ctrl* key (*Command* on a Mac) on your keyboard.



Then, click **Add** next to **Link** to add a link between the two elements. It should now look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_9866d635b649a21e.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.20 – Our newly created map

Visualizing Data, Inventory, and Reporting



Edit the information for our server again after creating the link by clicking on our icon. Click on **Edit** next to the newly created link, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e9469c13fdc21a2d.jpg) 











Figure 6.21 – Edit link in the Map element edit window



\18. Add the following information to the window:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e8e9a1b914de0cd1.jpg) 



































Figure 6.22 – Edit the link in the Map element edit window with our information



Let's also click **Add** in the **Link indicators** section and add the following trigger with the color red:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_c0141b45a27a4cca.jpg) 













Figure 6.23 – Link indicator filled with a trigger



Now, click **Apply** at the bottom of the window and then **Update** in the top-right corner of the page. That's our first map created!

Creating maps to keep an eye on infrastructure	211





**How it works…**



Now, after creating and opening our map, we can see the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_df442b2272b1ce04.jpg) 













Figure 6.24 – Our newly created map



The map shows our switch (which is not a monitored host at the moment) and our server (which is a monitored host). This means that when something is wrong with our server, the **OK** status will turn into a **PROBLEM** status on the map.



We can also see our configured label (see *Figure 6.24)*, which is showing us real-time information of traffic statistics. Now, when we break down the label, we get the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a9ac2dd8634c1b74.jpg) 





















Figure 6.25 – Map label breakdown



We can pull real-time statistics into a label by defining which statistics we want to pull into the label between {}. In this case, we collect our values for interface traffic and put them directly in the label, creating a real-time traffic analysis map.



We also put a trigger on this link. The cool thing about putting triggers such as this on our map is that when our link goes down, we can see the following happen:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_d0fec06424fed336.jpg) 













Figure 6.26 – Map showing problems

212	Visualizing Data, Inventory, and Reporting





Traffic stopped flowing because the link is now down, and our line has turned red. Also, our host is now showing a **PROBLEM** state under the hostname.



We can even create orange lines with triggers that state 50% traffic utilization like this and trace **Distributed Denial of Service** (**DDoS**) traffic through our network.



**Creating dashboards to get the right overview**



Now that we've created some graphs and a map, let's continue on by not only visualizing our data but also getting the visualization in an overview. We're going to create a dashboard for our Linux-monitored hosts.



**Getting ready**



Make sure you followed the previous two recipes and that you have your Zabbix server ready. We'll be using our SNMP-monitored host from the previous recipe, as well as our item, triggers, and map.



**How to do it…**



Navigate to **Monitoring** | **Dashboards** and click **All dashboards** in the left corner of the page.



Now, click the **Create dashboard** button in the top-right corner and fill in your dashboard name, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_1cf6736dcbe231b7.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.27 – Create dashboard window

Creating dashboards to get the right overview	213





**Tip**



Keeping Zabbix elements such as maps and dashboards owned by the Zabbix **Admin** user is a great idea. This way, they aren't dependent on a single user who might leave your environment at a later stage. The elements can be owned by a disabled user as well.



3.	Now, click **Apply**, and you'll be taken to your dashboard immediately:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_f8145cbf36314e50.jpg) 













































Figure 6.28 – New empty dashboard



After creating our dashboard, we will see that it is empty. We need to fill it with several widgets to create a good overview.

214	Visualizing Data, Inventory, and Reporting



​	
 	

​	Let's start by adding a problem widget. Click **+ Add widget** in the top-right corner. Add the following widget by filling out all the fields:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_bcdc1b3e73c37ad2.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.29 – New problem widget creation window

​	
 	

​	Click **Add**, and we have our first widget on our dashboard, displaying all **Unacknowledged Problems**. It will only show them for the **Severity** warning and higher on all Linux servers:

Creating dashboards to get the right overview	215

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_57ca306bf59ec757.png) 

































Figure 6.30 – Our Unacknowledged Problems widget



Let's immediately add some more widgets, starting with our **Map** widget. Click **+ Add widget** in the top-right corner, and add the following widget:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_d536242b2a3135d8.jpg) 

































Figure 6.31 – New Map widget creation window



Also, add a **Graph** type widget by clicking **+ Add widget** in the top-right corner again. This one is a bit more difficult. Let's add our name first:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_b83d7f1c08343701.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.32 – New Graph widget creation window

Visualizing Data, Inventory, and Reporting



Then, we need to add our first **Data set**, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e96337a2fc19ab80.jpg) 





































Figure 6.33 – Adding data set in New Graph widget creation window



9.	Then, add a second one by clicking **+ Add new data set** and adding the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_c077647b1238edc1.jpg) 









































Figure 6.34 – Adding another dataset in the New Graph widget creation window



\10. We can then click **Add**, and our graph will be added to our dashboard.

Creating dashboards to get the right overview	217





Let's also add the **Item value** widget to the page. Click on **+ Add widget** again. Then set up the following widget:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_649c97b200cf77f4.jpg) 















































Figure 6.35 – Adding the widget Item value



If you are interested in changing exactly how this widget looks, be sure to use the **Show** and **Advanced configuration** fields in this widget configuration screen.



Another we personally love is the very useful new **Top hosts** widget. Let's add it by using the **+ Add widget** button again.



On the widget configuration screen, set up the **Host groups** as Linux servers.



Next, click on the **Add** button next to **Columns** to add a column with information. Fill out the form like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_29c6006cb2bcbb59.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.36 – Top hosts widget 1, column 1

218	Visualizing Data, Inventory, and Reporting



​	
 	

\15. Click on the **Add** button next to **Columns** again and add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_2b96144a73b67d0e.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.37 – Top hosts widget 1, column 2


 	

\16. The end result should look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e13d9d4fb8d37953.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.38 – Top hosts widget 1

Creating dashboards to get the right overview	219





Do not forget to click the blue **Add** button at the bottom of the form to save the changes.



Let's create one more **Top hosts** widget by using the **+ Add widget** button again.



Set up the **Host groups** as Linux servers again. Then click on the **Add** button next to **Columns** again. Add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_10117bfdc110c62b.jpg) 























Figure 6.39 – Top hosts widget 2, column 1



\20. Click on the **Add** button next to **Columns** again and add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a76dff1caf793ee8.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.40 – Top hosts widget 2, column 2

220	Visualizing Data, Inventory, and Reporting



​	
 	

\21. The end result will look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_aea42fd6531f2488.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.41 – Top hosts widget 2

Creating dashboards to get the right overview	221





\22. We can then freely move around the widgets until we get this, for example:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_4393da5c98f4f087.jpg) 

















































Figure 6.42 – Our dashboard with information



\23. Click **Save changes** in the top-right corner, and you are done.



**How it works…**



Creating dashboards is the best way to create overviews for quick access to data during troubleshooting, day-to-day problem monitoring, and—of course—for use with big TV walls. We've probably all seen the big operation centers with TVs displaying data. Zabbix is great for all these purposes and more, as you can see from this recipe.





**Important note**



Zabbix has removed the **Screens** functionality, which is why you might be confused about where it went. **Screens** is fully replaced by **Dashboards**, and thus we will only be working with building dashboards now. **Dashboards** have the **slideshow** and host-specific functionality now.

222	Visualizing Data, Inventory, and Reporting





**Setting up Zabbix inventory**



Zabbix inventory is a feature I personally love, but it hasn't had a lot of love from the Zabbix development team lately. Sorry—I still love you, Zabbix developers, but if you're reading this, feel free to put some time into the feature!



The feature makes it possible for us to automatically put collected data in a visual inventory in the Zabbix frontend. Let's get started.



**Getting ready**



Make sure to log in to the Zabbix frontend and keep your SNMP-monitored host from the previous recipes ready.



**How to do it…**



Let's start by making sure our Zabbix server puts all of our hosts' inventory information into the fields. I like to do this by going to **Administration** | **General** and then selecting **Other** from the dropdown in the top-left corner.



We can then set our **Default host inventory mode** parameter to **Automatic**. Don't forget to click **Update**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_86e0d1170406a0f2.jpg) 



































Figure 6.43 – Administration | General | Other configuration parameters page

​	
 	

​	Alternatively, we can do this at a host level. Go to **Configuration** | **Hosts** and select our lar-book-templated_snmp SNMP-monitored host.

Setting up Zabbix inventory	223





Select **Inventory** and set it to **Automatic** here as well. As you may have noticed, the default applies only to newly created hosts from now on.





**Important note**



Changing the global setting does not apply it to all existing hosts but only to newly created hosts. It might be a good idea to run a **Mass update** for all the hosts, or change the inventory mode manually, host by host.



Now let's go to **Configuration** | **Templates** and select **Linux by SNMP**.



Go to **Items** and edit **System hostname**. We have to change the **Populates host inventory field** setting, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_81b4750565e054e4.jpg) 







Figure 6.44 – Edit item page



7.	Click **Update** and navigate to **Inventory** | **Hosts**. You will see the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_526a1d23c044a821.jpg) 











Figure 6.45 – Inventory | Hosts page



**How it works…**



Zabbix inventory is simple but underdeveloped at the moment. It's not amazing to filter to a point where it shows exactly what we want to see, but it can be very useful nonetheless.



If you are working with a lot of equipment, such as in a **managed service provider** (**MSP**) environment, it can become overwhelming to log in to every device and get the serial number by hand. If you poll the serial number and you populate the **inventory** field, suddenly you have an active list of up-to-date serial numbers.



The same of course works with anything from hardware information to software versions. We could get the active operating system versions from devices and generate an extensive list of all our operating system versions: very useful if you ever have to patch something, for example.



Use Zabbix inventory wisely when creating items, and put the population to **Automatic**, like we did in this chapter—you'll then never even have to think too much about the feature. You configure it almost automatically this way, and have nice lists waiting for you when you are in need of them.

224	Visualizing Data, Inventory, and Reporting





**Using the new Zabbix Geomap widget**



Now that we have seen how to create dashboards in the previous recipe, let's set up another dashboard. We'll use this one to create a full-fledged geographical overview of some of our hosts in Zabbix. We'll do this by using the Zabbix inventory functionality we have just learned how to use.



**Getting ready**



All we need for this recipe is our Zabbix setup with access to the frontend. It is also smart to follow the previous two recipes about dashboards and inventory. If you have not followed those yet, it is recommended to follow them first.



**How to do it…**



Using the Zabbix Geomap functionality is quite easy. We simply need to use our Zabbix inventory on our hosts in combination with a dashboard widget:



First, let's navigate to our **Configuration** | **Hosts** page and edit one of our hosts. I'll be using the lar-book-templated_snmp host.



Go to the **Inventory** tab and make sure that it is set to **Manual** or **Automatic**.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_4bd4dbd6b30655e8.jpg) 







Figure 6.46 – Inventory mode selector on the Zabbix host Inventory tab



3.	Now, in the **Location latitude** and **Location longitude** fields, fill in the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_7032cfd18be522b0.jpg) 













Figure 6.47 – Inventory tab fields on a Zabbix host



Click on the blue **Update** button to save these changes.



Back at **Configuration** | **Hosts**, let's do the same thing for another host. I'll use lar-book-agent_simple.

Using the new Zabbix Geomap widget	225





Go to the **Inventory** tab and fill in the **Location latitude** and **Location longitude** fields again:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_6bb722f07216c6a7.jpg) 











Figure 6.48 – Inventory tab fields on another Zabbix host



Click on the blue **Update** button to save these changes.



Now let's go to **Monitoring** | **Dashboards** and, at **All dashboards**, create a new dashboard or use our existing **Linux servers** dashboard, which is what I'll do.



Click on the blue **Edit dashboard** button in the top-right corner and use the **Add** button dropdown to click on **Add page**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_30ccb2486dda957.jpg) 























Figure 6.49 – Existing dashboard Add page button



\10. We'll add the following new page.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e728afaf9d1579be.jpg) 























Figure 6.50 – Existing dashboard Add page properties



\11. Click on **Apply** to add this new page.

226	Visualizing Data, Inventory, and Reporting



​	
 	

​	We can now add our Geomap widget simply by clicking anywhere on the page. Fill it in as follows.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_6b7b59f56cf16163.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.51 – Zabbix Geomap widget properties


 	

​	Click on **Apply** to save the widget configuration.


 	

​	We can now click on the blue **Save changes** button in the top-right corner to save our dashboard changes.

Using the new Zabbix Geomap widget	227





This will take us back to our dashboard, where we can click the **Geomap**page of this dashboard.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_bdb12c17b9420a16.jpg) 























































Figure 6.52 – Zabbix Geomap widget on a dashboard with two hosts



We now have a functioning Geomap in our Zabbix dashboard, using the latitude and longitude that are available in our Zabbix inventory.



**How it works…**



Instead of creating an entire new **Monitoring** | **Geomap** page, Zabbix has chosen to include this new feature using a widget, giving us the option to create even more advanced dashboards. It's important to note here that Zabbix also made the choice to use existing inventory data. Because it is possible to automatically fill in inventory data as we have seen in the *Setting up Zabbix inventory* recipe, we can also automate our Geomap widget content.



So, whether you go the manual route or the automatic route, the Geomap widget is a valuable extension of our dashboard. In general, Zabbix is extending the dashboard functionality quite a bit by including a bunch of new widgets in Zabbix 6.

228	Visualizing Data, Inventory, and Reporting





We will also set up our Zabbix automatic reporting in this chapter of the book, which will also use the dashboard functionality. If you'd like you can combine a Geomap widget with your automatic report to send out a geographical report. The key takeaway here is that Zabbix is building interoperability between components and giving us flexibility in the way we want to use a new widget like this.



When working with the initial release of the Geomap widget, some people asked us if it was possible to change the kind of map used by the Geomap widget. If we navigate to **Administration** | **Geographical maps** we can actually choose a number of built-in map providers:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_cdf715606d0f1d97.jpg) 

































Figure 6.53 – Zabbix Administration | Geographical maps options



If that is not enough, it is also possible to add our own custom map provider using the



**Other** option under **Tile provider**. Simply fill in the form and you are all set:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_783c4f65e9be4d87.jpg) 





























Figure 6.54 – Zabbix Administration | Geographical maps other form

​	
 	

​	A lot of possibilities have been added through this single widget, as you can see. One of the most requested features from the Zabbix community, we can now set it up and use it in the latest Zabbix releases.

Working through Zabbix reporting	229





**Working through Zabbix reporting**



Zabbix reporting got some well-deserved love from Zabbix development, especially with regard to getting reports out of the system and improving the audit log. First, let's take a look at some powerful features to show you exactly what's going on with your statistics right from the Zabbix frontend. Then, in the next recipe, we will take a look at how to create automatic PDF reports, a new and much-anticipated feature.



**Getting ready**



For this recipe, all you'll need is the Zabbix frontend and a monitored host. I'll be using the SNMP-monitored host from the previous recipes.



**How to do it…**



There isn't anything to configure really, as reporting is present in Zabbix from the start. So, let's dive into what each page of reporting offers us.



**System information**



If you navigate to **Reports** | **System information**, you will find the following table:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_681423787089a7b9.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.55 – Reports | System information page

230	Visualizing Data, Inventory, and Reporting





You might have seen this table before, as it is also configurable as a dashboard widget. This page gives us all of the quick information we need about our Zabbix server, such as the following:



**Zabbix server is running**: Informs us whether the Zabbix server backend is actually running and where it is running. In this case, it's running, and it's running on localhost:10051.



**Number of hosts**: This will detail to us the number of hosts **enabled**(7), the number of hosts **disabled** (0), and the number of **templates** we have (146). It gives us a quick overview of our Zabbix server host information.



**Number of items**: Here, we can see details of our Zabbix server's items—in this case, **enabled** (318), **disabled** (1), and **not supported** (38).



**Number of triggers**: This details the number of triggers to us. We can see how many are **enabled** (151) and **disabled** (1), but also how many are in a **problem** state (7) and how many are in an **ok** state (144).



**Number of users (online)**: The first value details the total number of users. The second value details the numbers of users currently logged in to the Zabbix frontend.

**Required server performance, new values per second**: Perhaps I'm introducing you to a completely new concept here, which is **New Values Per Second**, or **NVPS**. A server receives or requests values through items and writes this to our MariaDB database (or another database). The NVPS information detailed here shows the estimated number of NVPS received by the Zabbix server. Keep a close eye on this as your Zabbix server grows, as it's a good indicator to see how quickly you should scale up.



You might also see three additional values here depending on your setup:



**Database history tables upgraded**: If you see this, it's indicating that one of your database history tables hasn't been upgraded yet. Numeric (float) tables have been expanded to allow for more characters to be saved per data point. This table isn't upgraded automatically coming from Zabbix 4 to 5 or higher, as not everyone needs it and it might take a long time to upgrade.



**Database name**: If you see the name of your database with the value of your version it might indicate you are running a non-supported database version. You could see a message like Warning! Unsupported <DATABASE NAME> database server version. Should be at least <DATABASE VERSION>.



**High availability cluster**: If you are running a Zabbix server high availability cluster you will see if it is enabled here and what the failover delay is. Additionally, the **System information** page will display additional high availability information.

Working through Zabbix reporting	231





**Availability report**



Navigating to **Reports** | **Availability report** will give us some useful information about how long a trigger has been in a **Problems** state versus an **Ok** state for a certain time period:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_81ca0d66e92c9177.jpg) 





















Figure 6.56 – Reports | Availability report page



Looking at one of our hosts, we can see that in the last 30 days, the **Zabbix agent is not available (for 3m)** trigger has been in a **Problems** state for **10.6665%** of the time. This might be useful for us to know so that we can define how often a certain problem arises.



**Trigger top 100**



Navigating to **Reports** | **Trigger top 100**, we will find the top 100 triggers that have been firing in a certain amount of time:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_fc0a0b540df2f9b0.jpg) 

















Figure 6.57 – Reports | Trigger top 100 page



For my Zabbix server, the busiest trigger was a **Disk read/write** trigger on a server. It's a useful page to see what we are putting most of our time into, problem-wise.

232	Visualizing Data, Inventory, and Reporting





**Audit**



The audit log, which is a small addition to Zabbix, is found at **Reports** | **Audit**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_2153fdb7b7863cce.jpg) 



























Figure 6.58 – Reports | Audit log page



We can see which user has done what on our Zabbix server—identifying a culprit for something that shouldn't have been done, for instance.



**Action log**



When we go to **Reports** | **Action log**, we land on a page that shows which actions have been fired. If you've configured **Actions**, then you can get a list here, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_d7e9b43506d95a6.jpg) 





























Figure 6.59 – Reports | Action log page



If you're not sure if your action succeeded, then look at this list. It is very useful to troubleshoot your actions to a point where you get them up and running as you want.



When you hover over the **Info** box, you also get to see what went wrong. For example, for the **Failed** items on my Zabbix instance, I should define the appropriate media type for the **Admin** user:

Setting up scheduled PDF reports	233

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_cdb2ce37ca2286aa.jpg) 

















Figure 6.60 – Reports | Action log info box



**Notifications**



Last, but not least, navigating to **Reports** | **Notifications** will show us the number of notifications sent to a certain user for a period of time:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a1c59f64599afbb0.jpg) 

















































Figure 6.61 – Reports | Notifications page



In my case, 50 notifications have been sent to the **Admin** user this year, and 0 to other users.



**Setting up scheduled PDF reports**



This much-wanted feature was added in Zabbix 5.4 and is now finally available in an LTS release starting from Zabbix 6.0: sending automatic PDF reports through email. Now, let me start off by stating that this implementation might not fully cover every Zabbix user's situation yet. What this feature does is basically take a screenshot of any Zabbix dashboard and send it through email. It's the first setup from the Zabbix developers and I think we should really appreciate it for what it is.

234	Visualizing Data, Inventory, and Reporting





On top of that, it is a very flexible way of implementing this, as we can choose any kind of widget with filters to send in an automatic report. On top of that it gives the Zabbix development team the flexibility to add new widgets on the fly, which immediately work with your PDF reports.



**Getting ready**



We will need an existing Zabbix installation with access to the frontend and the CLI. We can use the server we have been using throughout this book for this or you can use your own installation.



In the case of a multi-host setup, the easiest method is to install this where the Zabbix server is also running, but it is possible to run this on any host. In the example, we've used our single-host installation.



You will also need to have set up a user with an email media type.



**How to do it…**



To get started with Zabbix scheduled reports, we need to install some things to our Zabbix server first:



Let's log in to our Zabbix server CLI and execute the following command to install the Google Chrome browser.



On RHEL-based systems:





**wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm**





**dnf install google-chrome-stable_current_x86_64.rpm**



On Ubuntu systems:





**wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb**





**apt install google-chrome-stable_current_amd64.deb**



Now let's install our required Zabbix web services package with the following commands.



On RHEL-based systems:



**yum install zabbix-web-service**



On Ubuntu systems:



​	
 	

**apt install install zabbix-web-service**

Setting up scheduled PDF reports	235





Now let's edit our new Zabbix web service configuration file: **vim /etc/zabbix/zabbix_web_service.conf**



We can find a bunch of Zabbix web-service-specific parameters here, including encryption. Make sure the following line is set up to match your Zabbix server(s) IP(s):





**AllowedIP=127.0.0.1,::1**



Now let's edit our Zabbix server configuration file: **vim /etc/zabbix/zabbix_server.conf**



Edit the WebServiceURL parameter to match your Zabbix web service IP and StartReportWriters to make sure we have a reporting subprocess:





**WebServiceURL=https://localhost:10053/report StartReportWriters=3**





**Important note**



For scheduled reporting to work you will need to set up SSL encryption for your Zabbix frontend; we recommend using Let's Encrypt. Alternatively, set the

IgnoreURLCertErrors=1 parameter in /etc/zabbix/zabbix_ web_service.conf.



That's it for the CLI part. Let's log in to our frontend and navigate to **Administration** | **General** | **Other**.



Make sure to fill out the **Frontend URL** parameter on this page with your own frontend URL like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_317ae43492824cde.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.62 – Administration | General | Others page with Frontend URL filled in

236	Visualizing Data, Inventory, and Reporting



​	
 	

​	Click the blue **Update** button at the bottom of the page and navigate to **Reports** | **Scheduled reports**.


 	

​	Now we are on the page where we set up and maintain our scheduled reports, so let's create a new one using the blue **Create report** button in the top-right corner.


 	

​	This will take us to a new page where we can set up a new report. Let's set up a weekly report using our existing dashboard **Global view**. First, we'll name this report Weekly overview of the Global view dashboard.


 	

​	Select the dashboard's **Global view** by clicking the **Select** button next to **Dashboard**.


 	

​	Put **Cycle** on **Weekly** with a start time of 9:00 and set **Repeat on** to **Monday**.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_dfd97a32d77031e6.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.63 – Reports | Scheduled reports, create new report page top part


 	

​	Also, make sure to fill in **Subject** and **Message** and set up the **Subscriptions** to match users that have media with the type of email set on their user profile.

Setting up scheduled PDF reports	237

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_2bf97a6b35d920b5.png) 























































Figure 6.64 – Reports | Scheduled reports, create new report page bottom part



You can now click the **Test** button to see if the report is working. Once it is, use the blue **Add** button to finish setting up this scheduled report.



**How it works…**



This feature is long awaited and finally here, but it's not finished and is simply still a building block for more advanced scheduled reports coming later. There are some key things to keep in mind with this new reporting functionality. I always state that Zabbix development tries to keep everything as customizable as possible by adding features and interconnecting them to make sure we can use existing functionality in new ways.



The Zabbix development team could have decided to create a fully-fledged PDF reporting engine for Zabbix. But by going the way of using Zabbix dashboards as building blocks for all your PDF reports, they have created versatility and customizability. Every single new dashboard widget that is added is now available for you to use in your PDF reports, and my guess is that more and more reporting-focused widgets will be added in the near future.

238	Visualizing Data, Inventory, and Reporting





Zabbix simply grabs the information from your dashboard and sends it to you in a PDF form using the new Zabbix web services module and the Google Chrome browser. Once we get these prerequisites out of the way, we are provided with a way to send PDF reports to any of our Zabbix users, provided they have an email media type set up.



**Setting up improved business service monitoring**



In Zabbix 6, business service monitoring has had an entire overhaul. If you've set it up in older versions, it might be wise to spend some time rediscovering the basics using this recipe. If you are entirely new to business service monitoring, do not worry as we will go through setting it up step by step in this recipe.



**Getting ready**



We will need our Zabbix server and access to its frontend. I'll be using my lar-book-centos host with the configuration we have done so far. We will also need a monitored host, for which I will use the Zabbix server itself.



**How to do it…**



I'll be using the Zabbix frontend as an example to set up business service monitoring, for which we will create a new host called lar-book-zabbix-frontend with some items and triggers.



**Setting up the items and triggers**



If you have followed the previous recipes, by now you should have a good understanding of setting up items and triggers. Let's go through it again and set up some for our business service monitoring example:



First, let's create a new template by logging in to our Zabbix frontend and navigating to **Configuration** | **Templates**.

Click on the blue **Create template** button in the top-right corner and fill in the page as follows:

Setting up improved business service monitoring	239

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e8ecf6c863d472e0.png) 









































Figure 6.65 – New Zabbix frontend template configuration page



Make sure to save this new template by clicking the blue **Add** button.



Then let's set up our new host navigating to **Configuration** | **Hosts**.



Click on the blue **Create host** button in the top-right corner and fill in the page as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_ac78d11eaee58cdd.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.66 – New Zabbix frontend host configuration page

Visualizing Data, Inventory, and Reporting



Then, add the following tag by navigating to the **Tags** page:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_6561a0d0ea069b40.jpg) 

























Figure 6.67 – New Zabbix frontend host configuration page, Tags tab



Click the blue **Add** button to save this new host configuration and navigate to **Configuration** | **Templates**.



Edit the **Zabbix frontend by Zabbix agent** template and go to **Value mapping**.



Click on the small **Add** button with the blue dotted line under it and add the following value mapping:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_1d7d396146302dbe.jpg) 































Figure 6.68 – Template Zabbix frontend by Zabbix agent, Service state value mapping



\10. Make sure to click the blue **Update** button. Then, back on the template, go to **Items**.

Setting up improved business service monitoring	241





\11. Click on the blue **Create item** button and add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_8baf56ec006e06a2.jpg) 



























Figure 6.69 – ICMP ping item



\12. Before adding the item, make sure to also add the **Value mapping** as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_8e8b86a2fd1a37d9.jpg) 









Figure 6.70 – ICMP ping item value mapping



\13. Then we will also go to the **Tags** page to add some tags to this item:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_33d5e58535e579eb.jpg) 



























Figure 6.71 – ICMP ping item Tags tab



\14. Now click the blue **Add** button at the bottom of the page.

Visualizing Data, Inventory, and Reporting



Back at **Items**, click on the blue **Create item** button to create another item. Fill in the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_69c8f112d5eee1dd.jpg) 





































Figure 6.72 – Agent ping item



\16. Then we will also go to the **Tags** page to add some tags to this item:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_11d057d3e3b282d2.jpg) 

























Figure 6.73 – HTTP service state item Tags tab



Now save the new item by clicking the blue **Add** button at the bottom of the page.



Now that we have two new items, let's navigate to the **Triggers** for this template.

Setting up improved business service monitoring	243





\19. Click the **Create trigger** button in the top-right corner and add the following trigger:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_84cacc31a0c5ecf3.jpg) 





































Figure 6.74 – ICMP down trigger configuration



On the **Tags** tab, let's also add a new tag, indicating that this trigger will be used in our SLA monitoring:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_25a87e8976d369d5.jpg) 



























Figure 6.75 – ICMP down trigger Tags tab



Now let's click the blue **Add** button to add this trigger. Then create another trigger using the **Create trigger** button in the top-right corner.

Visualizing Data, Inventory, and Reporting



Let's add the following trigger:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_ba86746b4e006982.jpg) 





































Figure 6.76 – Agent unreachable trigger configuration



\23. Make sure to add a tag for the SLA on this trigger as well on the **Tags** tab.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_cb4745082ec80b0b.jpg) 





























Figure 6.77 – Agent unreachable trigger Tags tab



\24. Click the blue **Add** button to finish setting up this trigger.

Setting up improved business service monitoring	245





**Adding the business service monitoring configuration**



That concludes our item and trigger configuration. We can now continue with actually setting up our business service monitoring:



First, let's define our SLA period by going to **Services** | **SLA** and clicking on the blue **Create SLA** button in the top-right corner. We'll define the following SLA:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a4277336618c8d37.jpg) 



































































Figure 6.78 – Services | SLA, Zabbix SLA setup



Click on **Add** at the bottom of the window to save this SLA.



Next, go to **Services** | **Services** and select **Edit** using the slider in the top-right corner.

246	Visualizing Data, Inventory, and Reporting



​	
 	

​	Now click **Create service** in the top-right corner to add a new service. We will add a new service for our Zabbix setup.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_6d31451fcbb9e197.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.79 – Services | Services, Zabbix setup service


 	

5.	On the **Tags** tab, also make sure to add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_a3a0f07692ee3ae5.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.80 – Services | Services, Zabbix setup service, SLA

Setting up improved business service monitoring	247





Click on the blue **Add** button at the bottom of the window to add this new service. Then click **Create service** in the top-right corner again to add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_1c2bb2311bdf44e2.jpg) 

































































Figure 6.81 – Services | Services, Zabbix server service



7.	At the **Tags** tab also make sure to add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_ce3027e0e83a085c.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 6.82 – Services | Services, Zabbix server service, SLA

248	Visualizing Data, Inventory, and Reporting



​	
 	

​	Click the blue **Add** button again and then **Create service** in the top-right corner again. We'll add another service on the same level as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_532de42ea643d130.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.83 – Services | Services, Zabbix database service


 	

9.	On the **Tags** tab, also make sure to add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_df3257fab03497fd.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.84 – Services | Services, Zabbix database service, SLA

​	
 	

\10. Let's click **Add** again to add this service.

Setting up improved business service monitoring	249





Then we'll add the last child of the Zabbix setup by clicking the **Create service** button again.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_977025cc582c4762.jpg) 





























































Figure 6.85 – Services | Services, Zabbix frontend service



Then select **Advanced configuration** and click on **Add** at **Additional rules**. We will add the following calculation here:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_bebd9ab1a5d7fd97.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.86 – Services | Services, Zabbix frontend service, additional rules

250	Visualizing Data, Inventory, and Reporting



​	
 	

\13. On the **Tags** tab, also make sure to add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_5978bd28af77dd7e.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.87 – Services | Services, Zabbix database service, SLA


 	

​	Finish setting up this service by clicking the **Add** button at the bottom of the window.


 	

​	Now, we'll have to add two more services, but this time under the Zabbix frontend. Click on **Zabbix frontend** and then **Create service** again and add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_dc1fc44f95f389fd.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.88 – Services | Services, Zabbix frontend, ICMP status child service

Setting up improved business service monitoring	251





Click the blue **Add** button and then **Create service** again to add the last service.



Let's also add the following service:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_4144e19fb8c68127.jpg) 













































































Figure 6.89 – Services | Services, Zabbix frontend, Zabbix agent status child service



\18. Click the blue **Add** button to add this service and let's see if it works as expected.

252	Visualizing Data, Inventory, and Reporting





**How it works…**



Let's take a look at what we have set up in our current configuration. We've used business service monitoring to monitor part of our Zabbix stack. Look at business service monitoring as a tree, where we just created two levels. Our initial level is the Zabbix setup, which consists of our Zabbix frontend, Zabbix server, and Zabbix database.



Beneath the Zabbix frontend level, we have one more level where we have defined two more services that represent the status of ICMP and the Zabbix agent. We only want to calculate the SLA if both ICMP and Zabbix agent are in a problem state.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e7faa4cc42d08ab7.jpg) 















































Figure 6.90 – Business service monitoring tree structure example



As you can see, we have a distinct tree structure forming once we start to visualize this in a drawing. The part where the magic happens in this case is the Zabbix frontend service, though, because this is where we defined what should happen to our SLA once something goes wrong with services.

Setting up improved business service monitoring	253





Let's take another look at that level:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_e0eae93b6ad88fa4.jpg) 











































































Figure 6.91 – Zabbix frontend service completed



Because we defined that the service should always **Set status to OK**, it will only use what we defined in our **Additional rules** section. This is where we specified that we only want to affect our SLA calculation: **If at least 2 child services have High status or above**.



Effectively, this means that our SLA is only going down if both the Zabbix agent is not reachable and ICMP is down.



We've built in a security measure for ourselves here, making sure that if someone stops the Zabbix agent but the server is still reachable by ICMP, the SLA won't be affected.

254	Visualizing Data, Inventory, and Reporting





Now let's take a look at the end result that we can use to monitor these SLAs. Over at **Services** | **SLA report**, we can find all we need to know about if our SLA is being met. We can set the filter to a time period we want to find the SLA for and it will result in something like the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_4b8fe6a6d36d690d.jpg) 





































Figure 6.92 – Services | SLA report, all services with our SLA:24x7 tag



We can see now see our monthly 24x7 SLA where an SLA of 99.9% is being expected. For our Zabbix set up back in October 2021 the SLA was 100, so we met our required SLA. Then in November 2021, we notice that the SLA dropped below 100, clearly indicating in red that our SLA was not met.



Drilling down even further and selecting our specific service Zabbix setup, we can create a more detailed overview:

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_9770f16ca38dd876.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 6.93 – Services | SLA report, Zabbix setup services with our SLA:24x7 tag

Setting up improved business service monitoring	255





Over here, we can see all the details regarding the up and downtime of our service and what our leftover error budget is like.



Using business service monitoring calculations like this, we can narrow down our services and how well they are calculated. In this case, we used a simple example with ICMP and the Zabbix agent trigger we know well by now, but the possibilities are endless using services in combination with tags.



**There's more...**



One of the main concerns with the old way of monitoring services through business service monitoring was the inability to automate and customize it. The automation has been mostly resolved through the use of tags, as we can now define tags on the host, template, or trigger level to define what's used in the business service monitoring configuration.



In terms of customization, Zabbix has given us a lot more options to do calculations.

![img](file:///tmp/lu106935qboif.tmp/lu106935qbzp5_tmp_dfc8231708bb88c0.jpg) 













































Figure 6.94 – Zabbix service, additional rule options



Looking at the numerous options here, we can see that we have a lot more to play around with. Not only can we specify the exact number of child services we'd like to use in our calculations, but we can also even work with weights and percentages, giving us the options we might need to build more complex setups.