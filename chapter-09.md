# 9 Integrating Zabbix with External Services

In this chapter, we are going to set up some of the useful external service integrations that Zabbix has to offer. We can use these external services to notify our Zabbix users of ongoing problems.



We will start by learning how to set up in-company chat applications such as Slack and Microsoft Teams. Then, we will learn how to use a personal chat application such as Telegram before learning how to integrate Atlassian Opsgenie for even more extensive alerting.



Once you've completed these recipes, you will be able to effectively integrate certain services with Zabbix. This is a good starting point for working with external services in general and the easiest way to set up Slack, Teams, Opsgenie, and Telegram.

In this chapter, we will cover the following recipes:



Setting up Slack alerting with Zabbix



Setting up Microsoft Teams alerting with Zabbix



Using Telegram bots with Zabbix



Integrating Atlassian Opsgenie with Zabbix



Let's get started!



**Technical requirements**



For this chapter, we are going to need our Zabbix server, preferably how we set it up throughout this book, though any Zabbix 6 server with some alerts on it will do.



We will also need access to a few external services, as follows:



Slack (free, to an extent) https://slack.com/



Microsoft Teams (free, to an extent) https://www.microsoft.com/microsoft-teams



Opsgenie (free, to an extent) https://www.atlassian.com/software/opsgenie/



Telegram (free)



https://telegram.org/



We will not cover how to set up the services themselves, but how to integrate them with Zabbix. Make sure you have set up the required service by following a guide and that you have some knowledge of the services in general.



**Setting up Slack alerting with Zabbix**

​	
 	

​	Slack is a widely used tool for easy text messaging, voice/video chat, and collaboration. In this recipe, we will learn how to use Zabbix Slack integration to send our Zabbix problem information to Slack so that we can gain a good overview of issues.

Setting up Slack alerting with Zabbix	319





**Getting ready**



Make sure you have Slack set up. You can go to https://slack.com/intl/en-in/ and set it up for free there. We will also need a Zabbix server with some active problems.



**How to do it…**



Follow these steps to complete this recipe:



1.	Once you have set up and opened Slack, you should see the following page:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_845ea638e638d60f.jpg) 





















































Figure 9.1 – Slack's default page

320	Integrating Zabbix with External Services



​	
 	

​	Let's create a new channel for our Zabbix notifications by clicking the **+Add channels** button. Then, from the dropdown that appears, click **Create a new channel**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_c0bbd277483a479a.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.2 – Slack – Create a channel window


 	

​	Click the green **Create** button. Then, on the next window, click the green **Done** button to add all members:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_dd6b35524681eeff.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.3 – Slack – Add people window

Setting up Slack alerting with Zabbix	321





Now, navigate to the following link to create a Slack bot for working with Zabbix: https://api.slack.com/apps.



You will see the option **Create New App** on this page. Click the **Create New App** button, which brings us here:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_42789d2b90aa5201.jpg) 





































Figure 9.4 – Slack API – Create an app page



After clicking the **Create New App** button, we have to click on **From scratch**.



Then we'll see a pop-up window where we can set up our new Slack bot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_47050b953d23c4eb.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.5 – Slack API – Name app & choose workspace window

322	Integrating Zabbix with External Services



​	
 	

​	Click on **Create App**. This will take you to the **Add features and functionality** page. On this page, click on **Bots**, as highlighted in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_855ff135153dfbf2.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.6 – Slack API – Add features and functionality page


 	

​	This will take you to the new app's **Home** page. On the left-hand side of the page, click the **OAuth & Permissions** option.


 	

​	Scroll down to **Scopes** and click on **Add an OAuth Scope**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_db0f4bade2d58ed7.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.7 – Slack API – Scopes

Setting up Slack alerting with Zabbix	323





From the drop-down menu, click on chat:write to allow our bot to write to a channel:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_e98149ee6175063e.jpg) 





































Figure 9.8 – Slack API – Add an OAuth Scope dropdown



Do the same for im:write and groups:write.



Scroll back up and click on the **Install to Workspace** button to finish setting up this app:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_6feb93c0988a600d.jpg) 

















Figure 9.9 – Slack API – Install to Workspace button



\14. Next, you will see a pop-up message. Click the green **Allow** button that appears.

324	Integrating Zabbix with External Services



​	
 	

​	After clicking **Allow**, you will see your new token. Copy the token by clicking the **Copy** button:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_e6eb9f8d072a4d7c.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.10 – Slack API – Our new Bot User OAuth Token


 	

​	Lastly, add your bot to the **#zabbix-notifications** channel by going back to your Slack channel, clicking on the members in the top-right corner, and selecting **Integrations**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_5374be09be959f16.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.11 – Slack – Connect an app to a channel

Setting up Slack alerting with Zabbix	325





Click on **Add an App**.



Simply add **Zabbix-Alert-Bot** by clicking the white **Add** button:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_13da85e12911ea1a.jpg) 























Figure 9.12 – Slack – Connect an app with a bot



Now, navigate to your Zabbix frontend and go to **Administration** | **Media types**.



Click on **Slack** to edit the Slack media type. You will see a whole list of preconfigured parameters. We need to paste our OAuth token into the bot_token parameter, like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_2351756efda9b90c.jpg) 































Figure 9.13 – Zabbix Slack media type – Edit page

326	Integrating Zabbix with External Services



​	
 	

​	At the **Message templates** tab, we already have five message types configured. We can edit these to our liking if we would like to do so.

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_1bf9276b2f7ccec8.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.14 – Zabbix media type Slack – Edit page for message types


 	

​	Now, let's create a new user group for our media types by navigating to **Administration** | **User groups** and clicking the **Create user group** button in the top-right corner. Add the following user group:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_c353d2c8421be9e.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.15 – Zabbix – Create user group page for the External Services group


 	

​	Click on the **Permissions** tab and then click on **Select**. Make sure that you select all the groups and subgroups with at least **Read** permissions, like so:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_e80194b09aca03b4.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.16 – Zabbix – Create user group permissions page for the External Services group

Setting up Slack alerting with Zabbix	327





**Important Note**



When applying permissions to the user group, make sure that you only add the host groups you want to receive notifications from. In my lab and even some production environments, I add all groups, but sometimes, we want to filter the notifications down. A great way to do this is to use different user groups and users so that you only receive notifications from host groups in certain channels. For a more in-depth look into Zabbix user permissions and triggers, check out *Chapter 2*, *Getting Things Ready with Zabbix User Management,* and *Chapter 4*, *Working with Triggers and Alerts, r*espectively.



Now, click on the blue **Add** button and finish creating this user group.



Next, navigate to **Administration** | **Users** to create a Slack user. Click on the blue **Create user** button in the top-right corner.



Add the following user to your Zabbix server:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_9f71215d69ffbf9f.jpg) 



































Figure 9.17 – Zabbix – Create user page for user Slack

328	Integrating Zabbix with External Services



​	
 	

​	Now, click on the **Media** tab and click on the underlined **Add** text. We will add the following media to this user:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_95f204808dffe995.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.18 – Zabbix – Create user media page for user Slack


 	

​	All users will also need a **User** role. To add it, go to **Permissions** and add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_a7552525a0ab8883.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.19 – Zabbix, Create user media page for user Slack, Permissions


 	


 	

**Important Note**


 	

​	For convenience, we are adding the user in the **Super admin** user role in the example. This overrides the read-only permissions we assigned on the **External Services** user group. To limit the permissions, choose a **User** or **Admin** role for your user, which will adhere to the host group permissions we assigned earlier.

​	
 	

​	Click on the blue **Add** button at the bottom of the window and then on the blue **Add** button at the bottom of the page.

Setting up Slack alerting with Zabbix	329





We will also need to add a macro to **Administration** | **General**. Use the drop-down menu to select **Macros** and add the following macro, which contains your Zabbix URL:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_1173f80f8a31d12d.jpg) 















Figure 9.20 – Zabbix – Administration | General macros page with Zabbix URL



Click on the blue **Update** button.



Last but not least, go to **Administration** | **Actions** and on the **Trigger Actions** page, click on the blue **Create action** button.

Use Notify external services for the name of the action. We won't set up any conditions for this example, but it's recommended to do so in most cases.

Go to **Operations** and add the following operations:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_7bf822323307f24b.jpg) 













































Figure 9.21 – Zabbix – Create action operations page for Notify external services





**Tip**



We could also use **Notify all involved** here to send a message to all the users involved in the **Operations** steps.

Integrating Zabbix with External Services



Now, click on the blue **Add** button. With that, you're done. You can now view any new problems (once they are generated by Zabbix) in your Slack channel:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_48e7c4ff6acbc00f.jpg) 



























Figure 9.22 – Slack – Zabbix notification sent from our Zabbix server



**How it works…**



Working with media types might be something completely new to you, or you might have done it in the past. Regardless, starting from Zabbix 5, the process has changed a bit. In the days of Zabbix before Zabbix 5, we had to find the right media types online or make them ourselves.



Now, with Zabbix 6, we get a lot of preconfigured media types that are ready to be used. We just need to do the necessary setup and fill in the right information, just like we just did for Slack. We are then ready to send our problem-related information from Zabbix 6 to Slack every time a media type is created.



In this recipe, we told our Zabbix server to only send problem-related information with a severity of warning or higher to Slack, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_32c925cdc0bb46b6.jpg) 











Figure 9.23 – Zabbix Media page for the Slack user



We can fully customize these severities, but we can also fully customize what severities we send to our Slack setup.



What we configured in this recipe is called a **Zabbix webhook**. A problem gets created in Zabbix and this problem matches our configured criteria, such as its severity. Our media type gets triggered and sends it to the configured links:

Setting up Microsoft Teams alerting with Zabbix	331

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_841670c0b2b82e99.png) 



































Figure 9.24 – Zabbix Slack integration diagram



Zabbix sends the problem to the Slack API, and then the API processes it has configured there. Then, the app we configured in Slack posts the problem to our channel.



Now that we've completed this recipe, we can view problems in Slack and keep an eye on our Zabbix alerts from there.



**See also**



If you want to do more with this integration, check out the Slack API documentation. There's a lot we can do with this API, and we can build a number of awesome apps/bots for our channels: https://api.slack.com/.



**Setting up Microsoft Teams alerting with Zabbix**



At the time of writing this book, I'm living in the Netherlands during the COVID-19 crisis. A lot of IT companies have been requested to make their employees work from home. Due to this, there's been a rise in the use of Microsoft Teams and applications like it. Suddenly, a lot of companies have started using Microsoft Teams and others to make working from home and collaboration easier.



Let's learn how we can make working with Microsoft Teams even better by integrating our Zabbix alerting into it.



**Getting ready**

​	
 	

​	We will need our Zabbix server to be able to create some problems for us. For this, you can use lar-book-centos from our previous chapters or any Zabbix server that you prefer.

332	Integrating Zabbix with External Services





We will also need general Microsoft Teams knowledge and, of course, Microsoft Teams itself set up and ready to go.



**How to do it…**



Follow these steps to complete this recipe:



Let's start by opening our Microsoft Teams application on either Windows, macOS, or Linux and creating a new channel. Go to **Teams** and click on the three dots (**…**) next to your team name, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_70f5eaa969f27628.jpg) 



































Figure 9.25 – MS Teams, Add channel option



In the **Add channel** window, fill in the following information to create our new channel:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_cb5c12d2d7bb2dd.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.26 – MS Teams, Add channel window

Setting up Microsoft Teams alerting with Zabbix	333





Now, click the purple **Add** button to add the channel. Upon doing this, we will be able to see our new channel in the list.



Click on the three dots (**…**) next to your channel, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_ced470b232d1189a.jpg) 













































Figure 9.27 – MS Teams, Connectors option



We want to select the **Connectors** option from this drop-down menu. This allows us to add our Microsoft Teams connector to this channel.



We are using the search field here to find the official **Zabbix Webhook**connector:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_fb1e0918e102c99a.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.28 – MS Teams, Add connectors window

334	Integrating Zabbix with External Services



​	
 	

​	Click on the purple **Add** button next to the **Zabbix Webhook** connector to add this connector to our channel. This will open another pop-up window, where we must click the purple **Add** button again.


 	

​	You will get another pop-up window, where you need to copy the webhook URL. Do this by clicking the white **Copy** button:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_399d81b0de3fcb2.jpg) 
 	


 	


 	


 	


 	


 	


 	

​	Figure 9.29 – MS Teams – Webhook URL


 	

​	Now, click the purple **Save** button. Upon doing this, you can close the pop-up window.


 	

​	Go to the Zabbix frontend and navigate to **Administration** | **Media types**. Click on the **MS Teams** media type here.


 	

​	Scroll down until you see **teams_endpoint**. Paste the URL you copied previously here, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_93d0d539ea03fc87.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.30 – Zabbix Administration | Media types, edit MS Teams page


 	

​	Now, click the blue **Update** button at the bottom of the page.


 	

​	If you didn't follow the previous recipe, then create a new user group for our media types by navigating to **Administration** | **User groups** and clicking the **Create user group** button in the top-right corner. Add the following user group:

Setting up Microsoft Teams alerting with Zabbix	335

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_844aa25f4ac90f0c.png) 































Figure 9.31 – Zabbix, Create user group page, the External Services group



Click on the **Permissions** tab and click on **Select**. Make sure that you select all the groups and subgroups with **Read** permissions. The **Permissions** tab will look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_8ccb659ad94790de.jpg) 





















Figure 9.32 – Zabbix – Create user group permissions page, the External Services group



Now, click on the blue **Add** button and finish creating this user group.



Navigate to **Administration** | **Users** and click on the blue **Create user** button in the top-right corner. Add the following user:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_76c3c7e9d21fc644.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.33 – Zabbix Administration | Media types – Create new user page, MS Teams

336	Integrating Zabbix with External Services



​	
 	

​	Next, go to the **Media** tab of the **Create user** page. Click the underlined **Add** text here to create the following media:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_5b2c147ee371d0e1.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.34 – Zabbix Administration | Media types – Create new user page, MS Teams


 	

​	All users will also need a **User role**. To add it, go to **Permissions** and add the following:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_6e0dcfb91ad38125.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.35 – Zabbix – Create user media page, user Slack, Permissions


 	


 	

**Important Note**


 	

​	For convenience, we are adding the user in the **Super admin** user role in the example. This overrides the read-only permissions we assigned on the **External Services** user group. To limit the permissions, choose a **User** or **Admin** role for your user, which will adhere to the host group permissions we assigned earlier.


 	

​	Once you've filled in this information, click the blue **Add** button at the bottom of the window and then the blue **Add** button at the bottom of the page.


 	

​	If you didn't follow the previous recipe, then you will also need to add a macro to **Administration** | **General**. Use the drop-down menu on this page to select **Macros** and add the following macro, which contains your Zabbix URL:

Setting up Microsoft Teams alerting with Zabbix	337

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_7f255a2085c48def.jpg) 

















Figure 9.36 – Zabbix Administration | General macros page, Zabbix URL for use with MS Teams



Click on the blue **Update** button. You will also need to go to **Administration** | **Actions** if you didn't follow the previous recipe and, on the **Trigger Actions** page, click on the blue **Create action** button.



Use Notify external services for the name of the action. We won't set up any conditions for this example, but it's recommended to do so in most cases.

Go to **Operations** and add the following operations:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_dc8d433ec056306e.jpg) 













































Figure 9.37 – Zabbix – Create action Operations page, Notify external services for use with MS Teams





**Tip**



We could also use **Notify all involved** here to send a message to all the users involved in the **Operations** steps.

Integrating Zabbix with External Services



Now, click on the blue **Add** button and you'll be done. You can now view new problems as they occur in your MS Teams channel:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_b92f8a5047208d1c.jpg) 

































Figure 9.38 – A Zabbix problem in an MS Teams channel



**How it works…**



Microsoft Teams works in about the same way as our Slack setup. A problem is created in the Zabbix server and if that problem matches our configured conditions in Zabbix, we send that problem to our Microsoft Teams connector.



For instance, we configured Zabbix so that it only sends problems with a severity of warning or higher to Microsoft Teams, as shown here:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_a83eee4f038981a9.jpg) 











Figure 9.39 – Zabbix Media page for MS Teams users



Our Microsoft Teams connector catches our problem and since this connector is configured directly on our channel, it posts a notification to the channel:

Using Telegram bots with Zabbix	339

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_e02d92f6d3bb8e92.png) 



































Figure 9.40 – Zabbix Microsoft Teams integration diagram



Now, we can see our Teams notifications in our channel and keep up to date with all our Zabbix issues directly via Microsoft Teams.



**See also**



For more information about the Zabbix webhook connector, check out this page:



[https://appsource.microsoft.com/en-us/product/office/](https://appsource.microsoft.com/en-us/product/office/WA200001837?tab=Overview) [WA200001837?tab=Overview](https://appsource.microsoft.com/en-us/product/office/WA200001837?tab=Overview).



**Using Telegram bots with Zabbix**



If you love automation in chat applications, you might have heard of or used Telegram messenger. Telegram has an extensive API and amazing bot features.



In this recipe, we are going to use a Telegram bot to create a Telegram group for Zabbix alerts. Let's get started.



**Getting ready**



Make sure you have your Zabbix server ready. You can use lar-book-centos or any Zabbix server capable of sending some alerts.



It would be useful if you have some knowledge of Telegram, but I'll be describing how to set it up step by step, even for those of you who have never used Telegram bots. Just make sure that you have the Telegram app on your computer and that your account is set up.

340	Integrating Zabbix with External Services





**How to do it…**



Follow these steps to complete this recipe:



First, let's create a new channel in Telegram. Click on the create icon next to the search box and select **New Group**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_f13c75bc671847fd.jpg) 



















Figure 9.41 – Telegram – New Group button



Add another user – someone in your team that needs to get notifications as well. Fill in a group name and add a picture if you want:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_879fb08b238f6243.jpg) 

























Figure 9.42 – Telegram – New Group page



Now, click on the **Create** button in the top-right corner. This will take you to the **New Group** page.

Using Telegram bots with Zabbix	341





Working with Telegram bots is made easy with the @BotFather user on Telegram. We can start creating our bot by searching for botfather and clicking the specified contact:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_c142e78a7499d78f.jpg) 

















Figure 9.43 – Telegram – BotFather user



Let's start by issuing the /start command in the chat. This will provide you with a list of commands you can use:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_609b532fc7a61f82.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.44 – Telegram – BotFather user help list

342	Integrating Zabbix with External Services



​	
 	

​	Now, let's immediately create a new bot by typing /newbot into the **BotFather** chat. Press *Enter* to send your message. This will give us the following result:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_240d9d9cb6731c58.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.45 – Telegram – BotFather /newbot command


 	

​	Type in the new name of the bot; we will call it zabbix-notfication-bot. Press *Enter* to send your name:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_37f2b3908f4fd8d2.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.46 – Telegram – BotFather bot username


 	

​	You will then be asked what username you want to give the bot. I will use lar_ zbx_notfication_bot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_66fa34228e0133ad.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.47 – Telegram – BotFather bot token

Using Telegram bots with Zabbix	343





**Important Note**



Your bot username must be unique, so you can't use lar_zbx_ notification_bot here. Pick a unique bot name that suits you and use that name throughout this recipe.



Make sure that you save the **HTTP API** key somewhere safe.



Let's go back to our **Zabbix Notifications** group and add our bot. Click on the group in your list of chats and click on the group's name.



Now, click on **Add** to add the bot, as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_9d58771c137bf7d4.jpg) 





























Figure 9.48 – Telegram – Add user to group button



Next, you will need to search for your bot using its username, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_10446299398a0ccd.jpg) 























Figure 9.49 – Telegram – Add user to group page

344	Integrating Zabbix with External Services



​	
 	

​	Click on the bot and click on the **Add** button. With that, your bot has been added to the channel.


 	

​	Let's navigate to the Zabbix frontend and go to **Administration** | **Media types**. Click on the media type titled **Telegram**.


 	

​	Here, you must add the **HTTP API** key you generated earlier to the **Token** field of our media type:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_5b59b9204c4b6b9e.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.50 – Zabbix Administration | Media types – Edit Telegram Media type page


 	

​	Click on the blue **Update** button at the bottom of the page to finish editing the **Telegram** media type.


 	

​	Now, let's go back to Telegram and add another bot to our group. Go to our new group and click on the group's name. Click on **Add** to add the following user:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_6aecee7e0e06013.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.51 – Add user page for a Telegram group


 	

​	Click on the user and click on **Add**. Then, navigate back to the Zabbix frontend.


 	

​	If you haven't followed any of the preceding recipes, create a new user group for our media types by navigating to **Administration** | **User groups** and clicking the **Create user group** button in the top-right corner. Add the following user group:

Using Telegram bots with Zabbix	345

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_f8797786224d10a0.png) 





























Figure 9.52 – Zabbix – Create user group page, External Services for use with Telegram



Click on the **Permissions** tab and click on **Select**. Make sure that you select all the groups and subgroups with **Read** permissions, as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_c4b95f7a918d499e.jpg) 

























Figure 9.53 – Zabbix – Create user group permissions page, External Services for use with Telegram



Now, click on the blue **Add** button and finish creating this user group.



At this point, you must create a new user in Zabbix. However, to create this user, you are going to need our new group ID. Go back to Telegram and issue /getgroupid@myidbot in the group chat. You will receive a value that you will need to copy:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_28def202e17941c4.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.54 – Telegram user group ID

346	Integrating Zabbix with External Services



​	
 	

​	Let's navigate to **Administration** | **Users** and click the blue **Create user** button. Add the following user:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_17564cccc44dd650.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.55 – Zabbix – Create user page, user Telegram


 	

​	Now, select the **Media** tab and click on the underlined **Add** text. Add the following media:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_60bdc9c3b6494641.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.56 – Zabbix – Create user media page, user Telegram


 	

​	All users will also need a **User role**. To add it, go to **Permissions** and add the following:

Using Telegram bots with Zabbix	347

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_2100dbd7587e677e.jpg) 

















Figure 9.57 – Zabbix – Create user media page, user Slack, Permissions





**Important Note**



For convenience, we are adding the user in the **Super admin** user role in the example. This overrides the read-only permissions we assigned on the **External Services** user group. To limit the permissions, choose a **User** or **Admin** role for your user, which will adhere to the host group permissions we assigned earlier.



Make sure that you add the group ID to the **Send to** field without the **–** text and click on the blue **Add** button.



If you haven't followed the previous recipes, you will also need to go to **Administration** | **Actions**. Then, on the **Trigger Actions** page, click on the blue **Create action** button.



Use Notify external services for the name of the action. We won't set up any conditions for this example, but it's recommended to do so in most cases.

Go to **Operations** and add the following operations:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_6ec253281ade02e.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.58 – Zabbix – Create action operations page, Notify external services for use with Telegram

348	Integrating Zabbix with External Services





**Tip**



We could also use **Notify all involved** here to send a message to all the users involved in the **Operations** steps.



Now, click on the blue **Add** button. With that, you're done. You can now view new problems in your Telegram group:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_9e0ab007f0ad6896.jpg) 



























Figure 9.59 – Zabbix notifications in Telegram chat



**How it works…**



Slack apps, Microsoft connectors, and Telegram bots – they all work the same in the end. There's just another backend provided by the respective companies, but the Zabbix webhook remains.



Now that we've added our Zabbix Telegram integration, we are receiving notifications in our Telegram group via the Zabbix webhook:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_cef981e0008bf584.jpg) 











Figure 9.60 – Zabbix Administration | Users – Edit user media page



However, we will only receive these notifications if they match our configured settings. For instance, we've added our media type so that it only sends problems with a severity of warning or higher to our Telegram bot:

Integrating Atlassian Opsgenie with Zabbix	349

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_361caada7485d275.png) 



































Figure 9.61 – Zabbix Telegram integration diagram



Our Zabbix server is now sending our problems that match the **Action** conditions to our Telegram bot. The bot catches these problems successfully. Because our bot is in our Telegram group, the problems are posted in our Telegram group.



**There's more…**



There's a very cool Zabbix community group on Telegram. Now that you have Telegram, do not forget to join using the following invite link: https://t.me/ZabbixTech.



**See also**



Make sure that you check out all the awesome features Telegram bots have to offer. There's a lot of information available directly from Telegram, and you can build amazing integrations by using them: https://core.telegram.org/bots.



**Integrating Atlassian Opsgenie with Zabbix**



Atlassian Opsgenie is so much more than just another integration service for receiving notifications. Opsgenie offers us a call system, an SMS system, iOS and Android apps, two-way acknowledgments, and even an on-call schedule.



I think Opsgenie is the best tool for replacing old-school call and SMS systems and fully integrating them with Zabbix. So, let's get started with Opsgenie and see how we can get this amazing tool set up.

350	Integrating Zabbix with External Services





**Getting ready**



Ensure that your Zabbix server is ready. I'll be using the lar-book-centos server, but any Zabbix server ready to send problems should work.



You are also going to need an Atlassian Opsgenie account with Opsgenie ready to go.



I won't show you how to create accounts, but we'll start this recipe with Opsgenie ready to go.



**How to do it…**



Follow these steps to complete this recipe:



Let's start by logging into our Atlassian Opsgenie setup and going to **Settings** on the home page. From the left sidebar, click on **Notifications**.



Make sure that you add your email and phone number here by using the **+ Add email** and **+ Add phone number** buttons. We need these in order to receive notifications:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_51259651cc413546.jpg) 













































Figure 9.62 – Opsgenie profile – Contact methods



Your settings will be automatically saved once you've added them, which means we can navigate away from **Settings** to **Teams** using the top bar.

From the **Teams** tab, click on the blue **Add team** button in the top-right corner. Then, add the following information:

Integrating Atlassian Opsgenie with Zabbix	351

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_233d8d09784c126f.png) 























































Figure 9.63 – Opsgenie – Add team window



I've set up our **Consultancy** team with two users that are part of this team. Click on the blue **Add** button at the bottom of the window to add the new team.



This will take you to the new **Consultancy team** page. Click on **Integrations**and click on the blue **Add integration** button in the top-right corner.



When we use the search field to search for the Zabbix integration, we can see it immediately, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_fc78f069b0bf6a10.jpg) 

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

​	Figure 9.64 – Opsgenie – Add integration page

Integrating Zabbix with External Services



Hover over the Zabbix integration and click the blue **Add** button. This will take you to the next page, where a **Name** and **API Key** will be generated:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_f1130f06e3ff696e.jpg) 





























Figure 9.65 – Opsgenie – Add Zabbix integration page



Copy the **API Key** information and scroll to the bottom of the page. Here, click on the blue **Save integration** button.



Now, navigate to your Zabbix server CLI and execute the following code:



For RHEL-based systems:





**wget https://github.com/opsgenie/oec-scripts/releases/ download/Zabbix-1.1.6_oec-1.1.3/opsgenie-zabbix-1.1.6.x86_64.rpm**



For Ubuntu systems:





**wget https://github.com/opsgenie/oec-scripts/releases/ download/Zabbix-1.1.6_oec-1.1.3/opsgenie-zabbix_1.1.6_ amd64.deb**



We can now install the downloaded Zabbix Opsgenie plugin by issuing the following command(s):



For RHEL-based systems:



**rpm -i opsgenie-zabbix-1.1.6.x86_64.rpm**



For Ubuntu systems:



**dpkg -i opsgenie-zabbix_1.1.6_amd64.deb**



Integrating Atlassian Opsgenie with Zabbix	353





Once you've installed the plugin, go to the Zabbix frontend. If you haven't followed any of the previous recipes, create a new user group for our media types by navigating to **Administration** | **User groups** and clicking the **Create user group** button in the top-right corner. Add the following user group:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_a70704ba76a0ec32.jpg) 



























Figure 9.66 – Zabbix – Create user group page, the External Services group for use with Opsgenie



Click on the **Permissions** tab and click on **Select**. Make sure that you select all the groups and subgroups with **Read** permissions, as follows:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_ef9112550e91797a.jpg) 





















Figure 9.67 – Zabbix – Create user group permissions page, the External Services group for use with Opsgenie



Now, click on the blue **Add** button and finish creating this user group.



Next, navigate to **Administration** | **Actions**. On the **Trigger actions** page, click on the blue **Create** action button in the top-right corner.

354	Integrating Zabbix with External Services



​	
 	

\16. In the **Name** field, type Opsgenie action and add the following items to

**Conditions**:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_a76816f4b28c284.jpg) 
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.68 – Zabbix – Create new action page, Opsgenie action


 	

​	Now, click on the **Operations** tab.


 	

​	Click the underlined **Add** text option next to **Operations** to add your first operation. Set **Operation type** to **Remote command** and paste in the contents of the /home/opsgenie/oec/opsgenie-zabbix/actionCommand.txt file, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_62cc64ae103e1c3f.jpg) 
 	

​	
 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	


 	

Figure 9.69 – Zabbix – Create action operations window, Opsgenie action

Integrating Atlassian Opsgenie with Zabbix	355





Repeat *step 17* for **Recovery operations** and **Update operations**. It should look like this:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_cd51b00797184f93.jpg) 



































Figure 9.70 – Zabbix – Create action operations page, Opsgenie action



Click on the blue **Add** button at the bottom of the page to finish setting up the action.



Now, you must configure the Opsgenie-to-Zabbix integration. Edit the config file with the following command:





**vim /home/opsgenie/oec/conf/config.json**



Make sure that you edit the apiKey, command_url, user, and password lines, as shown in the following screenshot:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_7044306b9e05e136.jpg) 

























Figure 9.71 – Opsgenie config.json file

356	Integrating Zabbix with External Services





**Important Note**



You will need to edit baseUrl if you are not located in the United States. I am in Europe, so I changed it to https://api.eu.opsgenie.com.



That's it! You can now see your alerts coming in and acknowledge them from Opsgenie:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_d7341d74ce90dcdf.jpg) 









Figure 9.72 – Opsgenie alert from Zabbix



**How it works…**



When an alert is created in Zabbix, it is sent to Opsgenie via the Zabbix integration. This integration utilizes the Opsgenie API to catch our Zabbix information and send back a reply if required. This way, we have two-way communication between the two applications:

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_af9581e078442819.jpg) 







































Figure 9.73 – Opsgenie setup diagram

​	
 	

​	Opsgenie is an amazing tool that can take several tasks away from your Zabbix server. I've used it in the past to migrate away from another monitoring tool to Zabbix. Opsgenie makes it easy to receive alerts from our products and centralize notifications:

Integrating Atlassian Opsgenie with Zabbix	357

![img](file:///tmp/lu106935qboif.tmp/lu106935qc164_tmp_850437c3f0787f79.png) 









































Figure 9.74 – Opsgenie setup example, inspired by Wadie



Another great feature of Atlassian Opsgenie is the integration it offers with other Atlassian products. We can build a setup like the one shown in the preceding diagram to integrate all the products used in our company.



**There's more…**



If you are interested in more information about Opsgenie, check out the following blog post:



[https://blog.zabbix.com/scheduling-and-escalation-made-easy-](https://blog.zabbix.com/scheduling-and-escalation-made-easy-zabbix-and-opsgenie-integration/10005/)[zabbix-and-opsgenie-integration/10005/](https://blog.zabbix.com/scheduling-and-escalation-made-easy-zabbix-and-opsgenie-integration/10005/)