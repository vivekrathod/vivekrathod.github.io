---
published: true
---
# AltoroJ WebApp for Security Scanning

## AltoroJ
[AltoroJ](https://github.com/HCL-TECH-SOFTWARE/AltoroJ) is a Java WebApp that is purposely configured with many known security vulnerabilities. I needed to use the the tool I work on for running automated webapp scanning.  

## Installation

### JDK 1.8
It requires *JDK 1.8*, *Apache Tomcat 7* and *Eclipse IDE for Enterprise Java and Web Developers* with *Gradle's Buildship Eclipse plug-in* for compiling it. I am using Ubuntu 20.4.
```
vivek@vivek-ub18:~$ sudo apt-get install openjdk-8-jdk
vivek@vivek-ub18:~$ which java
/usr/bin/java
vivek@vivek-ub18:~$ java -showversion
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (build 1.8.0_292-8u292-b10-0ubuntu1~20.04-b10)
OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)
```

### Apache Tomcat 7
Download *Apache Tomcat 7* and simply extract it somewhere (I chose home directory) - this will be the *installation* directory we will refer to later when configurin *Eclipse*
```
vivek@vivek-ub18:~$ wget https://apache.osuosl.org/tomcat/tomcat-7/v7.0.109/bin/apache-tomcat-7.0.109.tar.gz
vivek@vivek-ub18:~$ tar xvzf apache-tomcat-7.0.109.tar.gz
```

### Eclipse IDE for Enterprise Java and Web Developers
#### Installation
I used the latest *Eclipse IDE for Enterprise Java and Web Developers* package from [here](https://www.eclipse.org/downloads/packages/). Make sure you choose the for *Enterprise Java and Web Developers* as that contains allows one to use configure and run the webapp on Apache Tomcat server later on.
```
vivek@vivek-ub18:~$ wget https://eclipse.mirror.rafal.ca/technology/epp/downloads/release/2021-03/R/eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
vivek@vivek-ub18:~$ tar xvzf eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
```
One can also choose the [_Eclipse installer package_](https://www.eclipse.org/downloads/download.php?file=/oomph/epp/2021-03/R/eclipse-inst-jre-linux64.tar.gz) which will allow you to do the same. 
In case, you already have or choose the regular *Eclipse for Java Developers* then you can goto 
`Eclipse Marketplace -> Eclipse Enterprise Java and Web Developer Tools 3.16` to install the *Java EE Developer Tools* That should allow you to later configure *Apache Tomcat 7* for running AltoroJ.

#### Point to Open JDK 1.8
The default JRE for my version of eclipse was 15 - in order to use it for AltoroJ we need to change it and point it to Open JDK 1.8 that we just installed. Go to _Window_ -> _Preferences_
![e1.PNG]({{site.baseurl}}/assets/e1.PNG)

1. On the right hand side click *Add* button
2. choose to add *Standard VM*
3. click *Directory* and point to `usr` directory where Open JDK 1.8 was previously installed. Eclipse should find automatically find the JVM and add the required JARs as shown below 
![e2.PNG]({{site.baseurl}}/assets/e2.PNG)
4. check it to make it default as shown here 
![e3.PNG]({{site.baseurl}}/assets/e3.PNG)

#### Configure Apache Tomcat 7
Again, go to _Window_ -> _Preferences_ -> _Server_ -> _Runtime Environments_
![e4.PNG]({{site.baseurl}}/assets/e4.PNG)

1. click _Add_
2. choose _Apache Tomcat 7.0_
![e5.PNG]({{site.baseurl}}/assets/e5.PNG)
3. point to the installation directory (where we extracted the tar.gz for Tomcat )
![e6.PNG]({{site.baseurl}}/assets/e6.PNG)


### Import AltoroJ project from GitHub
1. Go to File -> Import -> Git -> Projects from Git
2. Choose "Clone URI" and use the following URI on the next screen: https://github.com/AppSecDev/AltoroJ/
3. Proceed through the wizard without making any changes until you get to Wizard for Project import. At this point, choose to "Import existing Eclipse projects" and then continue through and finish the wizard
(Taken from the documentaion at this [AltoroJ page](https://github.com/HCL-TECH-SOFTWARE/AltoroJ/blob/master/Importing%20AltoroJ%20into%20Eclipse%20from%20GitHub.md))

### Run AltoroJ
1. Right-click on your AltoroJ project in Eclipse's Project Explorer
2. Choose Run As -> Run On Server
3. Select your Apache Tomcat v7.0 Server instance and click Finish
4. AltoroJ should now come up in a built-in web browser
5. Confirm that AltoroJ is properly initialized by logging in using the following credentials: Username: jsmith Password: demo1234
(Taken from the documentaion at this [AltoroJ page](https://github.com/HCL-TECH-SOFTWARE/AltoroJ/blob/master/Importing%20AltoroJ%20into%20Eclipse%20from%20GitHub.md))

### Deploy AltoroJ
I wanted to automate this process of running AltoroJ whenever the machine starts. In order to do that first lets see how we can deploy the AltoroJ webapp to Tomcat 7 so we do not need Eclipse to run it 
1. Right-click on your AltoroJ project in Eclipse's Project Explorer and choose Export to WAR file
2. Choose the *webapps* directory under Tomcat 7 installation directory

### Autostart Tomcat 7
- I had untarred Tomcat 7 in my home directory earlier. I moved it to /user/share/Tomcat7
```
	:~$ mv apache-tomcat-7.0.109/ /usr/share/tomcat7
```
- Create the tomcat7 autostart script `sudo vi /etc/init.d/tomcat7`
- Paste this below inside
```
	#!/bin/bash

	### BEGIN INIT INFO
	# Provides:        tomcat7
	# Required-Start:  $network
	# Required-Stop:   $network
	# Default-Start:   2 3 4 5
	# Default-Stop:    0 1 6
	# Short-Description: Start/Stop Tomcat server
	### END INIT INFO

	PATH=/sbin:/bin:/usr/sbin:/usr/bin

	start() {
	sh /usr/share/tomcat7/bin/startup.sh
	}

	stop() {
	sh /usr/share/tomcat7/bin/shutdown.sh
	}

	case $1 in
	start|stop) $1;;
	restart) stop; start;;
	*) echo "Run as $0 <start|stop|restart>"; exit 1;;
	esac
```
- Set proper permissions and level at which it should run it during boot
```
	chmod 755 /etc/init.d/tomcat7
	update-rc.d tomcat7 defaults
```
- Test if it can run without errors usig `service tomcat7 <stop|start|restart>`
