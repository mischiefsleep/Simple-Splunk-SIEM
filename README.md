# Simple Splunk SIEM

For my final project for Logs and Detection at SecureSet Academy I have built out a simple splunk SIEM. The architecture used was setting up the SIEM through containerization, so that we can deploy this product on any operating system that can run Docker. I accomplished this on two different Linux operating systems, CentOS 8, and Ubuntu 18.04. I constructed a use case that would be valuble for any business getting started that has a need to monitor connections to a server, or changes on a machine. 

# Lab

I also have written up a lab with instructions on how to install Docker, and then get the containers running. This lab is meant to get you up and running with a splunk container which has your log files from the host OS mounted to it for easy monitoring. There are also steps in the lab to set up a forwarder container on another OS, as well as steps to set up a forwarder without using docker in case you would rather just install the splunk universal forwarder to send the data to the splunk enterprise instance. 




