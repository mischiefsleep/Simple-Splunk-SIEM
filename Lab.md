# Simple Splunk SIEM on CentOS or Ubuntu


THIS LAB IS UNDER DEVELOPMENT!!  

## CentOS
1. Run  
```
sudo yum update
```
2. Install Docker  
```	
sudo yum-config-manager --add-repo https://download.docker.com/linux/conetos/docker-ce.repo \
yum-config-manager --enable docker-ce-stable \
yum install containerd.io \
yum install docker-ce --nobest && \
systemctl start docker \
docker run hello-world
```

## Ubuntu
1. Run   
```
sudo apt update && sudo apt upgrade -y
```

2. Install Docker  
```
sudo apt install containerd.io && \
sudo apt install docker-ce && \
sudo service docker start && \
sudo docker run hello-world
```
Note: This install will work on any *nix system granted you have docker installed and running, and you replace the first location after the -v flag with the location of your log files.  

## Install Portainer

```
sudo docker run -d --name portainer-01 --restart unless-stopped -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```
### (CentOS Firewall Config Only)
```
sudo firewall-cmd --permanent --add-port=9000/tcp && \
sudo firewall-cmd reload
```
### (Ubuntu Firewall Config Only)
```
sudo ufw allow 9000 && \
sudo systemctl restart ufw
```
Log into portainer with browser url http://localhost:9000  

## Build Splunk Container

Primary Splunk: Keep in mind when running these commands to change the password.  
```
sudo docker run -d -p 8000:8000 -e 'SPLUNK_START_ARGS=--accept-license' -e 'SPLUNK_PASSWORD=<password>' --name splunk --hostname splunk -v /var/log:/var/log/HOST splunk/splunk:latest`
```
If you would like to run with my pre-set SSH Connection and Commands Run Dashboard I have created you can do so by changing the last variable in the command "splunk/splunk:latest" to "ohlittlebrain/centos:Splunk" or "ohlittlebrain/ubuntu:Splunk"  

## For Collection of a Single Host

Log into Splunk with browser url http://localhost:8000  

1. Once Logged in, click on Add Data.
2. Scroll down and click Monitor
3. From here, we will select the option on the left that says "Files and Directories"
4. Browse and select the /var/log/HOST directory
5. Click the Next button, and then the Review button leaving the default settings
6. Finally click Submit and on the next page start searching!

## For Collection of Multiple Hosts

Follow all steps above to get docker running on the host, and then instead of a full splunk we will just install a forwarder.   

Splunk Forwarder: Keep in mind when running these commands to change the password.  
```
sudo docker run -d -p 9997:997 -e 'SPLUNK_START_ARGS=--accept-license' -e 'SPLUNK_PASSWORD=<password>' --name splunk_forwarder --hostname splunk_forwarder -v /var/log:/var/log/HOST splunk/universalforwarder:latest
```
## Portainer Set Up

1. Open up Portainer at http://localhost:9000 
2. Go to containers, and click on the splunk container
3. Open up the command shell and cd /opt/splunkforwarder/bin
4. Run the Following Commands
```	 
sudo ./splunk start && \
sudo ./splunk add forward-server SPLUNKHOSTIP:9997 && \
sudo ./splunk add monitor /var/log && \
sudo ./splunk restart
```
5. From here your splunk forwarder should be sending log files to the Host machine you have previously installed your Splunk Instance on, which is bound to the container ports
6. It will also be important to check the permissions on the log files, if you want to make 100% sure that everything will be readable run the following command.
```
sudo chmod -R 755 /var/log/HOST
```
This will change the permissions to allow everything in the directory and all it's children to be able to be readable and writable.  

## CLI Set Up

1. Run the Following Commands
```
sudo docker exec -it splunk_forwarder /bin/bash && \	
sudo ./splunk start && \
sudo ./splunk add forward-server SPLUNKHOSTIP:9997 && \
sudo ./splunk add monitor /var/log && \
sudo ./splunk restart
```
2. From here your splunk forwarder should be sending log files to the Host machine you have previously installed your Splunk Instance on, which is bound to the container ports.
3. It will also be important to check the permissions on the log files, if you want to make 100% sure that everything will be readable run the following command.
```
sudo chmod -R 755 /var/log/HOST
```
This will change the permissions to allow everything in the directory and all it's children to be able to be readable and writable.  

## Splunk Forwarder Set Up Without Docker

Note: Use this set up on other hosts to send logs to your main Splunk instance!  

1. Download splunk universal forwarder from the following link, use .rpm for CentOS and .deb for Ubuntu. Alternatively you can download the .tar and unzip and copy files to /opt.<br/> https://www.splunk.com/en_us/download/universal-forwarder.html
2. Once you have the universal forwarder installed
```
cd /opt/splunkforwarder
```
3. Now run the following commands 
```
sudo ./splunk start && \
sudo ./splunk add forward-server SPLUNKHOSTIP:9997 && \
sudo ./splunk add monitor /var/log && \
sudo ./splunk restart
```

