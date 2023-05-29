# Linux Webhosting server 

### Zadání 

 - Cílem projektu je vytvořit __Linux Webhosting Server__, který bude poskytovat zákazníkům služby webového hostingu. 

### Server musí splňovat následující požadavky: 

 - Bezpečné připojení k serveru pomocí certifikátů. Připojení pomocí hesla bude zakázáno. 
 - Server bude bezpečný a odolný proti chybám. 
 - Server bude jednoduše škálovatelný o další subdomény zákazníků. 
 - Každá subdoména bude mít vlastní TSL certifikát. 
     - Certifikáty budou pro každého zákazníka (neboli subdoménu) zdarma. 
 - Varianta 1. Vlastní webserver pro každého zákazníka. 
     - Zákazníci budou mít přístup ke svému webu přes SFTP. 
 - Varianta 2. Redakční systém pro každého zákazníka. 
     - Zákazníci budou mít přístup ke svému webu pomocí webového rozhraní. 

### Předpoklady: 

 - Server s veřejnou IP adresou s nainstalovaným operačním systémem (doporučuji Ubuntu Server 22.04). 
     - Registraci a správu si zajistí žáci sami ([Cloud Infrastructure | Oracle](https://www.oracle.com/cloud/)). 
 - Doména pro realizaci projektu: __ssibrno.cz__ 
     - Registraci a správu zajistí vyučující. 
     - Žáci mají povoleno registrovat si pro účely tohoto projektu vlastní doménu na vlastní náklady. 
 - ~~Znalost základů práce s Linuxem a znalost základních příkazů pro správu systému.~~ 

### Cíle projektu: 

 - Funkční Webhostingový Server. 
 - Funkční webový projekt jednoho zákazníka. 
 - Podrobná dokumentace realizace projektu (např. na GitHub). 
 - Profesionální prezentace projektu. 

### Termín: 

 - Zadavatel projektu stanovil termín dokončení na __konec května__. Konkrétní termín bude zadavatelem upřesněn. 

### Prezentace projektu a hodnocení: 

 - Prezentace funkčního Webhostingového Serveru 
     - Přístup k serveru, princip řešení. 
 - Prezentace funkčního webového projektu jednoho zákazníka 
     - Funkční URL adresa s TLS certifikátem. 
 - Aktivita a přístup k realizaci projektu 
     - Průběžně hodnoceno vyučujícím. 
 - Podrobná dokumentace realizace projektu 
     - Bude odevzdáno v termínu stanoveném zadavatelem. 
     - Bezpodmínečně musí obsahovat: 
         - Popis realizace projektu. 
         - Použité technologie a jejich stručný popis. 
         - Schéma řešení. 
         - Pracovní postup krok za krokem. 
 - Bonus: Automatizační skript pro založení nového zákazníka. 

### Klíčová slova: 

 - Ubuntu Server 22.04 
 - Docker (Portainer, Watchtower, ...) 
 - Reverse Proxy (Caddy, Nginx, ...) 
 - Webový server (Caddy, Nginx, Apache, PHP, SQL, ...) 
 - CMS systém (Wordpress, Drupal, ...) 

### Základní informace webhosting o serveru: 

__Ip:__ 141.144.228.182 

__Doména:__ https://penize-zdarma.ssibrno.cz/ 

__Subdoména:__ https://penize-zdarma2.ssibrno.cz/ 

__OS:__ Ubuntu Server 22.04 

__Zakladatel:__ ~~Jára Cimrman~~

__Kontributoři:__  

 - ~~Jára Cimrman~~
 - Černý Vít
 - Žalud Adam 
 - ChatGPT 
 - Boháček Ondřej

### Struktura serveru a jeho funkčnosti: 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek1.png)

## Postup práce na serveru: 

1. Použití ChatGPT 
2. Nainstalování Apache2 
3. Vložení souborů ve formátu html a PHP na webovou stránku 
4. Využití “Certbot” k získání HTTPS certifikátu 
5. Konfigurace DNS a vytváření subdomén 
6. Vytvoření přístupu pro zákazníka 
7. ROZBITÍ SERVERU

### 1. Použití ChatGPT 

__Proč:__ 

 - ANO! 
 - Ušetří myšlení – CO JE TO WEBSERVER? 
 - Napíše všechny kódy za vás 
 - Konfigurace – nemusíte vůbec pracovat 

======> [ZKUSTE NYNÍ!!!](https://chat.openai.com/) <====== 

 
 

 

### 2. Nainstalování Apache2 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek2.png)

__Proč:__ 

 - Nainstalovali jsme Apache2 z důvodu správné funkčnosti webové stránky, jeho vysoké kompatibility a rozšířenosti napříč světem (=bylo jednoduší najít návody pro práci s prostředím) 
 - Na rozdíl od ostatních takových softwarů nám přišel také jednoduší na pochopení 
 - Jedná se o web server software sdílený jako open-source program  

__Postup instalace a základního použití služby podle stránky:__

[_Zdrojová Stránka_](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-22-04) 

#### Installing Apache2

```
sudo apt update 

sudo apt install apache2 

sudo ufw app list 
```

Your output will be a list of the application profiles: 

```
Available applications: 
  Apache 
  Apache Full 
  Apache Secure 
  OpenSSH 
```

As indicated by the output, there are three profiles available for Apache: 

 - Apache: This profile opens only port 80 (normal, unencrypted web traffic) 
 - Apache Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic) 
 - Apache Secure: This profile opens only port 443 (TLS/SSL encrypted traffic) 

It is recommended that you enable the most restrictive profile that will still allow the traffic you’ve configured. Since you haven’t configured SSL for your server yet in this guide, you’ll only need to allow traffic on port 80: 

```
sudo ufw allow 'Apache' 
```

You can verify the change by checking the status: 

```
sudo ufw status 
```

The output will provide a list of allowed HTTP traffic: 

```
Status: active 
 
To                         Action      From 
--                         ------      ---- 
OpenSSH                    ALLOW       Anywhere                   
Apache                     ALLOW       Anywhere                 
OpenSSH (v6)               ALLOW       Anywhere (v6)              
Apache (v6)                ALLOW       Anywhere (v6) 
```

__Checking your Web Server__

At the end of the installation process, Ubuntu 22.04 starts Apache. The web server will already be up and running. 

Make sure the service is active by running the command for the systemd init system: 

```
sudo systemctl status apache2 
```

Output: 

```
● apache2.service - The Apache HTTP Server 
    	Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor prese> 
   	Active: active (running) since Tue 2022-04-26 15:33:21 UTC; 43s ago 
      	Docs: https://httpd.apache.org/docs/2.4/ 
 	Main PID: 5089 (apache2) 
   	Tasks: 55 (limit: 1119) 
   	Memory: 4.8M 
        CPU: 33ms 
   	CGroup: /system.slice/apache2.service 
             ├─5089 /usr/sbin/apache2 -k start 
             ├─5091 /usr/sbin/apache2 -k start 
             └─5092 /usr/sbin/apache2 -k start 
```

As confirmed by this output, the service has started successfully. However, the best way to test this is to request a page from Apache. 

You can access the default Apache landing page to confirm that the software is running properly through your IP address, enter it into your browser’s address bar: 

`http://your_server_ip`

You will see the default Ubuntu 22.04 Apache web page as in the following: 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek3.png)

This page indicates that Apache is working correctly. It also includes some basic information about important Apache files and directory locations. 

__Managing the Apache Process__

Now that you have your web server up and running, let’s review some basic management commands using systemctl. 

To stop your web server, run: 

```
sudo systemctl stop apache2 
```

To start the web server when it is stopped, run: 

```
sudo systemctl start apache2 
```

To stop and then start the service again, run: 

```
sudo systemctl restart apache2 
```

If you are simply making configuration changes, Apache can often reload without dropping connections. To do this, use the following command: 

```
sudo systemctl reload apache2 
```

By default, Apache is configured to start automatically when the server boots. If this is not what you want, disable this behavior by running: 

```
sudo systemctl disable apache2 
```

To re-enable the service to start up at boot, run: 

```
sudo systemctl enable apache2 
```

Apache will now start automatically when the server boots again. 

__Setting Up Virtual Hosts (Recommended)__

When using the Apache web server, you can use virtual hosts (similar to server blocks in Nginx) to encapsulate configuration details and host more than one domain from a single server. We will set up a domain called your_domain, but you should replace this with your own domain name. 

Apache on Ubuntu 22.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. While this works well for a single site, it can become unwieldy if you are hosting multiple sites. Instead of modifying /var/www/html, create a directory structure within /var/www for a your_domain site, leaving /var/www/html in place as the default directory to be served if a client request doesn’t match any other sites. 

Create the directory for your_domain as follows: 

```
sudo mkdir /var/www/your_domain 
```

Next, assign ownership of the directory to the user you’re currently signed in as with the $USER environment variable: 

```
sudo chown -R $USER:$USER /var/www/your_domain 
```

The permissions of your web root should be correct if you haven’t modified your umask value, which sets default file permissions. To ensure that your permissions are correct and allow the owner to read, write, and execute the files while granting only read and execute permissions to groups and others, you can input the following command: 

```
sudo chmod -R 755 /var/www/your_domain 
```

Next, create a sample index.html page using vim or your favorite editor: 

```
sudo vim /var/www/your_domain/index.html 
```

Inside, add the following sample HTML: 

```
<html> 
    <head> 
        <title>Welcome to Your_domain!</title> 
    </head> 
    <body> 
        <h1>Success!  The your_domain virtual host is working!</h1> 
    </body> 
</html> 
```

Save and close the file when you are finished. If you’re using vim, you can do this by pressing ESC, then typing :wq and pressing ENTER. 

In order for Apache to serve this content, it’s necessary to create a virtual host file with the correct directives. Instead of modifying the default configuration file located at `/etc/apache2/sites-available/000-default.conf` directly, make a new one at `/etc/apache2/sites-available/your_domain.conf`: 

```
sudo vim /etc/apache2/sites-available/your_domain.conf 
```

Add in the following configuration block, which is similar to the default, but updated for your new directory and domain name: 

```
<VirtualHost *:80> 
    ServerAdmin webmaster@localhost 
    ServerName your_domain 
    ServerAlias www.your_domain 
    DocumentRoot /var/www/your_domain 
    ErrorLog ${APACHE_LOG_DIR}/error.log 
    CustomLog ${APACHE_LOG_DIR}/access.log combined 
</VirtualHost> 
```

Notice that we’ve updated the DocumentRoot to our new directory and ServerAdmin to an email that the your_domain site administrator can access. We’ve also added two directives: ServerName, which establishes the base domain that will match this virtual host definition, and ServerAlias, which defines further names that will match as if they were the base name. 

Save and close the file when you are finished. 

Now enable the file with the a2ensite tool: 

```
sudo a2ensite your_domain.conf 
```

Disable the default site defined in 000-default.conf: 

```
sudo a2dissite 000-default.conf 
```

Next, test for configuration errors: 

```
sudo apache2ctl configtest 
```

You should receive the following output: 

```
. . . 
Syntax OK 
```

Restart Apache to implement your changes: 

```
sudo systemctl restart apache2 
```

Apache will now be serving your domain name. You can test this by navigating to `http://your_domain`, where you will see something like the following: 

> <html> 
    <head> 
        <title>Welcome to Your_domain!</title> 
    </head> 
    <body> 
        <h1>Success!  The your_domain virtual host is working!</h1> 
    </body> 
</html> 

#### Konfigurace Serveru

__Co vše je zapotřebí nakonfigurovat?__
 - Obecná konfigurace serveru – již podrobně popsáno v předchozím kroku 
 - Správa souborů – je nutné určit adresář souborů, který bude sloužit jako kořenový adresář pro webové stránky a měl tedy obsahovat všechen webový obsah (např.: HTML soubory, obrázky atd.) pro jeho vzhled a účel – popsáno v dalším kroku 
 - Nastavení SSL/TLS – těmito certifikáty lze zajistit šifrovanou komunikaci mezi serverem a klientem pro zvýšení bezpečnosti – postup získání certifikátu v kroku 4 
 - Nachystat přidání dalších subdomén pro zákazníky 

### 3. Vložení souborů ve formátu html a PHP na webovou stránku 

__Proč:__ Aby na stránce bylo něco, co se točí (=odkaz na točící se dolar na naší stránce) a co je zapotřebí k dobrému vzhledu stránky je zapotřebí vložit do adresáře soubory, které budou použity pro finální vzhled webu. 

__Jak přidat soubory na server:__ 

Soubory zkopírujeme do serverového adresáře buďto pomocí nástrojů jako je SCP( secure copy) nebo FTP), nebo můžeme soubory přesunout ze stávajícího uložiště na server pomocí příkazů „cp“ anebo „mv“. 

Pokud by některý z příkazů neměl správná přístupová práva, lze je změnit pomocí příkazu chmod.  

Nakonec je důležité, aby obsah měl správnou struktury a byl co (popřípadě) nejlépe organizován v adresářích 

__Výsledek:__

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek5.png)

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek6.png)

### 4. Využití “Certbot” k získání HTTPS certifikátu 

__Co je to Certbot:__ je automatizovaný systém pro vystavení certifikátů, které se používají k šifrování komunikace na internetu prostřednictvím HTTPS.  

Jak nainstalovat a použít Certbot podle postupu z:https://certbot.eff.org/instructions?ws=apache&os=ubuntufocal 

__SSH into the server__

SSH into the server running your HTTP website as a user with sudo privileges. 

__Install snapd__

You'll need to install snapd and make sure you follow any instructions to enable classic snap support. 

Follow these instructions on [snapcraft's site to install snapd](https://snapcraft.io/docs/installing-snapd). 

__Ensure that your version of snapd is up to date__

Execute the following instructions on the command line on the machine to ensure that you have the latest version of snapd. 

```
sudo snap install core; sudo snap refresh core 
```

__Remove certbot-auto and any Certbot OS packages__

If you have any Certbot packages installed using an OS package manager like apt, dnf, or yum, you should remove them before installing the Certbot snap to ensure that when you run the command certbot the snap is used rather than the installation from your OS package manager. The exact command to do this depends on your OS, but common examples are 	`sudo apt-get remove certbot`, `sudo dnf remove certbot`, or `sudo yum remove certbot`

__Install Certbot__

Run this command on the command line on the machine to install Certbot. 

```
sudo snap install --classic certbot 
```

_Prepare the Certbot command_

Execute the following instruction on the command line on the machine to ensure that the certbot command can be run. 

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot 
```

__Choose how you'd like to run Certbot__

1. Either get and install your certificates... 

Run this command to get a certificate and have Certbot edit your apache configuration automatically to serve it, turning on HTTPS access in a single step. 

```
sudo certbot --apache 
```

2. Or, just get a certificate 

If you're feeling more conservative and would like to make the changes to your apache configuration by hand, run this command. 

```
sudo certbot certonly --apache 
```

__Test automatic renewal__

The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration. You can test automatic renewal for your certificates by running this command: 

```
sudo certbot renew --dry-run 
```

The command to renew certbot is installed in one of the following locations: 


 - `/etc/crontab/`
 - `/etc/cron.*/*`
 - systemctl list-timers 

__Confirm that Certbot worked__ 

To confirm that your site is set up properly, visit https://yourwebsite.com/ in your browser and look for the lock icon in the URL bar. 

__Získání certifikátu __

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek7.png)

### 5. Konfigurace DNS a vytváření subdomén 

__Proč:__

Konfigurace DNS je zapotřebí, aby byl server škálovatelný a bylo tedy možné vytvořit další subdomény tohoto serveru pro zákazníka.

__Jak vytvořit subdoménu:__

Certainly! Here's how you can configure DNS for wildcard subdomains on Ubuntu Server: 

Install and Configure DNS Server (BIND): 

Install BIND by running the following command: 

```
sql 
sudo apt update 
sudo apt install bind9 
```

Open the BIND configuration file for editing: 

```
bash 
zone "ssibrno.cz" { 
type master; 
file "/etc/bind/db.ssibrno.cz"; 
}; 
```

Create the zone file: 

```
bash 
sudo vim /etc/bind/db.ssibrno.cz 
```

Add the following content to the zone file: 

```
python 
$TTL 86400 
@       IN      SOA     ns1.ssibrno.cz. admin.ssibrno.cz. ( 
                      2023051501 ; Serial 
                      3600       ; Refresh 
                      1800       ; Retry 
                      604800     ; Expire 
                      86400      ; Minimum TTL 
                      ) 
@       IN      NS      ns1.ssibrno.cz. 
@       IN      NS      ns2.ssibrno.cz. 
@       IN      A       YOUR_SERVER_IP 
IN      CNAME   ssibrno.cz. 
```

Replace YOUR_SERVER_IP with the actual IP address of your server. 

Save the file and exit the text editor. 

Restart the BIND service for the changes to take effect: 

```
    sudo service bind9 restart 
```

Configure Apache Virtual Hosts (similar to previous instructions): 

Create a new Apache configuration file for wildcard subdomains: 

```
bash 
sudo vim /etc/apache2/sites-available/wildcard.ssibrno.cz.conf 
```

Add the following content to the file: 

```
less 
<VirtualHost :80> 
ServerName.ssibrno.cz 
ServerAlias *.ssibrno.cz 
VirtualDocumentRoot /var/www/html/%1 
 	<Directory /var/www/html/%1> 
AllowOverride All 
Require all granted 
</Directory> 
</VirtualHost> 
```

Save the file and exit the text editor. 

Enable the virtual host: 

```
sudo a2ensite wildcard.ssibrno.cz.conf 
```

Restart Apache for the changes to take effect: 

```
sudo service apache2 restart 
```

With these configurations in place, users will be able to create subdomains on the ssibrno.cz domain, and each subdomain will have its own directory in the /var/www/html/ path. Make sure to replace YOUR_SERVER_IP with the actual IP address of your server in the DNS configuration. 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek8.png)

### 6. Vytvoření přístupu pro zákazníka 

Naše skupina zvolila Variantu 1.(=Zákazníci budou mít přístup ke svému webu přes SFTP), jelikož jsme si mysleli, že tato varianta bude jednoduší  

Naše řešení tohoto kroku bylo provedeno podle https://linuxhint.com/setup-sftp-server-ubuntu/ : 

#### Setup SFTP Server Ubuntu 

2 years ago 

by__ Samreena Aslam__

 
Data security and credentials encryption are the thumb rules for a system administrator. FTP (File Transfer Protocol) is great for transferring files, but it is not as secure to use over the network. By using this protocol, your data and credentials are transferred without any encryption method. SFTP, abbreviated as Secure File Transfer Protocol, is used for providing better security. SFTP works over the SSH protocol by providing the encryption required to establish a secure connection. Therefore, you can transfer data to or from your local computer system in a secure way. Hence, the secure file transfer protocol (SFTP) is more secure than the simple file transfer protocol (FTP). Sometimes, you may need to provide remote access to the SFTP/FTP server to the development teams or other clients. In this case, SFTP allows you to provide secure limited access to specific directories and files. 

Today’s article will explore how to __configure or set up the SFTP server through SSH on the Ubuntu 20.04__ system using the command-line method. We will see how the SFTP user allows limited permissions to a specific directory for others. 

__Prerequisites__ 

You need root privileges for creating a new SFTP user and for executing the administrative commands. 

__Setting up SFTP Server on Ubuntu 20.04__

Follow the following provided steps to set up the SFTP server on Ubuntu 20.04 system: 

__Step 1: Install SSH__

As we mentioned earlier, SFTP works over SSH. So first, it is required to install SSH on Ubuntu 20.04. If you have not already installed SSH on your Ubuntu system then, install it by running the following apt command: 

```
$ sudo apt install ssh 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek9.png)

__Step 2: Change SSHD configuration for SFTP group__

After installing the SSH, you need to change the ‘/etc/ssh/sshd_config’ SSHD configuration file. So, use vim editor or any other to open this configuration file as follows: 

```
$ sudo vim /etc/ssh/sshd_config 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek10.png) 

Now, paste the following lines at the end or bottom of the file: 

```
Match group sftp 
ChrootDirectory /home 
X11Forwarding no 
AllowTcpForwarding no 
ForceCommand internal-sftp
``` 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek11.png)

The above configuration will allow the sftp users group to access their home directories through the SFTP. However, not allowed to access the normal SSH shell. Save the above-mentioned lines in the configuration file and close it. 

__Step 3: Restart SSH services__

For making the new changes to take effect, restart the SSH service using the ‘systemctl’ command: 

```
$ sudo systemctl restart ssh 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek12.png)

Now, the SSH configuration for SFTP users has been set up on your system. Next, you will create a new SFTP user account and assign permissions. 

__Step 4: Create SFTP users group__

To grant SFTP access to users, you will create SFTP user accounts. First, create a new user group for ‘SFTP’ users. For our convenience, all SFTP users will belong to the same group. So, run the below-mentioned command to create a new SFTP group: 

```
$ sudo addgroup sftp 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek13.png)

__Step 5: Create a new SFTP user__

Once the new group is added, create a new sftp user and then add this user into the sftp group by running the following command: 

```
$ sudo useradd -m sftp_user -g sftp 
```

Here, we have created a new sftp user named ‘samreena’ as follows: 

```
$ sudo useradd -m samreena -g sftp 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek14.png)

Set the password for the newly created sftp user by typing the following command: 

```
$ sudo passwd sftp_user 
$ sudo passwd samreena 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek15.png)

__Step 6: Grant permissions to the specific directory__

In this step, you grant full permissions to the sftp user on their home directory. But, other users on the system are not allowed to access this directory. So, grant access using the ‘chmod’ command as follows: 

```
$ sudo chmod 700 /home/sftp_user/ 
```

The above command will change according to the name of the sftp_user. 

```
$ sudo chmod 700 /home/samreena/ 
```
 
![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek16.png)

Here, the SFTP server configurations are completed. Now, you can log in with the sftp credentials to check either everything is working properly or not. 

__Login through the SFTP__

You can log in via the SFTP by using two different methods: 

1. Connect to the SFTP by using the command line method 

2. Connect to the SFTP using the GUI 

__Method 1: Connect to the SFTP using the command line__

You can connect to the SFTP server either using the IP address or system hostname. We are using the same system on which we have configured the SFTP server. 

Open the terminal and connect via sftp by using the sftp_user name along with the loopback address 127.0.0.1 as follows: 

```
$ sftp sftp_user@127.0.0.1 
$ sftp samreena@127.0.0.1 
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek17.png)

When you connect for the first time via the SFTP, the following dialog appears on the terminal screen. Type ‘yes’ to continue the connecting process. Now, set the password for the sftp user. After that, the following connected to 127.0.0.1 messages shows on the terminal window, and now you logged in on the sftp. 

Now, navigate into the sftp_user’s home directory. Since the sftp user has only access to the home directory. So here, create a new directory with the name ‘test-sftp’ to verify that sftp is working properly. 

```
sftp> cd sftp_user 
sftp> mkdir test-sftp 
sftp> ls
```

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek18.png)

__Method 2: Connect to the SFTP using the GUI__

You can connect to the SFTP server using the GUI SFTP client application. You can either connect with the preferred SFTP client or use the built-in default Ubuntu Nautilus file manager. 

Open the Nautilus file manager using the application menu and then click on the ‘other Locations’. Now, at the bottom of the current window, enter ‘sftp://127.0.0.1’ in the connect to server box and then click on ‘connect’. 

Enter the SFTP account credentials which you have been set up above and click on the connect as follows: 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek19.png)

On a successful connection, the following interface will show: 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek20.png)

Once you connected via the SFTP server, you can access your home directory and its directory contents as follows: 

![Struktura](https://github.com/Tab-e-n/UbuntuServer/blob/main/Obrazky/Obr%C3%A1zek21.png)

__Conclusion__

We configured the SFTP server through the SSH in this article using the command-line on Ubuntu 20.04 system. We explored how to secure the FTP by setting up the SFTP server on the Ubuntu system. Following the above-mentioned guidelines, a computer system across the internet or on your local network can securely access your system files to retrieve and store with assigned permissions. This can be performed either using their preferred SFTP client or via the command line. 

#### Zároveň je potřeba vytvořit ssh klíč pro připojení klienta: 

To create an SSH public key for a specific user to connect to an SFTP server, you can follow these steps: 

1. Generate the SSH key pair: 

   Open a terminal or command prompt. 

   Use the ssh-keygen command to generate the key pair. Specify the desired algorithm (e.g., RSA) and the file name for the key pair. For example: 

   ```
   ssh-keygen -t rsa -f mykey 
   ```

   Press Enter to accept the default location or provide a custom path for saving the key pair. 

2. Provide a passphrase (optional): You can choose to enter a passphrase for added security, but it's not mandatory. Press Enter twice if you don't want to set a passphrase. 

3. The key pair will be generated: This will create two files: mykey (private key) and mykey.pub (public key). 

4. Restrict the key usage (optional): If you want to restrict the usage of the generated key to only SFTP, you can add a command restriction in the authorized_keys file on the SFTP server. Open the ~/.ssh/authorized_keys file for the specific user on the server and add the following line at the beginning: 

   ```
   command="internal-sftp",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa <public-key> 
   ```

   Replace <public-key> with the content of the mykey.pub file. 

5. Save and exit the authorized_keys file. 

   With the SSH public key created and installed on the SFTP server, you should be able to use the corresponding private key (mykey) to connect securely to the SFTP server as the specified user.
 
### 7. ROZBITÍ SERVERU 

Finální dílo Járy Cimrmana a jeho ovlivnění Vítkova chatbota, aby rozbil náš server.
 
Oops... :(

### Další důležité příkazy: 

`ssh –i “umístění private key” ubuntu@penize-zdarma.ssibrno.cz` - příkaz pro připojení k serveru jako správce 

`history` - příkaz pro zobrazení historie příkazů 

`sudo reboot` - příkaz pro restart serveru 

`sudo apt update && sudo apt upgrade` – aktualizace systému a obnovení packagů 

`sudo systemctl reboot “služba”` - restartuje jmenovanou službu 

`sudo ufw allow "port”` - otevře daný port (např.: HTTPS je port 443) 

`sudo nano /etc/ssh/sshd_config` - úprava SSH configu
 
`cd /var/www/html` - přístup do složky s webovou stránkou
 
`sudo wget "soubor"` - stahování souborů z internetu
 
`cd /etc/apache2/sites-enabled/` - složka s configy domén
 
`sudo a2ensite penize-zdarma2.conf` - povolení fungování domény/subdomény
