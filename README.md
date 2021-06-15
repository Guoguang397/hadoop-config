# 大数据实验

## Websites
Documents: https://hadoop.apache.org/docs/r2.7.2/

Hadoop 2.7.2: http://archive.apache.org/dist/hadoop/core/hadoop-2.7.2/

## Installation
### 0x01: Turn off firewall (Run with root privilege)
- Check Status: `systemctl status ufw`

- Stop: `systemctl stop ufw`

- Disable: `systemctl disable ufw`

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

NameNode|NameNode2|DataNode|ResourceManager|NodeManager
--|--|--|--|--
 x| |x| |x
  | | |x|x|x
  | |x|x| |x

## Update Environment Variables
- `source /etc/profile`