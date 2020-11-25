# Production websites 

Please refer to wiki for the latest information: https://github.com/k8-proxy/GW-proxy/wiki/Production-websites


# OVAs

- ICAP Server
- Glasswall Engineering Website Proxy
- Minio Server 
- Minio Proxy 


## ICAP server OVA

- Download OVA file from [here](https://glasswall-sow-ova.s3.eu-west-2.amazonaws.com/vms/ICAP-Server/ICAP-Rancher.ova?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA3NUU5XSYVTP3BV6R%2F20201120%2Feu-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201120T151452Z&X-Amz-Expires=31536000&X-Amz-SignedHeaders=host&X-Amz-Signature=34aa9f375b4ea9e2ec3dee6c54d5ede62513c2a2c109290e2779d54ac44a9d8e)

- Open VirtualBox and import downloaded OVA file: icap-Rancher.ova

- Once OVA is imported, go to Settings>Network>Adapter 2 
 
  `Name: VirtualBox Host-Only Ethernet Adapter`

  `Attached to: Host-Only Adapter`

- Start ICAP Server VM

- Login (username: **user**, password: **secret**)

- In command line shell, type:
  
  `ip a show eth1`
​
- Check the ip address for eth1 (this address to be used in the following step)

- Give 5-10min for ICAP Server to completely start

- On your localhost machine (make sure you have c-icap-client installed) run below command to test the connectivity to ICAP server:

  `c-icap-client -i 192.168.56.97`

- Expected results: The command should respond with 200 OK.

- You can also verify that server is working correctly by rebuilding a file:

  `c-icap-client -f ./test.pdf -i 192.168.56.97  -p 1344 -s gw_rebuild -o rebuilt.pdf -v`

#### Here is the video with above instructions: [ICAP Server and Glasswall Engineering Website OVAs](https://www.loom.com/share/08068432eebd48cba0e7ffe4480bfb4f)



## Glasswall Engineering OVA
​
Glasswall Engineering OVA for demoing Glasswall Rebuild engine proxy for **engineering.glasswallsolutions.com** website

### Make sure that ICAP Server OVA is imported into VM and is started. Glasswall Enfineering OVA is dependent on previous one.
- Download OVA file from [here](https://glasswall-sow-ova.s3.eu-west-2.amazonaws.com/vms/Engineering-website/glasswall-engineering.ova?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA3NUU5XSYVTP3BV6R%2F20201120%2Feu-west-2%2Fs3%2Faws4_request&X-Amz-Date=20201120T151429Z&X-Amz-Expires=31536000&X-Amz-SignedHeaders=host&X-Amz-Signature=5c105c03c4e54e131a673705ee9c7603b73eea34d9d9b9b46eb84219aa80df74)

- Open VirtualBox and import downloaded OVA file: glasswall-engineering.ova

- Once OVA is imported, go to Settings>Network>Adapter 2 
 
  `Name: VirtualBox Host-Only Ethernet Adapter`

  `Attached to: Host-Only Adapter`

- Start Glasswall Engineering VM
​
- Login (username: **user**, password: **secret**)
​​​
- Open any browser (preferably Firefox) and try to access: [engineering.glasswallsolutions.com.local.glasswall-icap.com](https://engineering.glasswallsolutions.com.local.glasswall-icap.com)
  
- To avoid SSL Certificate warning on your host machine run:

  `scp user@192.168.56.96:/home/user/ca.pem ca.pem`

  `Import ca.pem certificate in Firefox under Options > View Certificates > Authorithies tab`
  
  `For Chrome this will not work since cert needs to be added on the system`
  
- Navigate to Documentation tab > Sample Files and try to download pdf file and verify that file has "Glasswall Approved" watermark

#### Here is the video with above instructions: [Glasswall Engineering Website OVA](https://youtu.be/vXrL_LYcamo)
#### Here is the video with above instructions: [ICAP Server and Glasswall Engineering Website OVAs](https://www.loom.com/share/08068432eebd48cba0e7ffe4480bfb4f)


## Minio Server OVA

- Download the OVA from [here](https://glasswall-sow-ova.s3.amazonaws.com/vms/Minio-Server/minio_server.ova?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA3NUU5XSYVTP3BV6R%2F20201119%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20201119T184914Z&X-Amz-Expires=31536000&X-Amz-SignedHeaders=host&X-Amz-Signature=c7596ef7f276979d660b9333ca4d5114654ad21d6fd23e6c3e997fbddfbf3132)

- Open VirtualBox and import downloaded OVA file: minio-server.ova

- Once OVA is imported, go to Settings>Network>Adapter 2
    
  `Name: VirtualBox Host-Only Ethernet Adapter`

  `Attached to: Host-Only Adapter`

- Start Minio Server VM

- Login (username: **user**, password: **secret**)

- In command line, type:
  
  `ip a show eth1`

- Check the ip address for eth1 (this address to be used in the following step)

- In your local hosts file (on win: C:\Windows\System32\drivers\etc, on MAC/Linux: /etc/hosts) add following lines

  `<VM IPADDRESS> minio.server`
Example:

    192.168.56.90 minio.server

- Open any browser and try to access: http://minio.server

- Login to Minio Server (username: **user**, password: **secret_password**)

#### Here is the video with above instructions:

[![Minio server OVA](https://img.youtube.com/vi/itMyB8-HTMk/0.jpg)](https://www.youtube.com/watch?v=itMyB8-HTMk)


## Minio Proxy OVA

### You setup ICAP Server and Minio Server as per steps above and they are up and running

- Download the Minio Proxy OVA from [here](https://glasswall-sow-ova.s3.amazonaws.com/vms/Minio-Server/minio_proxy.ova?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA3NUU5XSYVTP3BV6R%2F20201119%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20201119T185407Z&X-Amz-Expires=31536000&X-Amz-SignedHeaders=host&X-Amz-Signature=deb1b64f15610c5d201b8c62f74176155e42bfd0700fd85498040ac998a7a4d6)

- Open VirtualBox and import downloaded OVA file: minio-proxy.ova

- Once OVA is imported, go to Settings>Network>Adapter 2
    
  `Name: VirtualBox Host-Only Ethernet Adapter`

  `Attached to: Host-Only Adapter`

- Start Minio Proxy VM

- Login (username: **user**, password: **secret**)

- Access Proxied Minio Server at: http://minio.server.local.glasswall-icap.com/

- Note: It takes some time for page to load

- Login to Minio Proxied Server (username: **user**, password: **secret_password**)

## Possible issues and how to fix them

- If you are not able to rebuild file using ICAP-Server VM:
    - Windows/WSL2 are not legit way to test that (can be added as exception) 

    - If same issue is present on Linux: shut down virtual box, run in linux local host `sudo apt install build-essential dkms virtualbox-ext-pack`, in virtualbox: file> host network manager > make sure that the adapter created is enabled, then make sure that when the vm is shut down, in the vm's settings you can see the adapter that was created earlier in adapter 2

- If you get NGINX error "Faithfully yours" in VM run:
    
    `sudo docker-compose down`
    
    `sudo docker-compose up -d`

- If you get error for invalid URL containing additional `.local` you probably started GW ENG VM or Minio Proxy VM before starteing ICAP Server OVA. Make sure that ICAP Server OVA is always started first. 

- Do not change any local host network or firewall configuration during or after starting the VMs it can compromise the adapters. 

- In case you get error code 9 in your browser, it is due to ICAP TimeBomb for License files. Give it some time, it will disappears.

