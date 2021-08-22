# Walkthrough for SuiteCRM - ASTI Internship 

This walkthrough was made using the LAMP stack installed on an Ubuntu Virtual Machine hosted on Oracle VM Virtualbox, and assumes you already have SuiteCRM installed. 

Here are some helpful tutorials:\
https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04

https://upcloud.com/community/tutorials/installing-lamp-stack-ubuntu/

https://docs.suitecrm.com/admin/installation-guide/downloading-installing/

This walkthrough is made with the purpose of making things a little bit easier for the next intern taking on this project.

Version:\
Ubuntu - Version 18.04.02 LTS\
Apache2 - Version 2.4.46\
MySQL - Version 8.0\
PHP - Version 8.0.8\
Oracle VM Virtualbox - Version 6.1.25\
SuiteCRM - Version 7.11

## Making an Admin Account and Accessing the SuiteCRM Site

Following the official SuiteCRM installation guide, you can set up the admin account by accessing the Site Configuration page after going through the install. The SuiteCRM tutorial gives this link http://yourServer/yourSuiteCRMDirectory/install.php which isn't really all that helpful if you don't know what yourServer and yourSuiteCRMDirectory mean.
  
Basically, the webpage is stored on a php file within the directory you chose to install SuiteCRM into. If you're lazy, like me, or you're just having a hard time using the address of the server folder you're using, you can just access the site on localhost. After installing Apache there should be some default sites which you can disable with `a2dissite`, and once you've done that the only remaining server should be the SuiteCRM server which you can then access.
  
Following that, you can set up the admin account on the Site Configuration page. This can be done by filling in all the details asked for by the setup. After setting this up, you can use those details to log in as an admin and make changes to the site. Remember to set a strong password.

## Accessing the Virtual Machine Website from the Host Computer
  
Normally, you will want to mainly work on the SuiteCRM website from your host machine. This makes things more convenient since it will likely be easier to upload files, send emails, and so on. Most guides will describe some complicated port forwarding process that can be convoluted or hard to follow. Assuming you're using Virtual Box, there should be some menus to select from at the top of the screen. There, you can select Devices -> Network -> Network Settings. Our goal is to change the way that your virtual machine connects to the internet, since currently the default should be NAT, which means that it connects to the internet using a virtual router. There should be a dropdown menu for "Attached to:", and there select the option "Bridged Adapter". 
  
This tricks your host computer into thinking that servers hosted on the virtual machine are hosted locally. In the terminal, you can use `ifconfig` to find the new IP address of your virtual machine. It should then be clear that your virtual machine and host machine are within the same network range, and you can enter that IP address into your browser to visit your website.
 
## Changing the Site Name
  
I was tasked to make it so that I could access the site using the address suitecrm.asti.local. Since I was already able to access the site from my host computer, I opened up Notepad and edited the hosts file (with the type File, not iCalendar) in Windows/System32/drivers/etc (the location and steps may vary depending on your host computer). You can input the IP address in the format stipulated there, and use whatever alias you want. I inputted the address asked for as the alias, which lets me access it using that phrase.
  
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

Thie module building process lets you customize the name, the key, the relationships between modules, and the fields internal to the module itself.

## Removing or Adjusting Relationships

This can be done by going into the Studio, and moving to the module you want to adjust. There should be a Relationships option, which you can then click on. All the relationships to and from the module will show up, and you can click on individual ones to adjust, remove, or add them.

A relationship will have a name, a primary module, a type, and a related module. The purpose of the name is self-evident, and the primary module is the source of the relationship. There are three types of relationship: one to one, one to many, and many to many. In a one-to-one relationship, a unit in the primary module can only be related to one other unit in the related module, and vice-versa. In a one-to-many relationship, a unit in a primary module can be related to as many units in the related module as needed, but units in the related module can only be related to one unit in the primary module. In a many-to-many relationship, there are no restrictions on the amount of units a given unit can be related to. Modules can have several different relationships, even between the same modules.

These relationships are useful for creating things like tags, associated contact details, contracts, and so on.

## Filtering and Changing Default Views
  
Units in modules can be filtered through a button in the View Module tab (it's the one using the icon of a coffee filter). There criteria can be used to filter out entries with certain names, filter by type, and so on. Columns can also be ommitted from the view using the column chooser button to the right of the filter button.
  
The way searches and filters work is a bit weird, since while there is an inbuilt search function for tables, it searches from the leftmost character to the right, which means that if you search for the subsection of a string in the middle of the bigger string, the entry will not show up in the search. One way of getting around this is by tagging entries through a string that one can then search through using various mutually exclusive tags, albeit this is a bit convoluted.
  
## Creating Custom Dropdown Menus
  
There are two kinds of dropdowns, one external to modules which organizes them in different groups, and one internal to modules which sets discrete options for entries in a given field. You can adjust the first rather easily by moving to Configure Main Menu Filters in the Developer Tools section of Admin. From there you can drag and drop modules in and out of groups in order to organize things. For the internship, since we wanted to keep things simple, we removed all dropdown menus by emptying them out.

For the second type, you can create dropdown menus by going into the Dropdown editor. Then you can change the labels and order of different items in the list, as well as add and remove items.
  
## Making Tags
  
Checking briefly online it can be easily seen that there are paid options to add tags. We may want to implement some sort of tag system, since some supplier companies may fit into more than one, and so it may be useful to have more than one. Since we want to keep things cheap, what we can instead do is create a custom module with a many-to-many relationship with the module we want to tag. 

## Unfixed Problems

Analytics -- We were asked to download an Analytics module for SuiteCRM, but either the option was paid or not update safe. We had trouble both with the install instructions provided by the guide as well as the manual install which changed nothing in the cloned instance of SuiteCRM we attempted an install for. 

Ratings -- We were asked to create a variable that displays the rating of a supplier dynamically updating each time a rating was given, taking the average of all ratings. I had no idea how to start going about this, and the add-ons that automatically did it for you were paid. I ended up shelving this to prioritize other things.

Email Variables -- We wanted to be able to send emails in waves customized to different suppliers (to alert them to an opened bid, or update them on progress on their contract, and so on). It was easy enough to do this using variables from the module you are using plus one additional related module, but I was totally stumped on how to add more modules you might want to pull information from (for example, if you were already pulling from Suppliers and Contacts, and you needed details from Contracts, you would not be able to.) 

