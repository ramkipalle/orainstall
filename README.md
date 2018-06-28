# orainstall

Installing the Oracle Preinstallation RPM

`# yum install -y oracle-database-server-12cR2-preinstall`

..

..

..

Installed:
  
  oracle-database-server-12cR2-preinstall.x86_64 0:1.0-3.el7

Dependency Installed:

...

Complete!

Change the password for user oracle

`# passwd oracle`

![change password](/images/change_password.png)


Install wget

`# yum install -y wget`


Install some useful things

`# rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`

`# yum install -y screen htop collectl rlwrap p7zip unzip sysstat perf iotop`

`# cd /etc/yum.repos.d/`

`# wget http://download.opensuse.org/repositories/shells:fish:release:2/CentOS_7/shells:fish:release:2.repo`

`# yum install -y fish`

`# yum install -y  jq`

Download and install jdk

`# rpm -ivh jdk-8u162-linux-x64.rpm`


Download Oracle software from OTN

Useful link: https://blog.pythian.com/how-to-download-oracle-software-using-wget-or-curl/

Verify that the files you downloaded are the same size as the corresponding files on Oracle Technology Network. 

Also verify the checksums are the same as noted on Oracle Technology Network using the command

`# cksum filename.zip`

Move the zip file to /tmp

Change the user to oracle

`# unzip filename.zip`


------
#Setup X11 Forwarding

Download and Install X ming

Make changes to putty

![x11 forwarding](/images/x11_forwarding.PNG)

If you are behind a firewall in the work environment, update the proxy settings

![x11 forwarding](/images/proxy_settings1.PNG)

![x11 forwarding](/images/proxy_settings2.PNG)


Useful links to setup putty with proxy and X11

https://www.vscentrum.be/client/windows/putty-proxy

https://gist.github.com/Rillke/e2ceb88529aef1ae03f7f1c06c8376a2


-------

cd to database directory and run ./runInstall to install oracle. Make sure the X11 forwarding is enabled in Putty.

![oracle install](/images/oracle_install1.PNG)

![oracle install](/images/change_ownership.PNG)

Change the settings during installation

![oracle install](/images/oracle_settings.PNG)

![oracle install](/images/oracle_install3.PNG)

![oracle install](/images/oracle_install4.PNG)


----------

Once the installation is complete, add the following enntries to .bash_profile in oracle home directory (/home/oracle)

### Oracle Settings
export TMP=/tmp

export TMPDIR=$TMP

export ORACLE_HOSTNAME=localhost.localdomain

export ORACLE_UNQNAME=orcl12c

export ORACLE_BASE=/u01/app/oracle

export ORACLE_HOME=$ORACLE_BASE/product/12.2/db_1

export ORACLE_SID=orcl12c

export PATH=/usr/sbin:$PATH

export PATH=$ORACLE_HOME/bin:$PATH

### Java Settings
export JAVA_HOME=/usr/java/jdk1.8.0_162/

export LD_LIBRARY_PATH=/u01/app/oracle/product/12.2/db_1/lib:/usr/java/jdk1.8.0_162/jre/lib/amd64/server/


useful link https://pierreforstmanndotcom.wordpress.com/2013/06/27/how-to-install-oracle-12-1-in-silent-mode/

-------------

login to sqlplus

`$ rlwrap sqlplus sys/oracle as sysdba`

Make sure the orcl pdb has *read,write* as the value for open_mode

`SQL> set lines 200`

`SQL> select con_id, name, open_mode from v$pdbs;`

If not, run the following to change it.

`SQL> alter session set container=orcl;`

`SQL> alter database open;`
 
 
link:  https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/oracle/oracle-database-quick-create
 
 To set up the Oracle database to start automatically, first sign in as root. Then, create and update some system files.
 
 edit the file /etc/oratab and change the default N to Y
 
 `cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y`
 
 -------
 Create a file named /etc/init.d/dbora and paste the following contents:
 
#!/bin/sh
# chkconfig: 345 99 10
# Description: Oracle auto start-stop script.
#
# Set ORA_HOME to be equivalent to $ORACLE_HOME.
ORA_HOME=/u01/app/oracle/product/12.2/db_1
ORA_OWNER=oracle

case "$1" in
'start')
    # Start the Oracle databases:
    # The following command assumes that the Oracle sign-in
    # will not prompt the user for any values.
    # Remove "&" if you don't want startup as a background process.
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
    touch /var/lock/subsys/dbora
    ;;

'stop')
    # Stop the Oracle databases:
    # The following command assumes that the Oracle sign-in
    # will not prompt the user for any values.
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
    rm -f /var/lock/subsys/dbora
    ;;
esac

----------------

Change permissions on files with chmod as follows:

chgrp dba /etc/init.d/dbora
chmod 750 /etc/init.d/dbora


Create symbolic links for startup and shutdown as follows:

ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora

restart the VM

`# reboot`


link https://rmoff.net/2017/11/21/installing-oracle-goldengate-for-big-data-12-3-1-with-kafka-connect-and-confluent-platform/

Chek Oracle versions Etc..

[oracle@localhost ~]$ sqlplus SYS/oracle as sysdba

SQL> set lines 200
SQL> select * from v$version;
SQL> select instance_name from v$instance;
`SQL> select con_id, name, open_mode from v$pdbs;`

 
 Install Oracle Golden Gate
 
 cd /tmp
 Download Oracle GG software
 unzip the file
 ![oracle install](/images/unzip_gg.PNG)

 
 
 Build a response file (e.g. /tmp/oggcore.rsp)
 
oracle.install.responseFileVersion=/oracle/install/rspfmt_ogginstall_response_schema_v12_1_2
INSTALL_OPTION=ORA12c
SOFTWARE_LOCATION=/u01/app/ogg
START_MANAGER=true
MANAGER_PORT=7809
DATABASE_LOCATION=/u01/app/oracle/product/12.2/db_1/
INVENTORY_LOCATION=/u01/app/oraInventorygg/
UNIX_GROUP_NAME=oinstall

Insall OGG

fbo_ggs_Linux_x64_shiphome/Disk1/runInstaller -silent -nowait -responseFile /tmp/oggcore.rsp

![oracle install](/images/install_gg.PNG)

Press enter to get the command prompt

Check that OGG Manager is running

$ cd /u01/app/ogg

![oracle install](/images/is_ogg_running.PNG)

Configure DB for OGG
Add TNS entry in /u01/app/oracle/product/12.2/db_1/network/admin/tnsnames.ora

![oracle install](/images/tnsnames.PNG)

Setup Supplemental logging

![oracle install](/images/Configure_gg.PNG)

Configure OGG Extract

-----

Smoketest

----

Install OGG-BD


Install Confluent Platform

--

Configure & Smoke Test OGG-Kafka Connect → Kafka
Configure for OGG-BD Kafka Connect handler

----

Smoke test OGG -- Kafka Connect --> Kafka

------



 
 
 
 

