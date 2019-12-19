# Logs 300 - Use Case

1. Alert for more than 5 failed Login attempts on a single account in 24 hours, as well as ssh login attempts for a user per hour, and ANY login attempts over ssh for root.
2. SSH connections by IP, geolocation map to monitor where IP addresses are located.
3. Commands and processes run by the root account on the machine, monitoring of changes to system.

# Explaination of Use Case


To monitor these attempts on a CentOS host I will be monitoring the /var/log/secure log. On an Ubuntu host I will monitor the /var/log/auth.log file.  <br/><br/>

This use case could help a business in multiple ways. First, we are monitoring SSH connections into the system. This is helpful so that we know if someone is trying to access our system from an un approved location. I created a dashboard with connections by IP, and a geolocation map which would be useful for an analyst to easily monitor where the connections to the server are coming from. I could probably edit this to monitor more than just ssh connections, like remote desktop and vpn connections. Also, it would be possible to use this same search information on a log file for a web server to be able to see where in the world your website is getting hits from.  <br/><br/>

Second, I monitored what commands and processes were being run by the root user account on the machine. This is very useful for a businiess because we can see what someone was doing within a specified time period. This can aid a business in not only realizing something is going on, but also doing forensics of a breach once a time range has been discovered. They would be able to see things like privilige escalations, changing permissions, downloading a file, and executing files. Overall, this simple and easy to set up SIEM would be useful for any company that has a need to monitor who is connecting to their servers. I also set a dashboard panel to monitor the commands run by each user, which would be useful to track workflow and also help troubleshoot if a system ran into errors during some type of maintainence or task.   
