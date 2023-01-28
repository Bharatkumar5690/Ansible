sudo apt update
sudo apt install openjdk-11-jdk -y
java -version
sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
VERSION=10.1.4
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.71/bin/apache-tomcat-9.0.71.tar.gz -P /tmp
sudo tar -xf apache-tomcat-9.0.71.tar.gz -C /opt/tomcat/
sudo ln -s /opt/tomcat/apache-tomcat-9.0.71 /opt/tomcat/latest
sudo chown -R tomcat: /opt/tomcat
sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'
sudo nano /etc/systemd/system/tomcat.service

'''
[Unit]
Description=Tomcat 10 servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"

Environment="CATALINA_BASE=/opt/tomcat/latest"
Environment="CATALINA_HOME=/opt/tomcat/latest"
Environment="CATALINA_PID=/opt/tomcat/latest/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/latest/bin/startup.sh
ExecStop=/opt/tomcat/latest/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
'''

sudo systemctl daemon-reload
sudo systemctl enable --now tomcat
sudo systemctl status tomcat
sudo nano /opt/tomcat/latest/conf/tomcat-users.xml

'''
<tomcat-users>
<!--
    Comments
-->
   <role rolename="admin-gui"/>
   <role rolename="manager-gui"/>
   <user username="admin" password="admin_password" roles="admin-gui,manager-gui"/>
</tomcat-users>
'''

sudo nano /opt/tomcat/latest/webapps/manager/META-INF/context.xml

'''
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
'''

sudo nano /opt/tomcat/latest/webapps/host-manager/META-INF/context.xml

'''
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
'''

sudo systemctl restart tomcat