**vHosts ,** https://httpd.apache.org/docs/2.4/vhosts/examples.html

**Configuration Sections ,** http://httpd.apache.org/docs/2.4/sections.html


### Create the Directory Structure,

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.port.ek/public_html`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.port.ek/log/`

`[anup@c7-100-apache-server ~]$ ls -ltr /var/www/anuniqstv.apache.port.ek`

<br>

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.port.dui/public_html`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.port.dui/log/`

`[anup@c7-100-apache-server ~]$ ls -ltr /var/www/anuniqstv.apache.port.dui`


### Grant Permissions,

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.port.ek/public_html`

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.port.dui/public_html`

`[anup@c7-100-apache-server ~]$ sudo chmod -R 755 /var/www`


### Create Demo Pages for Each Virtual Host,

`[anup@c7-100-apache-server ~]$ nano /var/www/anuniqstv.apache.port.ek/public_html/index.html`

    <html>
        <head>
            <title>anuniqsTV</title>
        </head>
        <body>
            <h1>Hey ! from <em>anuniqstv.apache.port.ek</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>

[anup@c7-100-apache-server ~]$ nano /var/www/anuniqstv.apache.port.dui/public_html/index.html

    <html>
        <head>
            <title>anuniqsTV</title>
        </head>
        <body>
            <h1>Hey ! from <em>anuniqstv.apache.port.dui</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>


### Create New Virtual Host Files,

`[anup@c7-100-apache-server ~]$ sudo lsof -i -P -n | grep LISTEN`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:81`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:82`

<br>

`[anup@c7-100-apache-server ~]$ sudo nano /etc/httpd/sites-available/anuniqstv.apache.port.ek.conf`

    Listen 81  
    
    <VirtualHost *:81>
        ServerName 192.168.56.100 
        ServerAlias 192.168.56.100
        DocumentRoot /var/www/anuniqstv.apache.port.ek/public_html
        ErrorLog /var/www/anuniqstv.apache.port.ek/log/error.log
        CustomLog /var/www/anuniqstv.apache.port.ek/log/requests.log combined
    </VirtualHost>

<br>

`[anup@c7-100-apache-server ~]$ sudo nano /etc/httpd/sites-available/anuniqstv.apache.port.dui.conf`

    Listen 82
    
    <VirtualHost *:82>
        ServerName 192.168.56.100
        ServerAlias 192.168.56.100
        DocumentRoot /var/www/anuniqstv.apache.port.dui/public_html
        ErrorLog /var/www/anuniqstv.apache.port.ek/log/error.log
        CustomLog /var/www/anuniqstv.apache.port.ek/log/requests.log combined
    </VirtualHost>


`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-available/`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled`


### Enable the New Virtual Host Files,

`[anup@c7-100-apache-server ~]$ sudo ln -s /etc/httpd/sites-available/anuniqstv.apache.port.ek.conf /etc/httpd/sites-enabled/anuniqstv.apache.port.ek.conf`

`[anup@c7-100-apache-server ~]$ sudo ln -s /etc/httpd/sites-available/anuniqstv.apache.port.dui.conf /etc/httpd/sites-enabled/anuniqstv.apache.port.dui.conf`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled`


### Firewall,

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --zone=public --add-port=81/tcp --permanent`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --zone=public --add-port=82/tcp --permanent`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --reload`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --list-all`


### SELinux,

`[anup@c7-100-apache-server ~]$ sudo semanage port -l | grep http_port_t`

`[anup@c7-100-apache-server ~]$ sudo semanage port -a -t http_port_t -p tcp 81`

`[anup@c7-100-apache-server ~]$ sudo semanage port -a -t http_port_t -p tcp 82`

`[anup@c7-100-apache-server ~]$ sudo semanage port -l | grep http_port_t`

<br>

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.port.ek/log/`

`[anup@c7-100-apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstv.apache.port.ek/log(/.*)?"`

`[anup@c7-100-apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstv.apache.port.ek/log`

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.port.ek/log/`

<br>

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.port.dui/log/`

`[anup@c7-100-apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstv.apache.port.dui/log(/.*)?"`

`[anup@c7-100-apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstv.apache.port.dui/log`

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.port.dui/log/`


### Check all the available Virtual Host configurations,

`[anup@c7-100-apache-server ~]$ httpd -S`


### Check apache configurations,

`[anup@c7-100-apache-server ~]$ apachectl configtest`


### Restart httpd service,

`[anup@c7-100-apache-server ~]$ sudo systemctl restart httpd.service`

`[anup@c7-100-apache-server ~]$ sudo systemctl status httpd.service`


### Test Your Results, Open your favorite browser : 

http://192.168.56.100/

http://192.168.56.100:81/

http://192.168.56.100:82/


### List all Virtual Hosts available,

`[anup@c7-100-apache-server ~]$ httpd -S`


### How do I enable/disable a website hosted with Apache,

### Disable,

`[anup@c7-100-apache-server ~]$ sudo unlink /etc/httpd/sites-enabled/anuniqstvsite.com.conf`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled/`

`[anup@c7-100-apache-server ~]$ httpd -S`

`[anup@c7-100-apache-server ~]$ sudo systemctl restart httpd.service`


**Open your favorite browser :** http://192.168.56.100/


### Enable,

`[anup@c7-100-apache-server ~]$ sudo ln -s /etc/httpd/sites-available/anuniqstvsite.com.conf /etc/httpd/sites-enabled/anuniqstvsite.com.conf`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled/`

`[anup@c7-100-apache-server ~]$ httpd -S`

`[anup@c7-100-apache-server ~]$ sudo systemctl restart httpd.service`


**Open your favorite browser :** http://192.168.56.100/


### Apache processes,

`[anup@c7-100-apache-server ~]$ ps -ef | grep -i "httpd"`

`[anup@c7-100-apache-server ~]$ htop`

<br>
