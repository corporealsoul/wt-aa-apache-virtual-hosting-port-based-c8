vHosts , https://httpd.apache.org/docs/2.4/vhosts/examples.html 

Configuration Sections , http://httpd.apache.org/docs/2.4/sections.html


### Create the Directory Structure,

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.name`

<br>

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/public_html`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log/`

<br>

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/public_html`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log/`


### Grant Permissions,

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.name/`

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/public_html`

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/public_html`


### Create Demo Pages for Each Virtual Host,

`[anup@c7-100-apache-server ~]$ nano /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/public_html/index.html`

    <html>
        <head>
            <title>anuniqsTV</title>
        </head>
        <body>
            <h1>Hey ! from <em>anuniqstv.apache.name.ek</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>

<br>

[anup@c7-100-apache-server ~]$ nano /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/public_html/index.html

    <html>
        <head>
            <title>anuniqsTV</title>
        </head>
        <body>
            <h1>Hey ! from <em>anuniqstv.apache.name.dui</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>

### Create New Virtual Host Files,

`[anup@c7-100-apache-server ~]$ sudo lsof -i -P -n | grep LISTEN`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:80`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:81`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:82`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:83`

<br>

[anup@c7-100-apache-server ~]$ sudo nano /etc/httpd/sites-available/anuniqstv.apache.name.conf

    Listen 83
    
    <VirtualHost *:83>
        ServerName 192.168.56.100
        ServerAlias 192.168.56.100
        DocumentRoot /var/www/anuniqstv.apache.name/
        ErrorLog /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log/error.log
        CustomLog /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log/requests.log combined
    </VirtualHost>
    
    <VirtualHost *:83>
        ServerName 192.168.56.100
        ServerAlias 192.168.56.100
        DocumentRoot /var/www/anuniqstv.apache.name/
        ErrorLog /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log/error.log
        CustomLog /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log/requests.log combined
    </VirtualHost>

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-available/`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled`


### Enable the New Virtual Host Files,

`[anup@c7-100-apache-server ~]$ sudo ln -s /etc/httpd/sites-available/anuniqstv.apache.name.conf /etc/httpd/sites-enabled/anuniqstv.apache.name.conf`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled`


### Firewall,

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --zone=public --add-port=83/tcp --permanent`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --reload`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --list-all`


### SELinux,

`[anup@c7-100-apache-server ~]$ sudo semanage port -l | grep http_port_t`

`[anup@c7-100-apache-server ~]$ sudo semanage port -a -t http_port_t -p tcp 83`

<br>

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log/`

`[anup@c7-100-apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log(/.*)?"`

`[anup@c7-100-apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log/`

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.name/anuniqstv.apache.name.ek/log/`

<br>

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log/`

`[anup@c7-100-apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log(/.*)?"`

`[anup@c7-100-apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log/`

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.name/anuniqstv.apache.name.dui/log/`


### Check all the available Virtual Host configurations,

`[anup@c7-100-apache-server ~]$ httpd -S`

`[anup@c7-100-apache-server ~]$ apachectl configtest`

`[anup@c7-100-apache-server ~]$ sudo systemctl restart httpd.service`

`[anup@c7-100-apache-server ~]$ sudo systemctl status httpd.service`


### Test Your Results, Open your favorite browser :

http://192.168.56.100/

[http://192.168.56.100:80/](http://192.168.56.150/)

http://192.168.56.100:81/

http://192.168.56.100:82/

http://192.168.56.100:83/

http://192.168.56.100:83/anuniqstv.apache.name.ek/public_html/

http://192.168.56.100:83/anuniqstv.apache.name.dui/public_html/


### List all Virtual Hosts available,

`[anup@c7-100-apache-server ~]$ httpd -S`

<br>
