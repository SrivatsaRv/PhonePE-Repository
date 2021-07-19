# Useful Commands - PPEC Cloud

Frequently used commands. This is a helpful page to [add to your Favorites](https://www.notion.so/Navigating-with-the-sidebar-7ef7287cee00464d9a813073b02ce24a).

# 🚚 Configuring PPEC(PPEC)

**1-  CONFIGURE PPEC -** `*ppec` Configure ppec client_id and client_secret. (visit ppec-creds-ui to acquire client_id and client_secret). On your MAC Terminal , do this.*

```
> **ppec configure   -**visit ppec-creds-ui to acquire client_id and client_secret). 

> **ppec auth login  -** do it everyime you configure

> **ppec --help  -** lets you you help verify and check if ppec utility is running.

> **ppec vm list -r nb6 -** check if the ppec is working properly, should show list.
```

# 🚢 Spinning up VMs from Stage

*When you want to spin up VMs from Stage env, follow the steps below, on your terminal.* 

1. ***Create a YAML file on your local machine,   / directory.*** 

```
**Template Code:** nano stg-<appname-yourname>00x.yml
 
**Example:  nano stg-rmqsrivatsa001.yml**

stg-<appname-yourname>00x:
    DOMAIN: phonepe.nb6
    OS: focal
    RT: SRESTAGE-526
    SIZE: C1M2048
    TEAM:
      - INTERN 

**DONT FORGET THE INTERN TAG
NAME YOUR VMs PROPERLY**
```

***2. Now run the following commands in order to spin up VM from the YAML Config above***

- ***PPPEC Command to Create the VM** , 
it will ask are you sure to run this in nb6?   Give **y**
it will ask to check VM status, give **Y**
wait for ping, it won't work immediately*

```
   **> ppec vm create -i stg-rmqsrivatsa001.yml -r nb6**
```

***4- Mesos Site - VM Creation Confirmation + VM Ping Status**
- confirm your VM name is there, that's all.* 

```
**http://stg-mesosm001.phonepe.nb6:5050/

Paste in PPEC
> ppec vm status -h stg-ansiblelekhana001 -r nb6**
```

***5- Create ssh cert for your VM-SSH validity***

- On your terminal ,

```
**> mkdir cert
> nano cert

go to this - https://stg-mankuthimma.phonepe.com/ 
- sign in using phonepe email
- copy the ssh key, into your nano cert file

ssh-rsa-cert-v01@openssh.com AAAA........ and ends with ............
.................												  /umnczcQjFt/+m9foSUJz5Zw==

Template Code 
> ssh -i cert -i ~/.ssh/id_rsa stg-<appname-yourname>00x.phonepe.<region>

Example Code
> ssh -i cert -i ~/.ssh/id_rsa stg-aerospikesrivatsa001.phonepe.nb6

YOU ARE NOW IN!**
```

### *6- Delete your VM*

```
> copy your machine's information from the PhonePE Asset Tracker (AT) 
> paste it in delete.yml in you Macbook 

now, 

```