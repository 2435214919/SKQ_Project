mysql数据库部署
	在centos上安装mysql8
		进入local目录中，运行下载命令
		wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
		安装mysql源
		yum localinstall mysql80-community-release-el7-1.noarch.rpm
		检查是否安装成功
		yum repolist enabled | grep "mysql.*-community.*"
		安装mysql
		yum install mysql-community-server
		启动mysql
		systemctl start mysqld
		查看启动状态
		systemctl status mysqld
	修改root用户密码
		得到mysql登录密码
		grep 'temporary password' /var/log/mysqld.log
		服务器内登录mysql
		mysql -u root -p
		修改密码
		set password for 'root'@'localhost'=password('Skq123456!');
	创建本系统使用的数据库'myappdb'
		creact database myappdb;
	添加可以远程访问mysql的新用户
		创建用户'shenkunqi'密码为'Skq123456!'
		create user 'shengkunqi'@'%' identified by 'Skq123456!';
		给予用户'shenkunqi'访问'myappdb'数据库的权限
		grant all privileges on myappdb.* to 'shenkunqi'@'%' with grant option;
	在本地windows10上使用Navicat远程登录mysql并使用
		设置主机: 106.12.42.105 
		设置端口: 3306
		输入用户名: shenkunqi
		输入密码: Skq123456!

服务端源代码部署
	在centos上安装jdk8
		下载、解压安装jdk
		wget http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz
		tar xvf jdk-8u161-linux-x64.tar.gz -C /usr/local/
	在centos上部署Tomcat9
		下载、解压安装tomcat
		wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.31/bin/apache-tomcat-9.0.31.tar.gz
		tar xvf apache-tomcat-9.0.31.tar.gz -C /usr/local/
		mv /usr/local/apache-tomcat-9.0.31/ /usr/local/tomcat/
	修改Tomcat环境变量
		使用vim修改startup.sh和shutdown.sh两个脚本文件，添加以下内容
		export JAVA_HOME=/usr/local/java
		export TOMCAT_HOME=/usr/local/tomcat
		export CATALINA_HOME=/usr/local/tomcat
		export CLASS_PATH=$JAVA_HOME/bin/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tool.jar
		export PATH=$PATH:/usr/local/java/bin:/usr/local/tomcat/bin	
	使用Intellij IDEA 创建JavaWeb项目
		选择sdk应与服务器端保持一致
		选择服务器为Tomcat9
		在'Additional Libraries and Frameworks:'勾选'WebApplication'
		在WEB-INF 目录下点击右键，New --> Directory，创建 classes 和 lib 两个目录
		File --> Project Structure...，进入 Project Structure窗口，点击 Modules --> 选中项目“JavaWeb” --> 切换到 Paths 选项卡 --> 勾选 “Use module compile output path”，将 “Output path” 和 “Test output path” 都改为之前创建的classes目录
		点击 Modules --> 选中项目“JavaWeb” --> 切换到 Dependencies 选项卡 --> 点击右边的“+”，选择 “JARs or directories...”，选择创建的lib目录
	通过FileZilla将编译好的JavaWeb文件上传到Tomcat上
	运行'startup.sh'脚本启动'Tomcat'服务器

安卓客户端源代码部署
	使用 Android Studio 创建新 Project
		选择平台 Phone and Tablet
		选择 Empty Activity
		选择语言为 Java
		选择最低SDK版本为 API 22
	在项目的根目录下build.gradle中配置阿里云镜像
		maven { url 'http://maven.aliyun.com/nexus/content/groups/public/'}
		maven { url'https://maven.aliyun.com/repository/public/' }
		maven { url'https://maven.aliyun.com/repository/google/' }
		maven { url'https://maven.aliyun.com/repository/jcenter/' }
		maven { url'https://maven.aliyun.com/repository/central/' }
	在app目录下build.gradle中配置要用到的库
		gson库
		implementation 'com.google.code.gson:gson:2.8.5'
		okhttp3库
   		implementation 'com.squareup.okhttp3:okhttp:4.5.0'
	源代码编写完成后发布安装文件
		选择Build-->Generate Signed Bundle or APK
	在Android系统的手机上安装apk文件并运行程序

数据库用户及密码
	用户名: shenkunqi
	密码: Skq123456!
