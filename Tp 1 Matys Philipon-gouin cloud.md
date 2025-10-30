# Tp 1 Matys Philipon-gouin cloud

## 2. Une paire de clés SSH

### A. Choix de l'algorithme de chiffrement

#### **🌞 Déterminer quel algorithme de chiffrement utiliser pour vos clés**

[tencentcloud.com](https://www.tencentcloud.com/techpedia/102501)

> Computational Complexity

> Key Size

> Security Vulnerabilities

> Key Management

> Limited Security for Small Key Sizes

[nsf.gov](https://par.nsf.gov/servlets/purl/10507285)




j'etait parti sur kyber mais c'est plutot complexe a generer donc je suis parti sur ed25519
[ibm.com](https://www.ibm.com/fr-fr/think/topics/quantum-safe-cryptography)



### B. Génération de votre paire de clés

#### **🌞 Générer une paire de clés pour ce TP**

```
mpg@skibidi-Sigma-Ohio-Rizz:/home$  ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/mpg/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/mpg/.ssh/id_ed25519
Your public key has been saved in /home/mpg/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:inWmroZsNJeddDYXyxtahsJPkZJik8RRnjJjhBmMM2Q mpg@skibidi-Sigma-Ohio-Rizz
The key's randomart image is:
+--[ED25519 256]--+
|.E.*++.. .       |
|= +.*.o.o .      |
| o .=+o. + o     |
|   . ++ * B      |
|     +.BS* o     |
|  o ooo=o .      |
| o +. o          |
|  + ..           |
| . ....          |
+----[SHA256]-----+
```
```
mpg@skibidi-Sigma-Ohio-Rizz:~/.ssh$ mv id_ed25519 cloud_tp
```

#### **🌞 Configurer un agent SSH sur votre poste**


``` 
mpg@skibidi-Sigma-Ohio-Rizz:~/.ssh$ ssh-add ~/.ssh/cloud_tp 
Enter passphrase for /home/mpg/.ssh/cloud_tp: 
Identity added: /home/mpg/.ssh/cloud_tp (mpg@skibidi-Sigma-Ohio-Rizz)
```

## 1. Depuis la WebUI
c'est down donc ca sera via la cli 



v ( surtout pour moi)
```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az group create --name theSkibidiGroup --location spaincentral
{
  "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup",
  "location": "spaincentral",
  "managedBy": null,
  "name": "theSkibidiGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az vm create --resource-group theSkibidiGroup --name SkibidiHQ_1 --admin-username azureuser --image Ubuntu2404 --ssh-key-value /home/mpg/.ssh/cloud_tp.pub --storage-sku Standard_LRS --size Standard_B1ls
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
{
  "fqdns": "",
  "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup/providers/Microsoft.Compute/virtualMachines/SkibidiHQ_1",
  "location": "spaincentral",
  "macAddress": "7C-ED-8D-64-54-76",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "tpabo jk jk",
  "resourceGroup": "theSkibidiGroup"
}
```


#### **🌞 Connectez-vous en SSH à la VM pour preuve**
```
mpg@skibidi-Sigma-Ohio-Rizz:~$ ssh azureuser@qqch
The authenticity of host 'qqch (qqch)' can't be established.
ED25519 key fingerprint is SHA256:Eag9mf06sHMgxF3bD+r1SWcLkp1tGHPoVugr3zgRwm0.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'qqch' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1012-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Oct 29 20:37:16 UTC 2025

  System load:  0.08              Processes:             111
  Usage of /:   5.6% of 28.02GB   Users logged in:       0
  Memory usage: 68%               IPv4 address for eth0: 10.0.0.4
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@SkibidiHQ1:~$ 
```
## 2. az : a programmatic approach

#### **🌞 Créez une VM depuis le Azure CLI : azure1.tp1**

voir comamndes au dessus

```
mpg@skibidi-Sigma-Ohio-Rizz:~$ ssh azureuser@qqch
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1012-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Oct 29 21:38:49 UTC 2025

  System load:  0.19              Processes:             117
  Usage of /:   5.6% of 28.02GB   Users logged in:       0
  Memory usage: 68%               IPv4 address for eth0: 10.0.0.4
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

```

#### **🌞 Une fois connecté, prouvez la présence...**

```
azureuser@azure1:~$ systemctl list-units | grep "walinuxagent.service"
  walinuxagent.service                                                                                                                                       loaded active running   Azure Linux Agent
```
```
azureuser@azure1:~$ systemctl list-units | grep "cloud-init.service"
  cloud-init.service                                                                                                                                         loaded active exited    Cloud-init: Network Stage
```



```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az vm create --resource-group theSkibidiGroup --name azure2.tp1 --admin-username azureuser --image Ubuntu2404 --ssh-key-value /home/mpg/.ssh/cloud_tp.pub --storage-sku Standard_LRS --size Standard_B1ls  --public-ip-address ""
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
{
  "fqdns": "",
  "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup/providers/Microsoft.Compute/virtualMachines/azure2.tp1",
  "location": "spaincentral",
  "macAddress": "7C-1E-52-2F-BC-D6",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "",
  "resourceGroup": "theSkibidiGroup"
}
```
## 3. Spawn moar moar moaaar VMs
### A. Another VM another friend :d

#### **🌞 Créez une deuxième VM : azure2.tp1**

```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az vm create --resource-group theSkibidiGroup --name azure2.tp1 --admin-username azureuser --image Ubuntu2404 --ssh-key-value /home/mpg/.ssh/cloud_tp.pub --storage-sku Standard_LRS --size Standard_B1ls  --public-ip-address ""
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
{
  "fqdns": "",
  "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup/providers/Microsoft.Compute/virtualMachines/azure2.tp1",
  "location": "spaincentral",
  "macAddress": "7C-1E-52-2F-BC-D6",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "",
  "resourceGroup": "theSkibidiGroup"
}
```

#### **🌞 Affichez des infos au sujet de vos deux VMs**


```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az vm list-ip-addresses
[
  {
    "virtualMachine": {
      "name": "azure1.tp1",
      "network": {
        "privateIpAddresses": [
          "10.0.0.4"
        ],
        "publicIpAddresses": [
          {
            "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup/providers/Microsoft.Network/publicIPAddresses/azure1.tp1PublicIP",
            "ipAddress": "ton chat pensai que ct un flag :d",
            "ipAllocationMethod": "Static",
            "name": "azure1.tp1PublicIP",
            "resourceGroup": "theSkibidiGroup",
            "zone": null
          }
        ]
      },
      "resourceGroup": "theSkibidiGroup"
    }
  },
  {
    "virtualMachine": {
      "name": "azure2.tp1",
      "network": {
        "privateIpAddresses": [
          "10.0.0.5"
        ],
        "publicIpAddresses": []
      },
      "resourceGroup": "theSkibidiGroup"
    }
  }
]
```

### B. Config SSH client

#### **🌞 Configuration SSH client pour les deux machines**

```bash
Host az1tp1
  HostName cette foi c mon chat qui l as mange
  User azureuser
  ForwardAgent yes
## ^ bouge le fiac de l agent


Host az2tp1
  HostName 10.0.0.5
  User azureuser
  ProxyJump az1tp1
```

#### **🌞 Installer MySQL/MariaDB sur azure2.tp1**

```bash
azureuser@azure2:~$ sudo apt install mysql-server
```

#### **🌞 Démarrer le service MySQL/MariaDB sur azure2.tp1**

```bash
 azureuser@azure2:~$ sudo systemctl start mysql.service
```


#### **🌞 Ajouter un utilisateur dans la base de données pour que mon app puisse s'y connecter**

``` 
azureuser@azure2:~$ sudo mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.43-0ubuntu0.24.04.2 (Ubuntu)

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```
```sql
mysql> CREATE DATABASE meow_database;
Query OK, 1 row affected (0.03 sec)

mysql> CREATE USER 'meow'@'%' IDENTIFIED BY 'meow';
Query OK, 0 rows affected (0.03 sec)

mysql> GRANT ALL ON meow_database.* TO 'meow'@'%';
Query OK, 0 rows affected (0.05 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)
```

#### **🌞 Ouvrez un port firewall si nécessaire**

```bash
azureuser@azure2:~$ sudo ufw allow 3306
Rules updated
Rules updated (v6)
```
```
zureuser@azure1:~$ telnet 10.0.0.5 3306
Trying 10.0.0.5...
Connected to 10.0.0.5.
Escape character is '^]'.
[
8.0.43-0ubuntu0.24.04.2 ~iZIm�
PRZg9Uw6'caching_sha2_password2#08S01Got timeout reading communication packetsConnection closed by foreign host.
```

```
 sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

```
```
bind-address = 0.0.0.0

```
```
sudo systemctl start/restart mysql
```
#### **🌞 Récupération de l'application sur la machine**
```
azureuser@azure2:~$ git clone https://gitlab.com/it4lik/b2-pano-cloud-2025.git
Cloning into 'b2-pano-cloud-2025'...
remote: Enumerating objects: 381, done.
remote: Counting objects: 100% (301/301), done.
remote: Compressing objects: 100% (298/298), done.
remote: Total 381 (delta 140), reused 0 (delta 0), pack-reused 80 (from 1)
Receiving objects: 100% (381/381), 14.26 MiB | 29.43 MiB/s, done.
Resolving deltas: 100% (163/163), done.
```
``` 
azureuser@azure2:~$ sudo mv b2-pano-cloud-2025/ /opt/meow
```

#### **🌞 Installation des dépendances de l'application**

```
(venv) azureuser@azure2:/opt/meow$ ./bin/pip install -r requirements.txt
```

#### **🌞 Configuration de l'application**

```
azureuser@azure2:/opt/meow$ cat .env
# Flask Configuration
FLASK_SECRET_KEY=ewnFw95H7qBeGiVvkQl9YmnJohW6NCMMqR0arxfnWYASeCDvzwQwzLxMCboAOi3e
FLASK_DEBUG=False
FLASK_HOST=0.0.0.0
FLASK_PORT=8000

# Database Configuration
DB_HOST=localhost 10.0.0.5
DB_PORT=3306
DB_NAME=meow_database
DB_USER=meow
DB_PASSWORD=meow

```
#### **🌞 Gestion de users et de droits**

```bash
azureuser@azure2:/opt/meow$ sudo useradd webapp
azureuser@azure2:/opt/meow$ sudo chown -R webapp:webapp .
azureuser@azure2:/opt/meow$ sudo chmod -R o-rwx /opt/meow
azureuser@azure2:/opt/meow$ ls -l
ls: cannot open directory '.': Permission denied
azureuser@azure2:/opt/meow$ sudo ls -l
total 64
-rw-rw---- 1 webapp webapp 20131 Oct 30 08:12 LICENSE
-rw-rw---- 1 webapp webapp  3827 Oct 30 08:12 app.py
drwxrwx--- 2 webapp webapp  4096 Oct 30 08:19 bin
-rw-rw---- 1 webapp webapp   223 Oct 30 08:12 docker-compose.yml
drwxrwx--- 5 webapp webapp  4096 Oct 30 08:12 docs
drwxrwx--- 4 webapp webapp  4096 Oct 30 08:19 include
drwxrwx--- 3 webapp webapp  4096 Oct 30 08:14 lib
lrwxrwxrwx 1 webapp webapp     3 Oct 30 08:14 lib64 -> lib
-rw-rw---- 1 webapp webapp  1497 Oct 30 08:12 mkdocs.yml
-rw-rw---- 1 webapp webapp   148 Oct 30 08:14 pyvenv.cfg
-rw-rw---- 1 webapp webapp    58 Oct 30 08:12 requirements.txt
drwxrwx--- 2 webapp webapp  4096 Oct 30 08:12 templates
drwxrwx--- 5 webapp webapp  4096 Oct 30 08:15 venv
```

#### **🌞 Création d'un service webapp.service pour lancer l'application**

```bash
azureuser@azure2:/opt/meow$ sudo nano /etc/systemd/system/webapp.service
azureuser@azure2:/opt/meow$ sudo chown webapp:webapp /etc/systemd/system/webapp.service
azureuser@azure2:/opt/meow$ sudo systemctl daemon-reload
```



#### **🌞 Ouverture du port80 dans le(s) firewall(s)**

```bash
azureuser@azure2:/opt/meow$ sudo ufw allow 80
Rules updated
Rules updated (v6)
azureuser@azure2:/opt/meow$ sudo ufw reload
Firewall reloaded
azureuser@azure2:/opt/meow$ journalctl | grep 'ufw'
Oct 30 01:32:34 ubuntu systemd[1]: Starting ufw.service - Uncomplicated firewall...
Oct 30 01:32:34 ubuntu systemd[1]: Finished ufw.service - Uncomplicated firewall.
Oct 30 01:51:58 azure2 sudo[3032]: azureuser : TTY=pts/0 ; PWD=/home/azureuser ; USER=root ; COMMAND=/usr/sbin/ufw allow 3306
Oct 30 07:18:01 azure2 systemd[1]: Starting ufw.service - Uncomplicated firewall...
Oct 30 07:18:01 azure2 systemd[1]: Finished ufw.service - Uncomplicated firewall.
Oct 30 09:19:17 azure2 sudo[9137]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw allow 80
Oct 30 09:21:15 azure2 sudo[9155]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw reload
Oct 30 09:21:32 azure2 sudo[9159]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw enable
Oct 30 09:21:56 azure2 sudo[9163]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw allow 22
Oct 30 09:22:01 azure2 sudo[9177]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw reload
Oct 30 09:22:05 azure2 sudo[9181]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw enable
Oct 30 09:22:17 azure2 sudo[9327]: azureuser : TTY=pts/0 ; PWD=/opt/meow ; USER=root ; COMMAND=/usr/sbin/ufw reload
```

#### **🌞 L'application devrait être fonctionnelle sans soucis à partir de là**

```
azureuser@azure1:~$ sudo systemctl start webapp
azureuser@azure1:~$ sudo systemctl status webapp
```
ok ca marche pas et j'ai du faire de la merde a la racine meme de la vm parce que j'ai lu trop vite... je vais juste continuer sur le tp 2 :d 
