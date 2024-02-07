#도커 #실습
# 0 intro
3 계층 구조 이중화를 구현하는 실습입니다. proxy를 설정하고, 각 프로그램의 conf 파일에 대한 이해가 필요합니다. docker container는 7개만 사용합니다. 실습에 필요한 구성 방법은 `실습 메뉴얼.md` 를 참고해 주세요. 또, 내용에 오류가 존재할 경우 알려주세요.

해당 교육은 centOS 7의 Docker 학습을 진행합니다. Cloud 환경에서 테스트를 진행할 경우, ACG와 IP 전달 설정(방화벽, 보안)에 유의해주세요.

* last update: 2024/02/07
* write by [@minsubak](https://github.com/minsubak)
------------------------------------------------------------------------------------
# 1 nginx (external LB)
```bash
docekr exec -it [external lb container] bash
# entering container

vim /etc/yum.repos.d/nginx.repo
yum install nginx -y
# install nginx

systemctl enable nginx
systemctl start nginx
systemctl status nginx
# systemctl work

cd /etc/nginx/
mkdir [domain] # ssl key directory
vim conf.d/[domain].conf
# proxy work

systemctl restart nginx
# restart nginx

curl localhost
# test, result: nignx install success page

exit
# leaving container
```

nginx.repo
```repo
[nginx]
name=Nginx Repository \$basearch - Archive
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
enabled=1
gpgcheck=0
gpgkey=https://nginx.org/keys/nginx_signing.key
```

[domain].conf
```conf
upstream admin { # WEB LB
       server [web container1]:80 max_fails=3 fail_timeout=30s;
       server [web container2]:80 max_fails=3 fail_timeout=30s;
}

server { # sendine upstream to admin
        listen 80;
        server_name admin.[domain] [domain];
        location / {
                   proxy_set_header X-Forwarded-For $remote_addr;
                   proxy_set_header X-Forwarded-Proto $scheme;
                   proxy_set_header Host $http_host;
                   proxy_pass http://admin;
        }
}

upstream www { # WEB LB
       server [web container1]:80 max_fails=3 fail_timeout=30s;
       server [web container2]:80 max_fails=3 fail_timeout=30s;
}

server { # sending upstream to www
        listen 80;
        server_name www.[domain];
        location / {
                   proxy_set_header X-Forwarded-For $remote_addr;
                   proxy_set_header X-Forwarded-Proto $scheme;
                   proxy_set_header Host $http_host;
                   proxy_pass http://www;
        }
}

upstream pay { # WEB LB
       server [web container1]:80 max_fails=3 fail_timeout=30s;
       server [web container2]:80 max_fails=3 fail_timeout=30s;
}

server { # sending upstream to pay
        listen 80;
        server_name pay.[domain];
        location / {
                   proxy_set_header X-Forwarded-For $remote_addr;
                   proxy_set_header X-Forwarded-Proto $scheme;
                   proxy_set_header Host $http_host;
                   proxy_pass http://pay;
        }
}
```

-------------------------------------------------------------------------------

# 2 httpd (WEB)
web container 두 개 모두 동일하게 설정해야 한다.
```bash
docker exec -it [web container] bash
# entering container

yum install httpd -y
# install apache httpd

vim /var/www/html/index.html
# create and write test page

cd /etc/httpd/conf.d
vim [domain].conf
# proxy work

vim /etc/httpd/conf/httpd.conf
# default page edit and edit proxy setting

systemctl enable httpd
systemctl start httpd
systemctl status httpd
# systemctl work

exit
# leaving container
```

index.html
```html
<!-- 본인이 웹에서 출력하고 싶은 내용 작성 ex) Hi httpd -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to My Website</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            text-align: center;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
        }
        p {
            color: #666;
            margin-bottom: 30px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to My Website by Appache httpd</h1>
        <p>3-Tier Architecture Dualization</p>
    </div>
</body>
</html>
```

[domain].conf
```conf
<VirtualHost *:80>
  ServerName www.[domain]
</VirtualHost>

<VirtualHost *:80>
  ServerName admin.[domain]
  ProxyRequests Off
  ProxyPreserveHost On
  ProxyPass / "http://[internal lb container]/"
  ProxyPassReverse / "http://[internal lb container]/"
</VirtualHost>

<VirtualHost *:80>
  ServerName pay.[domain]
  ProxyRequests Off
  ProxyPreserveHost On
  ProxyPass / "http://[internal lb container]/sample/"
  ProxyPassReverse / "http:/[internal lb container]/sample/"
</VirtualHost>
```

httpd.conf
```conf
...
# Example:
# LoadModule foo_module modules/mod_foo.so
# 아래의 LoadModule 3개와 include 추가 (default는 알아서 로드하기에 패스해도 무방)

LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule proxy_wstunnel_module modules/mod_proxy_wstunnel.so
Include conf.d/[domain].conf
Include conf.modules.d/*.conf
...
...
# If your host doesn't have a registered DNS name, enter its IP address here.
#
ServerName www.[domain]:80
...
...
# Deny access to the entirety of your server's filesystem. You must
# explicitly permit access to web content directories in other 
# <Directory> blocks below.
#
<Directory />
    AllowOverride none
    Require all granted
</Directory>
...
```
-------------------------------------------------------------------------------
# 3 nginx (internal LB)
```bash
docekr exec -it [internal lb container] bash
# entering container

vim /etc/yum.repos.d/nginx.repo
yum install nginx -y
# install nginx

systemctl enable nginx
systemctl start nginx
systemctl status nginx
# systemctl work

cd /etc/nginx/
mkdir [domain] # ssl key directory
vim conf.d/[domain]
# proxy work

systemctl restart nginx
# restart nginx

curl localhost
# test, result: nignx install success page

exit
# leaving container
```

nginx.repo
```repo
[nginx]
name=Nginx Repository \$basearch - Archive
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
enabled=1
gpgcheck=0
gpgkey=https://nginx.org/keys/nginx_signing.key
```

[domain].conf
```conf
upstream pay { # WAS LB
       ip_hash;
       server [was container1]:8080 max_fails=3 fail_timeout=30s;
       server [was container2]:8080 max_fails=3 fail_timeout=30s;
}

server { # sending upstream to pay
        listen 80;
        server_name pay.[domain];
        location / {
                   proxy_set_header X-Forwarded-For $remote_addr;
                   proxy_set_header X-Forwarded-Proto $scheme;
                   proxy_set_header Host $http_host;
				   proxy_pass http://pay;
        }
}

upstream admin { # WAS LB
       ip_hash;
       server [was container1]:8080 max_fails=3 fail_timeout=30s;
       server [was container2]:8080 max_fails=3 fail_timeout=30s;
}

server {
        listen 80;
        server_name admin.[domain] [domain];
        location / {
                   proxy_set_header X-Forwarded-For $remote_addr;
                   proxy_set_header X-Forwarded-Proto $scheme;
                   proxy_set_header Host $http_host;
				   proxy_pass http://admin;
        }
}
```
------------------------------------------------------------------------------------
# 4 tomcat (WAS)
was container 두 개 모두 설정해야 한다.
```bash
docker exec -it [was container] /bin/bash
# entering container

yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel -y
yum install java-11-openjdk.x86_64 java-11-openjdk-devel.x86_64 -y
# install jdk package / only one choice jdk

cd /usr/local/src/
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.85/bin/apache-tomcat-9.0.85.tar.gz
tar xvzf apache-tomcat-9.0.85.tar.gz
mv apache-tomcat-9.0.85 tomcat
# install tomcat in /usr/local/src/

useradd -M tomcat
chown tomcat:tomcat -R tomcat/
# create new user `tomcat` and change acess of directory

cd /etc/systemd/system
vim tomcat.service
# add tomcat.service to systemctl

systemctl daemon-reload
systemctl enable tomcat
systemctl start tomcat
systemctl status tomcat
# systemctl work

curl localhost:8080
# test, result: tomcat index page

cd /tmp
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.3.0-1.el7.noarch.rpm
rpm -Uvh mysql-connector-j-8.3.0-1.el7.noarch.rpm
cp /usr/share/java/mysql-connector-j.jar /usr/local/src/tomcat/lib
chown tomcat:tomcat /usr/local/src/tomcat/lib/mysql-connector-j.jar
# install mysql connector to tomcat

cd /usr/local/src/tomcat/
vim conf/context.xml
vim conf/server.xml
# apply mysql connect to tomcat and WAS clustering
# the server.xml files for [was container]s are different

mkdir -p webapps/sample/WEB-INF
cd webapps/sample
vim WEB-INF/web.xml
vim index.jsp
vim mysql_data.jsp
# add web application deployment descripter and web pages

exit
# leaving container
```

tomcat.service
```service
[Unit]
Description=tomcat 9
After=network.target syslog.target

[Service]
Type=forking
Environment="/usr/local/src/tomcat"
User=tomcat
Group=tomcat
ExecStart= /usr/local/src/tomcat/bin/startup.sh
ExecStop= /usr/local/src/tomcat/bin/shutdown.sh
RestartSec=10
Restart=always
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```

context.xml
```xml
<Context>
	...
    <!-- 아래의 기능을 해당 위치에 추가 -->
    <Resource name="jdbc/mysql"
              auth="Container"
              type="javax.sql.DataSource"
              username="[db username]"
              password="[db password]"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://[db container]"
              maxTotal="50"
              maxIdle="20"
              maxWaitMillis="20000"/>
</Context>
```

server.xml (was container1)
```xml
        <!-- clustering -->
        <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" channelSendOptions="8"  channelStartOptions="3">
                <Manager className="org.apache.catalina.ha.session.DeltaManager" expireSessionsOnShutdown="false" notifyListenersOnReplication="true"/>
                <Channel className="org.apache.catalina.tribes.group.GroupChannel">
                        <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
                                <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender" />
                        </Sender>
                        <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
                        address="[was container1]"
                        port="4055"
                        autoBind="0"
                        selectorTimeout="5000"
                        maxThreads="6"/>

                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpPingInterceptor" staticOnly="true"/>
                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector" />
                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.StaticMembershipInterceptor">
                        <Member
                                className="org.apache.catalina.tribes.membership.StaticMember"
                                port="4055"
                                host="[was container2]"
                                uniqueId="{0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,2}"
                        />
                        </Interceptor>
                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatchInterceptor"/>
                </Channel>

                <Valve className="org.apache.catalina.ha.tcp.ReplicationValve" filter=".*\.gif;.*\.js;.*\.jpg;.*\.png;.*\.htm;.*\.html;.*\.css;.*\.txt;" />
                <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
                <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener" />
        </Cluster>
        <!-- clustering  -->
```

server.xml (was container2)
```xml
        <!-- clustering -->
        <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" channelSendOptions="8"  channelStartOptions="3">
                <Manager className="org.apache.catalina.ha.session.DeltaManager" expireSessionsOnShutdown="false" notifyListenersOnReplication="true"/>
                <Channel className="org.apache.catalina.tribes.group.GroupChannel">
                        <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
                                <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender" />
                        </Sender>
                        <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
                        address="[was container2]"
                        port="4055"
                        autoBind="0"
                        selectorTimeout="5000"
                        maxThreads="6"/>

                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpPingInterceptor" staticOnly="true"/>
                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector" />
                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.StaticMembershipInterceptor">
                        <Member
                                className="org.apache.catalina.tribes.membership.StaticMember"
                                port="4055"
                                host="[was container1]"
                                uniqueId="{0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,1}"
                        />
                        </Interceptor>
                        <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatchInterceptor"/>
                </Channel>

                <Valve className="org.apache.catalina.ha.tcp.ReplicationValve" filter=".*\.gif;.*\.js;.*\.jpg;.*\.png;.*\.htm;.*\.html;.*\.css;.*\.txt;" />
                <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
                <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener" />
        </Cluster>
        <!-- clustering  -->
```

web.xml
```xml
<web-app>
<distributable/>
</web-app>
```

index.jsp
```jsp
<!-- 본인이 웹에서 출력하고 싶은 내용 작성 ex) Hi tomcat -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Practice 3-Tier Acrhitecture Dualization</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            text-align: center;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
        }
        p {
            color: #666;
            margin-bottom: 30px;
        }
        a {
            text-decoration: none;
            color: #fff;
            background-color: #007bff;
            padding: 10px 20px;
            border-radius: 5px;
            transition: background-color 0.3s ease;
        }
        a:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to My Website by Apache tomcat</h1>
        <p>Practice 3-Tier Dualization</p>
        <a href="mysql_data.jsp" class="btn">Activated</a>
    </div>

</body>
</html>
```

mysql_data_jsp
```jsp
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.sql.*"%>
<%@ page import="javax.sql.*"%>
<%@ page import="javax.naming.*"%>
<%
Connection conn=null;
try{
	Context init = new InitialContext();
	DataSource ds = (DataSource) init.lookup("java:comp/env/jdbc/mysql");
	conn = ds.getConnection();
	out.println("JNDI Connection Success!!!<br>");
	Statement stmt = conn.createStatement();
	ResultSet rs = stmt.executeQuery("SELECT * FROM world.member");
	out.println("JNDI Query Result:<br>");
	while (rs.next()) {
		out.println(rs.getString(1) + " " + rs.getString(2) + " " + rs.getString(3) + "<br>");
	}
	rs.close();
	stmt.close();
	}
	catch(Exception e){
	out.println("JNDI Connection Failure!!!<br>");
	e.printStackTrace();
	}

try {
	String DB_URL = "jdbc:mysql://[db container]:3306/world";
	String DB_USER = "[db username]";
	String DB_PASSWORD= "[db password]";
	Class.forName("com.mysql.cj.jdbc.Driver");
	conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
	out.println("<br>DriverManager Connection Success!!!<br>");
	Statement stmt = conn.createStatement();
	ResultSet rs = stmt.executeQuery("SELECT * FROM world.member");
	out.println("DriverManager Query Result:<br>");
	while (rs.next()) {
		out.println(rs.getString(1) + " " + rs.getString(2) + " " + rs.getString(3) + "<br>");
	}
	rs.close();
	stmt.close();
}
catch(Exception e){
	out.println("DriverManager Connection Failure!!!<br>");
	out.println("Exception Error...");
	out.println(e.toString());
} finally {
	try {
		if (conn != null) conn.close();
	} catch (SQLException e) {
		out.println("Error closing connection: " + e.getMessage());
	}
}
%>
```
------------------------------------------------------------------------------------
# 5 MySQL (DB)
```bash
docker exec -it [db container] bash
# entering container

cd /tmp

# !!!only choice one route!!!

# route 1:: architecture independent version route: 8.0.36
wget https://dev.mysql.com/get/mysql80-community-release-el7-11.noarch.rpm
rpm -Uvh mysql80-community-release-el7-11.noarch.rpm
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
yum install mysql-community-server -y

# route 2:: general availability version route: 8.3.0
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-server-8.3.0-1.el7.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-client-8.3.0-1.el7.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-client-plugins-8.3.0-1.el7.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-common-8.3.0-1.el7.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-libs-8.3.0-1.el7.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-libs-compat-8.3.0-1.el7.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-8.3/mysql-community-icu-data-files-8.3.0-1.el7.x86_64.rpm
yum install libaio libaio-devel numactl-libs -y
rpm -Uvh mysql-community-icu-data-files-8.3.0-1.el7.x86_64.rpm
rpm -Uvh mysql-community-common-8.3.0-1.el7.x86_64.rpm
rpm -Uvh mysql-community-client-plugins-8.3.0-1.el7.x86_64.rpm
rpm -Uvh mysql-community-libs-8.3.0-1.el7.x86_64.rpm
rpm -Uvh mysql-community-libs-compat-8.3.0-1.el7.x86_64.rpm
rpm -Uvh mysql-community-client-8.3.0-1.el7.x86_64.rpm
rpm -Uvh mysql-community-server-8.3.0-1.el7.x86_64.rpm
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022

# explain:: 
# independent version can install independently, and only can download:v8.0.36.
# and alos need to check newer version update.
# but the others, need to solve dependence problem, and can download all version.

systemctl enable mysqld
systemctl start mysqld
systemctl status mysqld
# systemctl work

grep 'temporary password' /var/log/mysqld.log
mysql_secure_installation
# secure work
# root user password           > copied temp password
# temp password expired        > [db password]
# new password for root        > y, [db password]
# password provided            > y
# remvoe anonymous user        > n
# disallow root login remotely > y
# remove test db and access    > n
# privilege table reload now   > y

mysql -u root -p
# login mysql

exit
# leaving container
```

```mysql
create database world;
show databases;
use world;
# check db and create new db

create table member(no int NOT NULL, t_name varchar(20), content TEXT);
# create new table `member`

insert into member values('1', 'kkk', 'lee');
insert into member values('2', 'aaa', 'kim');
insert into member values('3', 'bbb', 'back');
insert into member values('4', 'ccc', 'boo');
insert into member values('5', 'ddd', 'hooo');
insert into member values('6', 'eee', 'ha');
# insert new data

select * from member;
# print all memeber table

create user '[db username]'@'%' identified by '[db password]';
GRANT ALL privileges ON world.* TO [db username]@'%';
# create new user `nsam` and apply access of world to nsam

exit
```