# Tomcat-CLuster-Setup
This will give you a step by step approach towards creating a cluster enviorment fromn your tomcat server

Apache Http Server
Download the Apache Http Server 2.4 or 2.2 by using the below link
Apache 2.4 is available under 
https://httpd.apache.org/download.cgi

 
For other previous versions like 2.2 available under 
https://archive.apache.org/dist/httpd/#releases 
 
after downloading extracts it to C:/Apache
 
Add mod_jk.so
Add mod_jk.so file to the modules folder  under  C:\Apache\modules
Download the required 32bit or 64bit mod_jk.so file from the following links
If your apache is a 2.2 version, choose this:
•	http://archive.apache.org/dist/tomcat/tomcat-connectors/jk/binaries/windows/tomcat-connectors-1.2.40-windows-i386-httpd-2.2.x.zip
If it is a 2.4, choose one of them depending on if you prefer 64 or 32-bit version:
•	http://archive.apache.org/dist/tomcat/tomcat-connectors/jk/binaries/windows/tomcat-connectors-1.2.40-windows-i386-httpd-2.4.x.zip
•	http://archive.apache.org/dist/tomcat/tomcat-connectors/jk/binaries/windows/tomcat-connectors-1.2.40-windows-x86_64-httpd-2.4.x.zip

Modify httpd.conf
Add the required configuartions parameters to the httpd.conf file , which available under C:\Apache\conf\
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/workers.properties
#JkShmFile logs/mod_jk.shm
JkLogLevel info
JkLogFile logs/mod_jk.log
JkMount /Manager_GTW/* lb

<Location /jkmanager/>
JkMount jkstatus
Order deny, allow
Deny from all
Allow from 127.0.0.1
</Location>
Download Tomcat server
Now we have to create two web servers in our local system try to download the Tomcat8.5 or Tomcat 9.0 from the below links.
For Tomcat9.0.x
https://tomcat.apache.org/download-90.cgi#9.0.17
For Tomcat 8.5.x
https://tomcat.apache.org/download-80.cgi#8.5.39
After downloading extract it to required folder and copy paste the same into another name, Here we named it as cluster1 and cluster2
 
 
 
Modify server.xml
Now modify the server.xml file under 
cluster1\conf\server.xml  and 

cluster2\conf\server.xml

Try to make sure that you have a different port number for Server in both clusters

cluster1\conf\server.xml  


 

cluster2\conf\server.xml
 

Try to make sure that you have different port number for HTTP Connectors in both clusters

cluster1\conf\server.xml  

 

cluster2\conf\server.xml

 

And also for AJP 1.3 Connector ports as well

cluster1\conf\server.xml  

 

Cluster2\conf\server.xml  

 


After this add the “jvmRoute” attribute to the Engine tag in the server.xml
cluster1\conf\server.xml
<Engine name="Catalina" defaultHost="localhost" jvmRoute="worker1">
 

Cluster2\conf\server.xml
<Engine name="Catalina" defaultHost="localhost" jvmRoute="worker2">
 








Add workers.properties
Now move Apache HTTP server
Create workers.properties file under the conf folder, like C:\Apache\conf\ workers.properties
Note: Try to verify the worker1 and worker2 ajp1.3 port numbers from the server.xml files from both culster1 nad cluster2
#Set properties for worker19 (ajp13)
worker.list=lb

worker.worker1.host=localhost
worker.worker1.port=8009
worker.worker1.type=ajp13
#worker.worker1.lbfactor=1

worker.worker2.host=localhost
worker.worker2.port=8109
worker.worker2.type=ajp13

worker.lb.type=lb
worker.lb.balance_workers=worker1,worker2

# status worker
worker.list=jkstatus
worker.jkstatus.type=status

# Associate real workers with virtual LoadBalancer worker

worker.jkstatus.balance_workers=worker1




Start clusters
Now start both tomcat servers.
Before starting the servers goto “catalina.bat” under bin folder of each server

set CATALINA_HOME and CATALINA_BASE  inside the catalina.bat as mentioned.

Note: Try to set these variables at the starting, because we need to override with the existing environment variables if any.

E:\cluster_apache\cluster1\bin\ catalina.bat 

 set "CATALINA_HOME=E:\cluster_apache\cluster1"
set "CATALINA_BASE=E:\cluster_apache\cluster1"

 

E:\cluster_apache\cluster2\bin\ catalina.bat

set "CATALINA_HOME=E:\cluster_apache\cluster2"
set "CATALINA_BASE=E:\cluster_apache\cluster2"
 

Start the tomcat server from the cmd (cluster1)
catalina.bat run
 
 

Start the tomcat server from the cmd (cluster2)
catalina.bat run

 
 

