## HDFS Commandline Usage

 #### 查看帮助
  `hdfs dfs -help `
     
 #### 查看当前目录信息
  `hdfs dfs -ls /`
     
 #### 上传文件
  `hdfs dfs -put /本地路径 /hdfs路径`
     
 #### 剪切文件
  `hdfs dfs -moveFromLocal a.txt /aa.txt`
     
 #### 下载文件到本地
  `hdfs dfs -get /hdfs路径 /本地路径`
     
 #### 合并下载
  `hdfs dfs -getmerge /hdfs路径文件夹 /合并后的文件`
     
 #### 创建文件夹
  `hdfs dfs -mkdir /hello`
     
 #### 创建多级文件夹
  `hdfs dfs -mkdir -p /hello/world`
     
 #### 移动hdfs文件
  `hdfs dfs -mv /hdfs路径 /hdfs路径`
     
 #### 复制hdfs文件
  `hdfs dfs -cp /hdfs路径 /hdfs路径`
     
 #### 删除hdfs文件
  `hdfs dfs -rm /aa.txt`
     
 #### 删除hdfs文件夹
  `hdfs dfs -rm -r /hello`
     
 #### 查看hdfs中的文件
  `hdfs dfs -cat /文件`
  `hdfs dfs -tail -f /文件`
     
 #### 查看文件夹中有多少个文件
  `hdfs dfs -count /文件夹`
     
 #### 查看hdfs的总空间
  `hdfs dfs -df /`

  `hdfs dfs -df -h /`
     
 #### 修改副本数    
  `hdfs dfs -setrep 1 /a.txt`