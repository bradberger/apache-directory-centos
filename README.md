# Apache Directory Installation

This is a setup guide for Apache Directory Server on CentOS 7.

## Install Java

### OpenJDK

Apache Directory requires Java, so the first step is to install Java. The OpenJDK should work
fine, and it simplifies installation:

```bash
yum install java-1.7.0-openjdk
```

You can verify it installed correctly with:

```bash
java -version
```

### Oracle JDK

Alternatively, you can install the Oracle JDK. Pick the appropriate RPM file from
the [Java download page](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html).
At time of writing, the latest version was `http://download.oracle.com/otn-pub/java/jdk/8u66-b17/jre-8u66-linux-x64.rpm`. To download and install
it, run the following command, replacing the RPM file name with the latest version:

```bash
cd ~
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie"  "http://download.oracle.com/otn-pub/java/jdk/8u66-b17/jre-8u66-linux-x64.rpm"
sudo yum localinstall jre-8u66-linux-x64.rpm
```

Verify it's install with:

```bash
java -version
```

If you've installed a different version of Java previously, and want to update the 
default version, use the command:

```bash
sudo alternatives --config java
```

## Download and Install

Download the latest version from
[directory.apache.org](http://directory.apache.org/apacheds/download/download-linux-rpm.html) and install:

```bash
cd ~
wget http://www.us.apache.org/dist//directory/apacheds/dist/2.0.0-M20/apacheds-2.0.0-M20-x86_64.rpm
sudo yum localinstall apacheds-2.0.0-M20-x86_64.rpm
```

This will install the server files in `/opt/apacheds-2.0.0_M20`. To make it a bit
easier to rememeber the location, you can set up a symlink:

```bash
ln -s /opt/apacheds-2.0.0_M20 /opt/apacheds
```

That creates a service called `apacheds-2.0.0_M20-default` so you can 
interact with it using the regular Linux `service` commands:

```bash
service apacheds-2.0.0_M20-default <command>
```

You'll probably want the `apacheds` binary on the regular system path,
as well as the Java paths, so you can do that by adding an `/etc/profile.d` 
file to add it to the `PATH`. The example below is the path for the Oracle
Java installtion, so if you're using OpenJDK it will vary.

```bash
# File /etc/profile.d/apacheds.sh
export PATH=/opt/apacheds/bin:/usr/java/default/bin:$PATH
export JAVA_HOME=/usr/java/default
```

Make sure the new file is executable with:

```
sudo chmod +x /etc/profile.d/apacheds.sh
```

Since the service name is a bit long, you can just 
rename it to make it easier to interact with:

```bash
mv /etc/init.d/apacheds-2.0.0_M20-default /etc/init.d/apacheds
```

## Interacting with the Server

You can check if the server is running with the command:

```bash
service apacheds status
```

If you want to interact with the server while connected to the
SSH terminal, you'll need the `ldap-clients` package:

```bash
 yum install -y openldap-clients
```

After installing that, you can see test access with the following
command. This will give you a verbose dump of the LDAP data for everything 
under the specified DN:

```bash
ldapsearch -D "uid=admin,ou=system" -w secret -p 10389 -h localhost -b "ou=system"
```

## Firewall Rules

To access the server from outside, you'll need to set up [firewall rules](iptables-setup.md).
There's an overview of the firewall configuration process [here](iptables-setup.md).
