# 大数据实验

## Websites
Documents: https://hadoop.apache.org/docs/r2.7.2/

Hadoop 2.7.2: http://archive.apache.org/dist/hadoop/core/hadoop-2.7.2/

## Installation
### 0x01: Turn off firewall (Run with root privilege)
- Check Status: `systemctl status ufw`

- Stop: `systemctl stop ufw`

- Disable: `systemctl disable ufw`

### 0x02: Setup Java Environment
- Path: `vim /etc/profile`
```sh
#set java environment
JAVA_HOME=/home/hadoop/jdk1.8.0_144
JRE_HOME=$JAVA_HOME/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib/rt.jar
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASSPATH PATH
```
- Update environment variables: `source /etc/profile`


### 0x03: Setup SSH
- Install SSH: `apt install ssh`

- Edit SSH config: `sudo vim sshd_config` -> `PermitRootLogin yes`

- Generate Key Pair: `ssh-keygen -t rsa`

- Copy key: `ssh-copy-id root@hadoop2`

- Copy files: `scp <src> root@hadoop2:/tmp`

- Copy folder: `scp -r ...`

- Forget saved Key(s): `ssh-keygen -R hadoop1`


### 0x04: Setup Hadoop Config
- Site config: `~/hadoop/hadoop-2.7.2/etc/hadoop/core-site.xml`

```xml
<configuration>
<!--NameNode -->
<property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop1:9000</value>
</property>

<!--file store -->
<property>
        <name>hadoop.tmp.dir</name>
        <value>/root/hadoop/hadoop-2.7.2/data/tmp</value>
</property>

</configuration>
```
- HDFS config: `~/hadoop/hadoop-2.7.2/etc/hadoop/core-site.xml`
```xml
<configuration>

<!-- Number of copies -->
<property>
        <name>dfs.replication</name>
        <value>3</value>
</property>

<!-- Secondary NameNode -->
<property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>hadoop3:50090</value>
</property>

<!-- Web Page to show hadoop status -->
<property>
        <name>dfs.http.address</name>
        <value>0.0.0.0:50070</value>
</property>

</configuration>
```

- Edit: `hadoop-env.sh` -> `export JAVA_HOME=/root/hadoop/jdk1.8.0_144`

- Edit slaves: `~/hadoop/hadoop-2.7.2/etc/hadoop/slave`
```
hadoop1
hadoop2
hadoop3
```
## Extra Operations
### 0x01: Change Host Name
- Temporary: `/etc/hostname`

- Permanent `hostnamectl set-hostname <hostname>`

### 0x02: Change hosts
- Lookup ip: `ifconfig`

- Edit file: `/etc/hosts`

- Ping test: `ping [-c <Times>] <addr>`

## Structure
- NameNode
- DataNode
- Snapshot
- Resource Manager
- Node Manager

|NameNode|NameNode2|DataNode|ResourceManager|NodeManager|
|:--:|:--:|:--:|:--:|:--:|
|x| |x| |x|
| | |x|x|x|
| |x|x| |x|

## Update Environment Variables
- `source /etc/profile`