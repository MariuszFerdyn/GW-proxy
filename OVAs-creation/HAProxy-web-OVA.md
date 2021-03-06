# Creating & exporting HAProxy-WEB OVA on VMware ESXI



## Creating the VM

* Access esxi server with valid credentials   [esxi01.glasswall-icap.com](http://esxi01.glasswall-icap.com) 

* Create a new Ubuntu linux (64-bit) VM with minimal hardware specs (1 CPU , 1 GB ram & 16 GB of Harddisk(remember to make disk Provisioning to be thin provisioned  )) 

  ![image](https://user-images.githubusercontent.com/58347752/100459771-b0a60a80-30ce-11eb-959e-018d88a8cf2b.png)

* Also CD/DVD drive is connected at power on and choose the ISO to boot from

  ![image](https://user-images.githubusercontent.com/58347752/100460151-66715900-30cf-11eb-914e-2f802acb5052.png)

* Finish installation and boot the machine with default configuration

* In the network configuration, edit the IPV4 method to be manual and add the network configuration 

  ![image](https://user-images.githubusercontent.com/58347752/100460549-0a5b0480-30d0-11eb-89cb-5cabfeebbefd.png)

* Set the username to be glasswall and the agreed password (same password as the controller VM)

Once installation is done restart the VM and press enter when it asks to remove the CD



## Installation

* Switch to root user and execute the following commands 

```bash
sudo su -
```

```bash
apt update
```

```bash
apt upgrade -y
```

```bash
apt install -y haproxy
```

* Edit Haproxy configuration file by appending the following to it.

```bash
cat >> /etc/haproxy/haproxy.cfg << EOF
#The frontend is the node by which HAProxy listens for connections.
frontend glasswall
        bind *:80
        bind *:443
        option tcplog
        mode tcp
        default_backend nodes        
#Backend nodes are those by which HAProxy can forward requests
backend nodes
        mode tcp
        balance roundrobin
        option ssl-hello-chk
        server web01 54.78.209.23:443 check
#Haproxy monitoring Webui(optional) configuration, access it <Haproxy IP>:32700
listen stats
bind :32700
stats enable
stats uri /
stats hide-version
stats auth username:password

EOF

```

```bash
systemctl restart haproxy.service
```

* Make sure haproxy is active and running

```bash
systemctl status haproxy.service
```



## Client configuration 

* Add hosts records to your client system hosts file ( i.e **Windows**: C:\Windows\System32\drivers\etc\hosts , **Linux, macOS and  Unix-like:** /etc/hosts ) as follows

```
91.109.25.90 glasswallsolutions.com.glasswall-icap.com
```

make sure that tcp ports **80** and **443** are reachable and not blocked by firewall.

## Access the proxied site

* You can access the proxied site by browsing [glasswallsolutions.com.glasswall-icap.com](https://glasswallsolutions.com.glasswall-icap.com).
* Verify that your access is established through the haproxy loadbalancer through the network tab, the Request address should show the Loadbalancer server IP as shown below

![image](https://user-images.githubusercontent.com/58347752/100607205-4500af00-3313-11eb-8f14-b075e74108a7.png)

## Exporting OVA

* Shut down the machine 
* Open the controller machine (Or from your local machine, just the controller machine speed the things up)
* Run the following command to export the VM with OVA extension, it will be exported in your current working directory.

Note: the username and password to be provided here are the initial ESXI server credentials  

```bash
ovftool vi://46.165.225.145/HAProxy-WEB ./HAProxy-WEB.ova
```

![image](https://user-images.githubusercontent.com/58347752/100608447-15eb3d00-3315-11eb-90b9-788e0d59a0e6.png)

## Importing OVA to ESXI

* From the controller (or from whatever the machine you have exported the OVA file to) , access the esxi server [esxi01.glasswall-icap.com](http://esxi01.glasswall-icap.com) 
*  Register a new VM and choose to be deployed from OVA or OVF file option
* Upload the OVA file and then finish the installation with default configuration
* Wait the upload to be done and you are good to go.