---
layout: post
title: "OSM Task Manager"
tagline: deploy OSMTM in da cloud
category: openstreetmap
tags:
  - openstreetmap
---

*I originally wrote this up for an intern who wanted to spin-up their own Task Manager...putting it here, in case others may benefit.* Gist here for comments, etc. [https://gist.github.com/oeon/a9dd7c4c73691030c135](https://gist.github.com/oeon/a9dd7c4c73691030c135).
<br><br> 

**Login** to [https://aws.amazon.com/](https://aws.amazon.com/) > Go to the **Console** > click **EC2**.  

**Launch Instance** > Select provided AMI in Quick Start section, for **Ubuntu** > **t2.micro/free tier eligible** will work fine...Click the **Review and Launch** button > accept defaults and click **Launch** button.  

Create a **new key pair**, give key pair a **name** value, **download** key pair > Click **Launch Instances** after key pair has downloaded.  

Click **View Instances** > In EC2 Dashboard, click **Instances** and first note your **Public IP** address (write it down), then scroll all the way to the right - click the *launch-wizard* link in the **Security Groups** section > In the **Inbound** tab > click **Edit** > **Add Rule** > *Type='All traffic', Source='Anywhere'*, **Save**.  

Connecting to your server, for Mac and Windows:

**(For Mac/Unix)** Open Terminal.app > alter the permissions of the key, something like `chmod 0600 yourkey.pem`.  
Run something like `ssh -i yourkey.pem ubuntu@12.345.67.89` in your Terminal.  

**(For Windows)** Download [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) if needed. Navigate to where PuTTY is installed in All Programs and open the PuTTYgen utility. Load the .pem file and save as .ppk private key.  

Putty > Connection > SSH > click on Auth section > load private key.  
Click back on Session section and enter your IP address.  
Save Session with a name for quick loading next time.  

The login in username is `ubuntu`.  

*(Resume instructions for all operating systems)* after sucessfully connecting to your server via SSH...  

Update/upgrade server with `sudo apt-get update`, then `sudo apt-get upgrade` (accept by pressing Y). If reaching purple screen, accept default choice and press enter.  

`sudo apt-get install python-setuptools python-dev libpq-dev apache2 libapache2-mod-wsgi git postgresql-9.3-postgis-2.1`  (accept by pressing Y).  

Now follow setup steps documented at [https://github.com/hotosm/osm-tasking-manager2](https://github.com/hotosm/osm-tasking-manager2).  

(hint: right click to paste in PuTTY) enter commands line by line.  

Create `local.ini` file with nano e.g. `nano local.ini`, change `PASSWORD` to your password.  

Stop following the steps before the *Launch the Application* section.  

Create an Apache config file. Copy the code from here: [https://github.com/hotosm/osm-tasking-manager2#installation-as-a-mod_wsgi-application](https://github.com/hotosm/osm-tasking-manager2#installation-as-a-mod_wsgi-application) > Paste into a new file e.g. `sudo nano /etc/apache2/sites-available/osmtm.conf` be sure to alter the Location (line 13) parameter if you're serving your app from a URL like [http://myip.com/osmtm](http://myip.com/osmtm).  

Create a file named `OSMTM.wsgi` in `~/osm-tasking-manager/env` > Copy its contents from [https://github.com/hotosm/osm-tasking-manager](https://github.com/hotosm/osm-tasking-manager) but change last line to match `osm-tasking-manager2` not `osm-tasking-manager`.  

Execute this series of commands:   
`sudo a2dissite 000-default`  
`sudo a2ensite osmtm`  
`sudo service apache2 reload`  

Open [http://your-public-ip/osmtm](http://your-public-ip/osmtm) e.g. [http://12.345.67.89/osmtm](http://12.345.67.89/osmtm) in a web browser.   
profit  
