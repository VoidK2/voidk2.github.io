---
layout:  post
title: 在centos上部署web项目
subtitle: tomcat+java
date: 2018-12-11
author: zhangzexin
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - python
---

## 0x00 安装Tomcat
```
mkdir web
cd ./web
wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-9/v9.0.13/bin/apache-tomcat-9.0.13.tar.gz
tar -zxvf apache-tomcat-9.0.13.tar.gz
```
## 0x01 安装JDK
```
yum install java-1.8.0-openjdk
```
## 0x02 安装mysql
```
wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server
service mysqld restart
mysql -u root
```
## 0x03 配置mysql
```
show databases;
set password for 'root'@'localhost' =password('passwd');
use mysql;
grant all privileges on *.* to 'root'@'%' identified by 'passwd';
flush privileges;
```
## 0x04 war包部署到Tomcat
- 将war包改名成ROOT.war放在webapps文件夹下，原root文件夹改名备份
- 在tomcat的bin文件夹下执行./startup.sh
