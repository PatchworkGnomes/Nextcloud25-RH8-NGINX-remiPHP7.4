# Nextcloud25-RH8-NGINX-remiPHP7.4

This was a small project for a friend a while ago, they suggested I post it on GitHub...
<br />
<br />
I had built something similar for Apache first, but was curious about NGINX so I made this.  I have built a reverse proxy with NGINX, but still don’t have a good understanding of all the nuances the NGINX requires, especially around the host configuration
<br />
<br />
So, please…without hesitation, give me feedback.  More important, give me the best practice. I have no qualms with people showing and telling me how to better my limited knowledge.
<br />
<br />
Quote by Ram Dass: (or << Backtrack)<br /> 
“The quieter you become, the more you can hear.”

## Dependencies

Keep in mind of Internal and External DNS and rules on your routers\firewalls, assuming you want this to be public.
Also note that the versions may be different, based on when you install
* NextCloud Hub 3 (25.0.1)
* Red Hat Enterprise Linux 8.7 (Ootpa)
* Zend Engine v3.4.0, 
* Zend OPcache v7.4.30
* nginx version: nginx/1.14.1
* certbot 1.22.0
* Ver 15.1 Distrib 10.3.35-MariaDB
* ImageMagick 6.9.12-64 Q16 x86_64 17467

## Installing\Executing 
What I like to do for this is log in as root or sudo -i.  then copy the script into the /tmp/ and then kick it off.
<br />
The first part of the script is for variables. 
<br />
A few things to consider first:

### SERVER STUFF
This is basically going to be your external DNS name for your website.  Its what you will enter into the URL
<br />
<b>SERVERNAME=ncloud.Your.Domain</b>

### CERT STUFF
The script creates a demo self-signed certificate and places it in<br />
<b>/etc/nginx/ssl/nextcloud.key</b><br />
<b>/etc/nginx/ssl/nextcloud.crt</b><br />
<br />
If you do use the certbot, you may have to go back into:<br />
<b>/etc/nginx/conf.d/nextcloud.conf</b><br />
and change the Cert location
<br />
<br />
The fields used to create the cert are:
<br />
<b>COUNTRY=US
<br />
STATE=California
<br />
CITY=Malibu
<br />
ORG=FunInTheSun
<br />
ORG_UNIT=Nextcloud
<br />
CERTMAIL=your@email.com</b>
<br />
<br />
The last line on the Script enables CertBot to create the SSL cert for the site. It is commented out, so make sure you have the External DNS setup properly and your firewall rules setup.  CertBot also needs port 80 and 443.  Please be careful how you setup your firewalls\NAT\Port forwarding
<br />

### DB STUFF
This is the name of the database that will create for nextcloud<br />
<b>DBNAME=nextcloud</b>

This is the username that is needed for the nextcloud database. <br />
NOTE: This is not a nextcloud user account<br />
<b>USER=nextclouduser</b>
<br />
<br />
This is the name of the nextcloud Admin user
<br />
<b>ADMIN=admin</b>

### PASSWORD STUFF
Generates a random password for the root account of mariadb. You can change it if you wish
</br>
<b>date +%s | sha256sum | base64 | head -c 16 > /tmp/.MARIADBPASSWORD</b>

Generates a random password for the nextcloud user account in mariadb. You can change it if you wish
</br>
<b>date +%s | sha256sum | base64 | head -c 16 >> /tmp/.APPPASSWORD</b>

Generates a random password for the Nextcolud admin user account password. You can change it if you wish
</br>
<b>date +%s | sha256sum | base64 | head -c 16 >> /tmp/.ADMINPASS</b>

### TRUSTED DOMAIN STUFF
If you try to log in and you get a "Trusted Domain error" you will have to add it.  You can do that by editing</br>
<b>/var/www/html/nextcloud/config/config.php</b>
<br />
you may have to add the domain name or IPs.  

### LANGUAGE STUFF
In this script the default language is EN for all the settings, so you may want to change that if you want.  You can find that here:
</br>
<b>/usr/share/nginx/nextcloud/config/config.php</b>

### CLEANING UP
When the script is done running it will remind you that there are passwords and scripts in the /tmp. Keep them there if you need them but do remember to clean house.  If a bad actor popped into your server, they will have the key to your kingdom.
