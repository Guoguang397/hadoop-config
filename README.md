# 大数据实验

## Websites
Documents: https://hadoop.apache.org/docs/r2.7.2/

Hadoop 2.7.2: http://archive.apache.org/dist/hadoop/core/hadoop-2.7.2/

## Web Interfaces
- http://hadoop1:50070
- http://hadoop2:8088
- http://hadoop3:50090

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

## Installation
### 0x01: Turn off firewall (Run with root privilege)
- Check Status: `systemctl status ufw`
- Stop: `systemctl stop ufw`
- Disable: `systemctl disable ufw`

### 0x02: Setup Environment Variables
#### Java Environment
- Path: `vim /etc/profile`

```sh
#set java environment
JAVA_HOME=/home/hadoop/jdk1.8.0_144
JRE_HOME=$JAVA_HOME/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib/rt.jar
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASSPATH PATH
```

#### Hadoop Environment
- Path: `vim /etc/profile.d/hadoop.sh`

```sh
export HADOOP_HOME=/home/student/hadoop/hadoop-2.7.2
export PATH=$PATH:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
```

- Update environment variables: `source /etc/profile`

### 0x03: Change Host Name and hosts
- Temporary: `/etc/hostname`
- Permanent `hostnamectl set-hostname <hostname>`
- Lookup ip: `ifconfig`
- Edit file: `/etc/hosts`
- Don't forget this one: `/etc/profile.d/hosts.sh`
- Ping test: `ping [-c <Times>] <addr>`


### 0x04: Setup SSH
- Install SSH: `apt install ssh`
- Edit SSH config: `sudo vim sshd_config` -> `PermitRootLogin yes`
- Generate Key Pair: `ssh-keygen -t rsa`
- Copy key: `ssh-copy-id root@hadoop2`
- Forget saved Key(s): `ssh-keygen -R hadoop1`

#### Scp Usage
- Copy files: `scp <src> root@hadoop2:/tmp`
- Copy folder: `scp -r ...`

### 0x05: Setup Hadoop Config
#### core-site.xml
- Core config: `~/hadoop/hadoop-2.7.2/etc/hadoop/core-site.xml`

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
        <value>/home/student/hadoop/hadoop-2.7.2/data/tmp</value>
</property>

</configuration>
```
---
#### hdfs-site.xml
- HDFS config: `~/hadoop/hadoop-2.7.2/etc/hadoop/hdfs-site.xml`
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

---
#### yarn-site.xml
- Yarn config: `~/hadoop/hadoop-2.7.2/etc/hadoop/yarn-site.xml`
```xml
<configuration>

<!-- Site specific YARN configuration properties -->

<!-- Determine how the reducer gets data -->
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>

<!-- Resource Manager Addr -->
<property>
    <name>yarn.resourcemanager.hostname</name>
    <value>hadoop2</value>
</property>

<!-- Resource Manager Web Addr -->
<property>
    <name>yarn.resourcemanager.webapp.address</name>
    <value>0.0.0.0:8088</value>
</property>

<!-- Log aggregation enabled -->
<property>
    <name>yarn.log-aggregation.enable</name>
    <value>true</value>
</property>

<!-- Log aggregation retain (s) -->
<property>
    <name>yarn.log-aggregation.retain</name>
    <value>604800</value>
</property>

</configuration>
```

---
#### mapred-site.xml
- Mapreduce config: `~/hadoop/hadoop-2.7.2/etc/hadoop/mapred-site.xml`
```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>

<!-- Run on yarn -->
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>

<!-- History Server Addr -->
<property>
    <name>mapreduce.jobhistory.address</name>
    <value>hadoop1:10020</value>
</property>

<!-- History Server Web Addr -->
<property>
    <name>mapreduce.jobhistory.webapp.address</name>
    <value>0.0.0.0:19888</value>
</property>

</configuration>
```

#### Config Env scripts
- Edit: `hadoop-env.sh` -> `export JAVA_HOME=/home/student/hadoop/jdk1.8.0_144`
- Edit: `yarn-env.sh` -> `export JAVA_HOME=/home/student/hadoop/jdk1.8.0_144`
- Edit: `mapred-env.sh` -> `export JAVA_HOME=/home/student/hadoop/jdk1.8.0_144`

#### Config Nodes
- Edit slaves: `~/hadoop/hadoop-2.7.2/etc/hadoop/slave`
```
hadoop1
hadoop2
hadoop3
```
### 0x06 Format HDFS (On hadoop1)
- Exec: `bin/hdfs namenode-format`

This command should be run only once.

### 0x07 Start Hadoop
- Start HDFS (Run on Namenode): `sbin/start-dfs.sh`
- Start yarn (Run on ResourceManager): `sbin/start-yarn.sh`
- Download file from hdfs: `hdfs dfs -get <src> <dst>`

### 0x08 Execute Jar on Hadoop
- Make directory: `bin/hdfs dfs -mkdir -p /user/root/input`
- Upload files: `bin/hdfs dfs -put <local> /user/root/input`
- Execute jar: `bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount /user/root/input /user/root/output`

**Notice:** output folder should **NOT** be created before launch jar file.

For more usage about HDFS please view [HDFS Document](docs/hdfs.md).