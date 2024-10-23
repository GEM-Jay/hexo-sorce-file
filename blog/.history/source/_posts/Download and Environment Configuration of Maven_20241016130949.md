---
title: Maven的下载与环境配置(Windows)
date: 2024-10-16 13:08:22
tags:
---

# Maven的下载与环境配置\(Windows\)


## 一、Maven的主要内容

![](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven01.png)

## 二、Maven的简介

Maven\['meven\]这个词可以翻译为"专家"内行"。作为Apache组织中的一个颇为成功的开源项目，Maven主要服务于基于java平台的项目构建，依赖管理和项目信息管理。

无论是小型的开源类库项目，还是大型的企业级应用;无论是传统的瀑布式开发，还是流行的敏捷开发，Maven都能大显身手。

本文将教你掌握Maven的安装以及配置方法。

## 三、Maven的下载

在maven的官网可以下载，点击跳转[maven官网下载页](https://maven.apache.org/download.cgi "mave下载地址")  
选择`Files->Link->apache-maven-3.x.x-bin.zip`点击下载即可。  
![maven下载页](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven02.png)  
下载解压后，文件目录结构如下：  
![maven文件目录](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven03.png)

## 四、Maven常用配置

！！！在配置之前请务必将JDK安装好。

### 1.环境变量配置

选择`此电脑（右键）--> 属性 \--> 高级系统设置`  
进入如下页面：  
![配置系统变量](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven04.png)  
新手注意这里分为用户变量和系统变量，对于一台电脑（系统）来说，你是使用它的用户，而同一台电脑可以被另外一个用户使用，所以当更换用户时，你在用户变量配置的环境就消失了，为了方便使用，我们一般将环境变量全部配置在系统变量下，如图所示：  
![用户变量和系统变量](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven05.png)  
点击新建后我们添加Maven的HOME（在后文解释为什么要建立HOME）  
![新建HOME](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven06.png)  
在变量名一栏中输入  
`MAVEN_HOME`  
在变量值中输入maven的解压的地址，我使用的是自己文件的解压地址，请你换成你自己的,获取解压地址的方法如下：  
![获取解压地址](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven07.jpg)  
如图复制了之后在变量值中填入  
`D:\Workspace\Maven\apache-maven-3.8.3-bin\apache-maven-3.8.3(替换成你自己的)`

1.  编辑Path  
    接下来编辑Path，配置Path的目的是，当在cmd输入指令的时候，系统会在Path中寻找你的指令是否存在，所以配置Path的步骤最为关键  
    ![配置Path](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven08.png)  
    在编辑界面中添加`%MAVEN_HOME%\bin`

* 注：其实前面的HOME不配置都可以，只要把在Path中的`%MAVEN_HOME%\bin`换成`变量值\bin`是一样的，只不过变量值的路径一般都很长，为了增加可读性，我们会建立一个HOME

2.  测试  
    win+r输入cmd回车  
    在cmd窗口输入`mvn -v`查看  
    显示如下即为安装成功：  
    ![测试](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven10.png)

### 2.修改配置文件

通常我们需要修改解压目录下`conf/settings.xml`文件，这样可以更好的适合我们的使用。

* 此处注意：所有的修改一定要在注释标签外面，不然修改无效。Maven很多标签都是给的例子，都是注释掉的。

**文末附上我的整个Settings.xml文件配置。**

![配置xml](https://img-blog.csdnimg.cn/20181229170024817)

 1.     本地仓库位置的修改  
    在标签内添加自己的本地位置路径

```none
 <!-- localRepository
  | The path to the local repository maven will use to store artifacts.
  |
  | Default: ${user.home}/.m2/repository
 <localRepository>/path/to/local/repo</localRepository>
 -->
<localRepository>D:\tools\repository</localRepository>
```

 2.     修改maven默认的JDK版本  
    在标签下添加一个标签，修改maven默认的JDK版本。

```none
<profile>     
    <id>JDK-1.8</id>       
    <activation>       
        <activeByDefault>true</activeByDefault>       
        <jdk>1.8</jdk>       
    </activation>       
    <properties>       
        <maven.compiler.source>1.8</maven.compiler.source>       
        <maven.compiler.target>1.8</maven.compiler.target>       
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>       
    </properties>       
</profile>
```

 3.     添加国内镜像源  
    添加标签下，添加国内镜像源，这样下载jar包速度很快。默认的中央仓库有时候甚至连接不通。一般使用阿里云镜像库即可。这里我就都加上了，Maven会默认从这几个开始下载，没有的话就会去中央仓库了。

```none
<!-- 阿里云仓库 -->
<mirror>
    <id>alimaven</id>
    <mirrorOf>central</mirrorOf>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>

<!-- 中央仓库1 -->
<mirror>
    <id>repo1</id>
    <mirrorOf>central</mirrorOf>
    <name>Human Readable Name for this Mirror.</name>
    <url>http://repo1.maven.org/maven2/</url>
</mirror>

<!-- 中央仓库2 -->
<mirror>
    <id>repo2</id>
    <mirrorOf>central</mirrorOf>
    <name>Human Readable Name for this Mirror.</name>
    <url>http://repo2.maven.org/maven2/</url>
</mirror>
```

## 五、常用IDE下配置Maven

目前常用的开发工具如idea，eclipse都自身集成了一个版本的Maven。但是通常我们使用自己已经配置好的Maven。

### IDEA下配置Maven

跟着图片选择  
![setting](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven11.png)  
然后  
![IDAE](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven12.png)  
1：此处修改为自己解压的Maven目录

2：勾选Override，修改为自己目录下的settings.xml目录

3：修改为自己的本地仓库地址，一般会自动识别。

![自动导包](https://cdn.jsdelivr.net/gh/GEM-Jay/images/Maven13.png)  
此处勾选，当修改pom文件时，Maven就能帮我们自动导包了。

### Eclipse下配置Maven

将eclipse使用的Maven修改为自己的。点击add后选择自己Maven的安装目录即可。添加好之后记得勾选。  
![](https://img-blog.csdnimg.cn/20181229170024964)  
将所有的settings修改为自己Maven目录下的conf/settings.xml.点击Update Settings按钮，下面的Local Respository会自动识别出来。  
![](https://img-blog.csdnimg.cn/201812291700252)

## 附：完整的Settings.xml文件

```none
<?xml version="1.0" encoding="UTF-8"?>

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

<!--
 | This is the configuration file for Maven. It can be specified at two levels:
 |
 |  1. User Level. This settings.xml file provides configuration for a single user,
 |                 and is normally provided in ${user.home}/.m2/settings.xml.
 |
 |                 NOTE: This location can be overridden with the CLI option:
 |
 |                 -s /path/to/user/settings.xml
 |
 |  2. Global Level. This settings.xml file provides configuration for all Maven
 |                 users on a machine (assuming they're all using the same Maven
 |                 installation). It's normally provided in
 |                 ${maven.conf}/settings.xml.
 |
 |                 NOTE: This location can be overridden with the CLI option:
 |
 |                 -gs /path/to/global/settings.xml
 |
 | The sections in this sample file are intended to give you a running start at
 | getting the most out of your Maven installation. Where appropriate, the default
 | values (values used when the setting is not specified) are provided.
 |
 |-->
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>D:\tools\repository</localRepository>

  <!-- interactiveMode
   | This will determine whether maven prompts you when it needs input. If set to false,
   | maven will use a sensible default value, perhaps based on some other setting, for
   | the parameter in question.
   |
   | Default: true
  <interactiveMode>true</interactiveMode>
  -->

  <!-- offline
   | Determines whether maven should attempt to connect to the network when executing a build.
   | This will have an effect on artifact downloads, artifact deployment, and others.
   |
   | Default: false
  <offline>false</offline>
  -->

  <!-- pluginGroups
   | This is a list of additional group identifiers that will be searched when resolving plugins by their prefix, i.e.
   | when invoking a command line like "mvn prefix:goal". Maven will automatically add the group identifiers
   | "org.apache.maven.plugins" and "org.codehaus.mojo" if these are not already contained in the list.
   |-->
  <pluginGroups>
    <!-- pluginGroup
     | Specifies a further group identifier to use for plugin lookup.
    <pluginGroup>com.your.plugins</pluginGroup>
    -->
  </pluginGroups>

  <!-- proxies
   | This is a list of proxies which can be used on this machine to connect to the network.
   | Unless otherwise specified (by system property or command-line switch), the first proxy
   | specification in this list marked as active will be used.
   |-->
  <proxies>
    <!-- proxy
     | Specification for one proxy, to be used in connecting to the network.
     |
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->
  </proxies>

  <!-- servers
   | This is a list of authentication profiles, keyed by the server-id used within the system.
   | Authentication profiles can be used whenever maven must make a connection to a remote server.
   |-->
  <servers>
    <!-- server
     | Specifies the authentication information to use when connecting to a particular server, identified by
     | a unique name within the system (referred to by the 'id' attribute below).
     |
     | NOTE: You should either specify username/password OR privateKey/passphrase, since these pairings are
     |       used together.
     |
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    -->

    <!-- Another sample, using keys to authenticate.
    <server>
      <id>siteServer</id>
      <privateKey>/path/to/private/key</privateKey>
      <passphrase>optional; leave empty if not used.</passphrase>
    </server>
    -->
  </servers>

  <!-- mirrors
   | This is a list of mirrors to be used in downloading artifacts from remote repositories.
   |
   | It works like this: a POM may declare a repository to use in resolving certain artifacts.
   | However, this repository may have problems with heavy traffic at times, so people have mirrored
   | it to several places.
   |
   | That repository definition will have a unique id, so we can create a mirror reference for that
   | repository, to be used as an alternate download site. The mirror site will be the preferred
   | server for that repository.
   |-->
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    <mirror>
      <id>maven-default-http-blocker</id>
      <mirrorOf>external:http:*</mirrorOf>
      <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
      <url>http://0.0.0.0/</url>
      <blocked>true</blocked>
    </mirror>
    <!-- 闃块噷浜戜粨搴?-->
    <mirror>
        <id>alimaven</id>
        <mirrorOf>central</mirrorOf>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>

    <!-- 涓ぎ浠撳簱1 -->
    <mirror>
        <id>repo1</id>
        <mirrorOf>central</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://repo1.maven.org/maven2/</url>
    </mirror>

    <!-- 涓ぎ浠撳簱2 -->
    <mirror>
        <id>repo2</id>
        <mirrorOf>central</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://repo2.maven.org/maven2/</url>
    </mirror>

  </mirrors>

  <!-- profiles
   | This is a list of profiles which can be activated in a variety of ways, and which can modify
   | the build process. Profiles provided in the settings.xml are intended to provide local machine-
   | specific paths and repository locations which allow the build to work in the local environment.
   |
   | For example, if you have an integration testing plugin - like cactus - that needs to know where
   | your Tomcat instance is installed, you can provide a variable here such that the variable is
   | dereferenced during the build process to configure the cactus plugin.
   |
   | As noted above, profiles can be activated in a variety of ways. One way - the activeProfiles
   | section of this document (settings.xml) - will be discussed later. Another way essentially
   | relies on the detection of a system property, either matching a particular value for the property,
   | or merely testing its existence. Profiles can also be activated by JDK version prefix, where a
   | value of '1.4' might activate a profile when the build is executed on a JDK version of '1.4.2_07'.
   | Finally, the list of active profiles can be specified directly from the command line.
   |
   | NOTE: For profiles defined in the settings.xml, you are restricted to specifying only artifact
   |       repositories, plugin repositories, and free-form properties to be used as configuration
   |       variables for plugins in the POM.
   |
   |-->
  <profiles>
    <profile>     
      <id>JDK-1.8</id>       
      <activation>       
          <activeByDefault>true</activeByDefault>       
          <jdk>1.8</jdk>       
      </activation>       
      <properties>       
          <maven.compiler.source>1.8</maven.compiler.source>       
          <maven.compiler.target>1.8</maven.compiler.target>       
          <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>       
      </properties>       
</profile>

    <!-- profile
     | Specifies a set of introductions to the build process, to be activated using one or more of the
     | mechanisms described above. For inheritance purposes, and to activate profiles via <activatedProfiles/>
     | or the command line, profiles have to have an ID that is unique.
     |
     | An encouraged best practice for profile identification is to use a consistent naming convention
     | for profiles, such as 'env-dev', 'env-test', 'env-production', 'user-jdcasey', 'user-brett', etc.
     | This will make it more intuitive to understand what the set of introduced profiles is attempting
     | to accomplish, particularly when you only have a list of profile id's for debug.
     |
     | This profile example uses the JDK version to trigger activation, and provides a JDK-specific repo.
    <profile>
      <id>jdk-1.4</id>

      <activation>
        <jdk>1.4</jdk>
      </activation>

      <repositories>
        <repository>
          <id>jdk14</id>
          <name>Repository for JDK 1.4 builds</name>
          <url>http://www.myhost.com/maven/jdk14</url>
          <layout>default</layout>
          <snapshotPolicy>always</snapshotPolicy>
        </repository>
      </repositories>
    </profile>
    -->

    <!--
     | Here is another profile, activated by the system property 'target-env' with a value of 'dev',
     | which provides a specific path to the Tomcat instance. To use this, your plugin configuration
     | might hypothetically look like:
     |
     | ...
     | <plugin>
     |   <groupId>org.myco.myplugins</groupId>
     |   <artifactId>myplugin</artifactId>
     |
     |   <configuration>
     |     <tomcatLocation>${tomcatPath}</tomcatLocation>
     |   </configuration>
     | </plugin>
     | ...
     |
     | NOTE: If you just wanted to inject this configuration whenever someone set 'target-env' to
     |       anything, you could just leave off the <value/> inside the activation-property.
     |
    <profile>
      <id>env-dev</id>

      <activation>
        <property>
          <name>target-env</name>
          <value>dev</value>
        </property>
      </activation>

      <properties>
        <tomcatPath>/path/to/tomcat/instance</tomcatPath>
      </properties>
    </profile>
    -->
  </profiles>

  <!-- activeProfiles
   | List of profiles that are active for all builds.
   |
  <activeProfiles>
    <activeProfile>alwaysActiveProfile</activeProfile>
    <activeProfile>anotherAlwaysActiveProfile</activeProfile>
  </activeProfiles>
  -->
</settings>
```