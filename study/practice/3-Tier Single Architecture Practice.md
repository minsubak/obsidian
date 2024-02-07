#NCP #도커 #실습
# 0 intro
3 계층 구조 + nginx를 단일 형태로 구현하는 실습입니다. 실습에 필요한 환경 구성은 `실습 메뉴얼.md` 를 참고해주시고, 내용에 오류가 존재할 경우 알려주세요.

해당 강좌는 centOS 7 환경에서 진행합니다. Cloud 환경에서 진행할 경우, ACG와 Proxy설정에 유의해주세요. docker를 사용할 경우, 컨테이너 이름에 대한 설정에 유의하고, docker-compose 사용을 권장합니다.

*NCP의 경우 4~5(niginx 사용 여부)개의 서버를 사용하며, `ssh`명령어를 통해 접근합니다.*
*docker의 경우 4개의 컨테이너를 사용하며, `docker exec -it [container] bash`명령어를 통해 접근합니다.*

+ last udpate: 2024/02/07
+ wirte & edit by [@minsubak](https://github.com/minsubak) [@DicafriO](https://github.com/DicafriO)

-------------------------------------------------------------------------------
# 1 nginx
```bash
vim /etc/yum.repo.d/nginx.repo
yum install nginx -y
# install nginx

systemctl enable nginx
systemctl start nginx
systemctl status nginx
# systemctl work

cd /etc/nginx/
mkdir [domain]
vim conf.d/[domain].conf
# proxy work, [domain]: your domain
# if you are using ssl, you must change port 80 to 443 
# and clear commnet of the ssl script and redirection

systemctl restart nginx
# restart nginx

curl localhost
# test, result: nginx install success page
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
# server { redirection
#    listen [port];
#    server_name www.[domain] [domain];
#    return 301 [http | https]://$server_name$request_uri;
#}
#
#server { # redirection
#    listen [port];
#    server_name admin.[domain];
#    return 301 [http | https]://$server_name$request_uri;
#}
#
#server { # redirection
#    listen [port];
#    server_name pay.[domain];
#    return 301 [http | https]://$server_name$request_uri;
#}

upstream admin { # web LB
        server [web address]:80 max_fails=3 fail_timeout=30s;
}

server { # sending upstream to admin
        listen 80;
        #ssl on;
        server_name admin.[domain] [domain];
        #ssl_certificate /etc/nginx/[domain]/fullchain.pem;
        #ssl_certificate_key /etc/nginx/[domain]/privkey.pem;
        location / {
                proxy_set_header X-Forwarded-For $remote_addr;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header Host $http_host;
                proxy_pass http://admin;
        }
}

upstream www { # web LB
        server [web address]:80 max_fails=3 fail_timeout=30s;
}

server { # sending upstrem to www
        listen 80;
        #ssl on;
        server_name www.[domain];
        #ssl_certificate /etc/nginx/[domain]/fullchain.pem;
        #ssl_certificate_key /etc/nginx/[domain]/privkey.pem;
        location / {
                proxy_set_header X-Forwarded-For $remote_addr;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header Host $http_host;
                proxy_pass http://www;
        }
}

upstream pay { # web LB
        server [web address]:80 max_fails=3 fail_timeout=30s;
}

server { # sending upstream to pay
        listen 80;
        #ssl on;
        server_name pay.[domain];
        #ssl_certificate /etc/nginx/[domain]/fullchain.pem;
	    #ssl_certificate_key /etc/nginx/[domain]/privkey.pem;
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
```bash
yum install httpd -y
# install apache httpd

vim /var/www/html/index.html
# create and wirte test page

cd /etc/httpd/conf.d
vim [domain].conf
# proxy work

vim ../conf/httpd.conf
# default page edit and edit proxy setting

systemctl enable httpd
systemctl start httpd
systemctl status httpd
# systemctl work
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
        <p>3-Tier Architecture + nginx.</p>
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
  ProxyPass / "http://[was address]:8080/"
  ProxyPassReverse / "http://[was address]:8080/"
</VirtualHost>

<VirtualHost *:80>
  ServerName pay.[domain]
  ProxyRequests Off
  ProxyPreserveHost On
  ProxyPass / "http://[was address]:8080/sample/"
  ProxyPassReverse / "http://[was address]:8080/sample/"
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
# 3 tomcat (WAS)

```bash
yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel -y
yum install java-11-openjdk.x86_64 java-11-openjdk-devel.x86_64 -y
# install jdk package

cd /usr/local/src/
wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.98/bin/apache-tomcat-8.5.98.tar.gz
tar xvzf apache-tomcat-8.5.98.tar.gz
mv apache-tomcat-8.5.98 tomcat
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
# apply mysql connect to tomcat and etc

mkdir -p webapps/sample/WEB-INF
cd webapps/sample
vim WEB-INF/web.xml
vim index.jsp
vim mysql_data.jsp
# add web application deployment descripter and web pages
```

tomcat.service
```service
[Unit]
Description=tomcat 8.5
After=network.target syslog.target

[Service]
Type=forking
Environment="/usr/local/src/tomcat"
User=tomcat
Group=tomcat
ExecStart=/usr/local/src/tomcat/bin/startup.sh
ExecStop=/usr/local/src/tomcat/bin/shutdown.sh
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
              url="jdbc:mysql://[db address]"
              maxTotal="50"
              maxIdle="20"
              maxWaitMillis="20000"/>
    <!-- ... ... ...-->
</Context>
```

server.xml
```xml
...
    <Connector port="8080" 
               protocol="HTTP/1.1"
               redirectPort="8443" 
               minSpareThreads="128"
               connectionTimeout="30000"
               maxThreads="2048" />
<!-- A "Connector" using the shared thread pool-->
...
...
      <Host>
        ...
        <!-- <Host></Host> 내에 <Context> 삽입 -->
        <Context docBase="/usr/local/src/tomcat/webapps/sample" path="/sample" reloadable="false" />
      </Host>
...
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
    <title>Practice 3-Tier Acrhitecture + nginx</title>
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
        <p>Practice 3-Tier Acrhitecture + nginx</p>
        <a href="mysql_data.jsp" class="btn">Activated</a>
    </div>

</body>
</html>
```

mysql_data.jsp
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
	String DB_URL = "jdbc:mysql://[db address]:3306/world";
	String DB_USER = "[db username]";
	String DB_PASSWORD= "[db password]]";
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

-------------------------------------------------------------------------------
# 4 mysql (DB)

```bash
cd /tmp
# architecture independent version route: 8.0.36
wget https://dev.mysql.com/get/mysql80-community-release-el7-11.noarch.rpm
rpm -Uvh mysql80-community-release-el7-11.noarch.rpm
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
yum install mysql-community-server -y

# general availability version route: 8.3.0
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
# independent version can install independently, and only can download latest:v8.0.36.
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