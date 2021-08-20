# Walkthrough for SuiteCRM - ASTI Internship 

This walkthrough was made using the LAMP stack installed on an Ubuntu Virtual Machine hosted on Oracle VM Virtualbox, and assumes you already have SuiteCRM installed. 

Here are some helpful tutorials:\
https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04

https://upcloud.com/community/tutorials/installing-lamp-stack-ubuntu/

https://docs.suitecrm.com/admin/installation-guide/downloading-installing/

This walkthrough is made with the purpose of making things a little bit easier for the next intern taking on this project.

Version:\
L\
Apache\
M\
P\
Oracle VM Virtualbox\
SuiteCRM

## Making an Admin Account and Accessing the SuiteCRM Site

Following the official SuiteCRM installation guide, you can set up the admin account by accessing the Site Configuration page after going through the install. The SuiteCRM tutorial gives this link http://<yourServer>/<yourSuiteCRMDirectory>/install.php which isn't really all that helpful if you don't know what <yourServer> and <yourSuiteCRMDirectory> means.
  
Basically, the webpage is stored on a php file within the directory you chose to install SuiteCRM into. If you're lazy, like me, or you're just having a hard time using the address of the server folder you're using, you can just access the site on localhost. After installing Apache there should be some default sites which you can disable with `a2dissite`, and once you've done that the only remaining server should be the SuiteCRM server which you can then access.
  
Following that, you can set up the admin account on the Site Configuration page. This should be reasonably straightforward, and once you're at this point, things should get easier.

## Accessing the Virtual Machine Website from the Host Computer
  
Normally, you will want to mainly work on the SuiteCRM website from your host machine. This makes things more convenient since it will likely be easier to upload files, send emails, and so on. Most guides will describe some complicated port forwarding process that can be convoluted or hard to follow. Assuming you're using Virtual Box, there should be some menus to select from at the top of the screen. There, you can select Devices -> Network -> Network Settings. Our goal is to change the way that your virtual machine connects to the internet, since currently the default should be NAT, which means that it connects to the internet using a virtual router. There should be a dropdown menu for "Attached to:", and there select the option "Bridged Adapter". 
  
This tricks your host computer into thinking that servers hosted on the virtual machine are hosted locally. In the terminal, you can use `ifconfig` to find the new IP address of your virtual machine. It should then be clear that your virtual machine and host machine are within the same network range, and you can enter that IP address into your browser to visit your website.
  
## Changing the Site Name
  
I was tasked to make it so that I could access the site using the address suitecrm.asti.local. Since I was already able to access the site from my host computer, I opened up Notepad and edited the hosts file (with the type File, not iCalendar) in Windows/System32/drivers/etc (the location and steps may vary depending on your host computer). You can input the IP address in the format stipulated there, and use whatever alias you want.
  
## Extracting and Importing Data
  
For ASTI, we were given a masterlist of suppliers on Google Sheets with inconsistent data entries. It is likely that you will have to input at least some of them by hand, but the majority of companies and contacts can be inputted first by using native data cleaning options on Google Sheets. You can also write your own ETL scripts to normalize the data. What I did was separate email entries using all possible delimiters (as we only need one email for the company) and then inputting either the email or a blank entry if the remaining text was invalid.
  
You can directly import entries into prebuilt SuiteCRM modules. When on the module in question (Companies, Contacts, etc) there should be an option to click a button to import a CSV file from your computer. From there you can assign columns in the file to different data fields in the module itself. The process is reasonably quick, and omits rows where there are errors. Be careful to check that the amount of entries on the file you import is the same as the amount of entries in the table.
  
I have noticed that you cannot import data into custom modules for whatever reason. I have not found a fix since this problem came up in the last week of my internship, but this link may help: https://community.suitecrm.com/t/custom-module-and-bulk-importing/70094/2
  
Note: There is also likely an option to directly import data using SQL, but I have not tried it out myself.
  
## Changing Module Names and Variable Names
  
This is reasonably simple. You can change the names of modules by going into the Developer Tools section and clicking on Rename Modules. There you can easily rename the labels each module will have. Similarly, can change the names of variables by going into the Studio. From there, changing the name is as simple as scrolling to the module you want, opening it up, and changing the labels within.
  
## Creating Custom Modules
 
Custom modules can be made in the Module Builder section, which can be found in the Developer Tools section of the Administration Page. There are several templates to choose from. You can customize relationships, names, and variables at your leisure. 
  
For ASTI, we wanted a version of the Suppliers module without the multiple relationships cluttering up the page and distracting users. This became difficult, since some relationships were impossible to remove for one reason or another, so we instead decided to go forward with custom modules.

section about how to make custom module
## Removing or Adjusting Relationships

This can be done by going into the Studio, and moving to the module you want to adjust. There should be a Relationships option. All the relationships to and from the module will show up, and you can click on individual ones to adjust, remove, or add them.
  
## Filtering and Changing Default Views
  
This can be done by going into the ______. 
  
There is an inbuilt search function for tables that searches from the leftmost character to the right (which means that if you search for the subsection of a string in the middle of the bigger string, the entry will not show up in the search). You can also filter by industry and so on, and do bulk actions based on the remaining entries on sreen.
  
## Creating Custom Dropdown Menus
  
You can create dropdown menus by going into the ______.
  
For the internship, since we wanted to keep things simple, we removed all dropdown menus by doing ______.
  
## Making Tags
  
Checking briefly online it can be easily seen that there are paid options to add tags. We may want to implement some sort of tag system, since some supplier companies may fit into more than one, and so it may be useful to have more than one. Since we want to keep things cheap, what we can instead do is create a custom module with a many-to-many relationship with the module we want to tag. 

## Email Variables
  
Email variables can be done by selecting in the top right which module you want to pull data from. This lets you use it down in the body of the email to customize 
  
## Unfixed Problems

Analytics
Ratings
Import using SQL

