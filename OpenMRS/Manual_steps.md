## Ubuntu
[Refer here](https://www.linuxcloudvps.com/blog/how-to-install-openmrs-on-ubuntu-20-04/)
java
----
apt update
apt install openjdk-8-jdk -y
java -version

tomcat7
-------
groupadd tomcat
useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
wget https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.109/bin/apache-tomcat-7.0.109.tar.gz 
mkdir /opt/tomcat
tar -xvzf apache-tomcat-7.0.109.tar.gz -C /opt/tomcat/ --strip-components=1
cd /opt/tomcat
chgrp -R tomcat /opt/tomcat
chmod -R g+r conf
chmod g+x conf
chown -R tomcat webapps/ work/ temp/ logs/
nano /etc/systemd/system/tomcat.service

[Unit]
Description=Apache Tomcat Web Application Container
After=network.target
[Service]
Type=forking
Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment=’CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC’
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh
User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always
[Install]
WantedBy=multi-user.target

systemctl daemon-reload
systemctl start tomcat
systemctl status tomcat

Install OpenMRS
----------------
mkdir /var/lib/OpenMRS
chown -R tomcat:tomcat /var/lib/OpenMRS
wget https://sourceforge.net/projects/openmrs/files/releases/OpenMRS_Platform_2.5.0/openmrs.war
cp openmrs.war /opt/tomcat/webapps/
chown -R tomcat:tomcat /opt/tomcat/webapps/openmrs.war
