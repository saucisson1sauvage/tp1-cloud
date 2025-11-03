# Tp 2 Matys Philipon-gouin cloud

##  I. Un p'tit nom DNS

### 🌞 Prouvez que c'est effectif

```
mpg@skibidi-Sigma-Ohio-Rizz:~/.ssh$ nslookup contient-pas-meow.spaincentral.cloudapp.azure.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	contient-pas-meow.spaincentral.cloudapp.azure.com
Address: 158.158.51.108

```
```json
mpg@skibidi-Sigma-Ohio-Rizz:~$ az vm list-ip-addresses
[
  {
    "virtualMachine": {
      "name": "azure2.tp1",// je m'etait un peut melanger sur les noms ca c'est la db azure2tp2
      "network": {
        "privateIpAddresses": [
          "10.0.0.5"
        ],
        "publicIpAddresses": []
      },
      "resourceGroup": "theSkibidiGroup"
    }
  },
  {
    "virtualMachine": {
      "name": "Skazure1tp2",// ca l'app azure1tp2
      "network": {
        "privateIpAddresses": [
          "10.0.0.6"
        ],
        "publicIpAddresses": [
          {
            "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup/providers/Microsoft.Network/publicIPAddresses/Skazure1tp2PublicIP",
            "ipAddress": "158.158.51.108",
            "ipAllocationMethod": "Static",
            "name": "Skazure1tp2PublicIP",
            "resourceGroup": "theSkibidiGroup",
            "zone": null
          }
        ]
      },
      "resourceGroup": "theSkibidiGroup"
    }
  }
]
```



```html
pg@skibidi-Sigma-Ohio-Rizz:~$ curl http://contient-pas-meow.spaincentral.cloudapp.azure.com:8000/
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Purr Messages - Cat Message Board</title>
    <style>
        /* Modern CSS with cat-themed design */
        :root {
            --primary: #ff6b6b;
            --secondary: #4ecdc4;
```


## 2. Gooooo

### 🌞 Tester cloud-init
```
pg@skibidi-Sigma-Ohio-Rizz:~$ az vm create --resource-group theSkibidiGroup --name azuretesttp2 --image Ubuntu2404 --ssh-key-value /home/mpg/.ssh/cloud_tp.pub --storage-sku Standard_LRS --size Standard_B1ls  --public-ip-address "" --custom-data ~/cloud-init.txt 
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
{
  "fqdns": "",
  "id": "/subscriptions/0cf8146f-081c-45c7-9b87-f3d40458546e/resourceGroups/theSkibidiGroup/providers/Microsoft.Compute/virtualMachines/azuretesttp2",
  "location": "spaincentral",
  "macAddress": "60-45-BD-FC-F5-F1",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.7",
  "publicIpAddress": "",
  "resourceGroup": "theSkibidiGroup"
}
```

### 🌞 Vérifier que cloud-init a bien fonctionné

```
azureuser@azuretesttp2:~$ sudo systemctl status cloud-init
● cloud-init.service - Cloud-init: Network Stage
     Loaded: loaded (/usr/lib/systemd/system/cloud-init.service; enabled; prese>
     Active: active (exited) since Sat 2025-11-01 17:30:22 UTC; 2min 30s ago
   Main PID: 703 (code=exited, status=0/SUCCESS)
        CPU: 1.717s

Nov 01 17:30:22 azuretesttp2 cloud-init[707]: |       . . B     |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: |      . o = * .  |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: |   . .   * o * . |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: |  . . . S o + =  |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: | . = . . o . . . |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: | oB B   o .   o  |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: |oXEX .   .. .  . |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: |+.X..     .+...  |
Nov 01 17:30:22 azuretesttp2 cloud-init[707]: +----[SHA256]-----+
Nov 01 17:30:22 azuretesttp2 systemd[1]: Finished cloud-init.service - Cloud-in>
 ^X
azureuser@azuretesttp2:~$ cloud-init status
status: done
```
## 3. Write your own¶

### 🌞 Utilisez cloud-init pour préconfigurer une VM comme azure2.tp2 


```yaml
#cloud-config
disable_root: false
system_info:
  default_user:
    name: azureuser

users:
  - name: azureuser
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: sudo
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHPqB1kVGqd9hl5tqTesybyZhrjjyJtRaY8wuF17GIdd mpg@skibidi-Sigma-Ohio-Rizz


  - name: saucisson_sauvage
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: sudo
    shell: /bin/bash
    passwd: $6$rounds=4096$lsDuQ0xAlSYJKN2W$pR1I9i6KaZYMpRCKo4Pa2QDJ3HkrVagnmVAsEBAExb2qBmkOi11XejBsfMerBZ/k6wRxusDWoya7TpCh.D0Fw/
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHPqB1kVGqd9hl5tqTesybyZhrjjyJtRaY8wuF17GIdd mpg@skibidi-Sigma-Ohio-Rizz



package_update: true
package_upgrade: true
packages:
  - mysql-server
  - mysql-client-core-8.0


runcmd:
  - systemctl start mysql
  - systemctl enable mysql
  - until mysqladmin ping --silent; do sleep 1; done
  - mysql < /init.sql
  - rm -f /init.sql

write_files:
  - path: /init.sql
    permissions: '0644'
    content: |
      CREATE DATABASE meow_database;
      CREATE USER 'meow'@'%' IDENTIFIED BY 'meow';
      GRANT ALL ON meow_database.* TO 'meow'@'%';
      FLUSH PRIVILEGES;

```

```
azureuser@azureteste:~$ sudo mysql -e "SHOW DATABASES;"
+--------------------+
| Database           |
+--------------------+
| information_schema |
| meow_database      |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```
## 1. Un premier secret

```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az keyvault create --name keysig --resource-group THESKIBIDIGROUP  --location spaincentral --enable-rbac-authorization false

```
<!-- en haut pour creer keyvault-->
```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az keyvault secret set --vault-name keysig --name skibidisecret --value 69
```
<!-- la pour foutre une value dedans -->
<!--
--value 
--description
--enabled true|false
--expires YYYY-MM-DD
--not-before YYYY-MM-DD: Activation date
--tags key=value: Tags
--file /path/to/file: From file.
 -->
```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az vm identity assign --name Skazure1tp2 --resource-group THESKIBIDIGROUP 
```
```
mpg@skibidi-Sigma-Ohio-Rizz:~$ az keyvault set-policy --name keysig --object-id f9f67c71-8c49-4dba-bcd6-ce2fe60c1fb3 --secret-permissions get list
```

<!-- managed id
--system-assigned
--user-assigned <ID>
 -->
<!-- f9f67c71-8c49-4dba-bcd6-ce2fe60c1fb3 id app-->
### 🌞 Récupérer votre secret depuis la VM



```json
azureuser@azure1tp2-app:~$ az keyvault secret show \
  --vault-name keysig  --name skibidisecret
{
  "attributes": {
    "created": "2025-11-02T16:38:18+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoverableDays": 90,
    "recoveryLevel": "Recoverable+Purgeable",
    "updated": "2025-11-02T16:38:18+00:00"
  },
  "contentType": null,
  "id": "sum",
  "kid": null,
  "managed": null,
  "name": "skibidisecret",
  "tags": {
    "file-encoding": "utf-8"
  },
    "value": "69"
}
```

## 2. Gérer les secrets de l'application

### 🌞 Coder un ptit script bash : get_secrets.sh

```shell
#!/bin/bash

az login --identity --allow-no-subscriptions
sed -i 's/DB_PASSWORD=.*/DB_PASSWORD=/' /opt/meow/.env
Password=$(az keyvault secret show   --vault-name keysig  --name DBPASSWORD | grep value | cut -d '"' -f4 )
sed -i "s/DB_PASSWORD=.*/DB_PASSWORD=$Password/" /opt/meow/.env
```

### 🌞 Environnement du script get_secrets.sh, il doit :

```
azureuser@azure1tp2-app:/usr/local/bin$ ls -l
total 4
-rwxrwx--- 1 webapp root 285 Nov  2 21:55 get_secrets.sh
```

## B. Exécution automatique

### 🌞 Ajouter le script en ExecStartPre= dans webapp.service


```
[Unit]
Description=Super Webapp MEOW

[Service]
User=webapp
WorkingDirectory=/opt/meow
ExecStartPre=/usr/local/bin/get_secrets.sh
ExecStart=/opt/meow/bin/python app.py

[Install]
WantedBy=multi-user.target
azureuser@azure1tp2-app:/$ sudo systemctl start webapp

```
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->
<!-- -->

