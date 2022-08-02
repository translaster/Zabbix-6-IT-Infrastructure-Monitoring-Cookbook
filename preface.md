# Preface

Welcome to *Zabbix 6 IT Infrastructure Monitoring Cookbook*. IT infrastructure ranges from Windows and Linux to networking and development, and basically anything that runs on computer hardware. In this book, we will go over various topics useful to anyone in IT that wants to use Zabbix to monitor their IT infrastructure.

**Who this book is for**

Monitoring systems are often overlooked within IT organizations, but they can provide an overview that will save you time, money, and headaches. This book is for IT engineers that want to learn about Zabbix 6 and how to use it to bring their IT environments to the next level.

**What this book covers**

*Chapter 1*, *Installing Zabbix and Getting Started Using the Frontend*, covers how to set up Zabbix, optionally with HA. We will also work our way through the Zabbix frontend.

*Chapter 2*, *Getting Things Ready with Zabbix User Management*, covers how to set up your first users, user groups, and user roles.

*Chapter 3*, *Setting Up Zabbix Monitoring*, covers how to set up almost any type of monitoring within Zabbix.

*Chapter 4*, *Working with Triggers and Alerts*, covers how to set up triggers and get alerts from them.

*Chapter 5*, *Building Your Own Structured Templates*, covers how to build templates that are structured, which will work wonders for keeping your Zabbix setup organized.

*Chapter 6*, *Visualizing Data, Inventory, and Reporting*, covers how to visualize data in graphs, maps, and dashboards. It also covers how to use the Zabbix inventory, reporting, and business service monitoring functionality.

*Chapter 7*, *Using Discovery for Automatic Creation*, covers how to use Zabbix discovery for automatic host creation as well as items, triggers, and more with agents, SNMP, WMI, and JMX.

*Chapter 8*, *Setting Up Zabbix Proxies*, teaches you how to set up Zabbix proxies correctly for use in a production environment.

*Chapter 9*, *Integrating Zabbix with External Services*, teaches you how to integrate Zabbix with external services for alerting.

*Chapter 10*, *Extending Zabbix Functionality with Custom Scripts and the Zabbix API*, covers how to extend Zabbix functionality by using custom scripts and the Zabbix API.

*Chapter 11*, *Maintaining Your Zabbix Setup*, covers how to maintain a Zabbix setup and keep its performance up over time.

*Chapter 12*, *Advanced Zabbix Database Management*, teaches you how to manage Zabbix databases for an advanced setup.

*Chapter 13*, *Bringing Zabbix to the Cloud with Zabbix Cloud Integration*, covers how to use Zabbix in the cloud with services such as AWS, Azure, and Docker.

**To get the most out of this book**

You should have a good basis in IT to understand the terminology used in this book. This book is best for people with at least a starting knowledge of monitoring systems, Linux, and network engineering.

![img](file:///tmp/lu258424lw1ev.tmp/lu258424lw1f0_tmp_d707e2ccf7107b93.jpg) 

Make sure you have a virtualization environment ready to create virtual machines for use with the recipes. VirtualBox, VMware, or any type of client/hypervisor will do.

Throughout the book, we will make use of VIM to edit files, so make sure to install it. If you do not feel comfortable using VIM, you can substitute the command for NANO or anything else you prefer.

**If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book's GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.**

**Download the example code files**

You can download the example code files for this book from GitHub at:

[https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook)[Monitoring-Cookbook](https://github.com/PacktPublishing/Zabbix-6-IT-Infrastructure-Monitoring-Cookbook)

If there's an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at:

https://github.com/PacktPublishing/

Check them out!

**Download the color images**

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here:

[https://static.packt-cdn.com/downloads/9781803246918_](https://static.packt-cdn.com/downloads/9781803246918_ColorImages.pdf) [ColorImages.pdf](https://static.packt-cdn.com/downloads/9781803246918_ColorImages.pdf)

**Conventions used**

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "It's important to back up all of our Zabbix configuration data, which is located in the /etc/zabbix/ folder."

A block of code is set as follows:

1. MariaDB Server

1. ​	To use a different major version of the server, or to pin to a specific minor version, change URI below.

deb [arch=amd64] http://downloads.mariadb.com/MariaDB/ mariadb-10.5/repo/ubuntu xenial main

Any command-line input or output is written as follows:

**systemctl start mariadb**

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For instance, words in menus or dialog boxes appear in **bold**. Here is an example: "Then we navigate to **Monitoring** | **Hosts** and click on **Latest data** for the Zabbix server host."

**Tips or Important Notes**

Appear like this.

**Get in touch**

Feedback from our readers is always welcome.

**General feedback**: If you have questions about any aspect of this book, email us at [customercare@packtpub.com](mailto:customercare@packtpub.com)[ ](mailto:customercare@packtpub.com)and mention the book title in the subject of your message.

**Errata**: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit [www.packtpub.com/support/errata](http://www.packtpub.com/support/errata)[ ](http://www.packtpub.com/support/errata)and fill in the form.

**Piracy**: If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or website name. Please contact us at [copyright@packt.com](mailto:copyright@packt.com)[ ](mailto:copyright@packt.com)with a link to the material.

**If you are interested in becoming an author**: If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit [authors.](http://authors.packtpub.com/) [packtpub.com](http://authors.packtpub.com/).

**Share Your Thoughts**

Once you've read *Zabbix 6 IT Infrastructure Monitoring Cookbook - Second Edition*, we'd love to hear your thoughts! Please [click here to go straight to the](https://packt.link/r/180324691X) [Amazon review page](https://packt.link/r/180324691X)[ ](https://packt.link/r/180324691X)for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.
