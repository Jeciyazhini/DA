# Apache Hadoop Installation Guide for v3.4.0
## Requirements
### Java

Hadoop requires Java version 8.
If you have a different version of Java installed, uninstall it via Control Panel --> Uninstall a program.
Download Java version 8 from JavaJDKv8.
Choose Windows x86 Installer for Intel processors, otherwise download Windows x64 Installer.
Proceed with the installation.
7Zip
Hadoop is distributed in compressed tarballs (.tar.gz), and 7Zip is an ideal tool for decompressing them.
Download 7Zip from here.
Install 7Zip.
Hadoop
Download Hadoop from the official release page: link.
Ensure you download the binary version.
Installation
Decompression
Run 7Zip as Administrator.
Unzip the downloaded hadoop-<VER>.tar.gz file.
This will generate a tarball (.tar) file.
Unzip the tarball again.
Copy the extracted folder to C:\.
Decompression (Alternate Method)
Open Command Prompt and run mkdir C:\Hadoop.
Navigate to your Downloads directory and run tar -xvzf hadoop-3.3.0.tar.gz -C C:\Hadoop\.
Setting up the Environment
Search for Edit the System Environment Variables and select Environment Variables.
Under System variables, click New and set the name to JAVA_HOME with the value as your JDK installation directory, e.g., C:\Java (this may vary depending on where you installed Java).
Create another new variable named HADOOP_HOME with the value as your Hadoop installation directory, e.g., C:\Hadoop\hadoop-<VER> (adjust the path based on where you extracted Hadoop).
You can verify that the Environment Variables are set correctly by typing echo %VARIABLE_NAME% in Command Prompt.
Click on the Path variable, then click Edit.
Add the following paths to the Path variables one by one:
shell
Copy code
%JAVA_HOME%/bin
%HADOOP_HOME%/bin
Verify the setup by running java -version and hadoop -version in Command Prompt.
Additional Packages
Download the bin.zip from the specified repository.
Extract it and paste it into %HADOOP_HOME%\bin.
Open and run winutils.exe from %HADOOP_HOME%\bin.
Configuration
Now, configure Hadoop with settings for Core, YARN, MapReduce, and HDFS.

Configure Core Site
Edit the core-site.xml file located in the %HADOOP_HOME%\etc\hadoop folder.
Replace the configuration element with the following:
xml
Copy code
<configuration>
   <property>
     <name>fs.default.name</name>
     <value>hdfs://0.0.0.0:19000</value>
   </property>
</configuration>
Configure HDFS
Edit the hdfs-site.xml file in the %HADOOP_HOME%\etc\hadoop folder.
Create the following two subfolders on your system for the namenode and datanode directories:
arduino
Copy code
mkdir C:\hadoop\hadoop-3.4.0\data\datanode
mkdir C:\hadoop\hadoop-3.4.0\data\namenode
Replace the configuration element in hdfs-site.xml with:
xml
Copy code
<configuration>
   <property>
     <name>dfs.replication</name>
     <value>1</value>
   </property>
   <property>
     <name>dfs.namenode.name.dir</name>
     <value>/hadoop/hadoop-3.4.0/data/namenode</value>
   </property>
   <property>
     <name>dfs.datanode.data.dir</name>
     <value>/hadoop/hadoop-3.4.0/data/datanode</value>
   </property>
</configuration>
Configure MapReduce and YARN Site
Edit the mapred-site.xml file in the %HADOOP_HOME%\etc\hadoop folder.

Replace the configuration element with the following:

xml
Copy code
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>%HADOOP_HOME%/share/hadoop/mapreduce/*,%HADOOP_HOME%/share/hadoop/mapreduce/lib/*,%HADOOP_HOME%/share/hadoop/common/*,%HADOOP_HOME%/share/hadoop/common/lib/*,%HADOOP_HOME%/share/hadoop/yarn/*,%HADOOP_HOME%/share/hadoop/yarn/lib/*,%HADOOP_HOME%/share/hadoop/hdfs/*,%HADOOP_HOME%/share/hadoop/hdfs/lib/*</value>
    </property>
</configuration>
Edit the yarn-site.xml file in the %HADOOP_HOME%\etc\hadoop folder.

xml
Copy code
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME,HADOOP_HOME</value>
    </property>
</configuration>
Initialization
HDFS
Format the HDFS namenode by running the following command in Command Prompt:
lua
Copy code
hdfs namenode -format
Start HDFS Daemons
To start HDFS daemons, run the following command in Command Prompt:
perl
Copy code
%HADOOP_HOME%\sbin\start-dfs.cmd
Two Command Prompt windows will open: one for the datanode and another for the namenode. Do not close these windows.
Verify the HDFS web portal by visiting http://localhost:9870/dfshealth.html#tab-overview in your browser.
Start YARN Daemons
Important: You may encounter permission issues if you start YARN daemons as a regular user. To avoid this, open Command Prompt as an administrator.
Run the following command in an elevated Command Prompt window (Run as administrator) to start YARN daemons:
perl
Copy code
%HADOOP_HOME%\sbin\start-yarn.cmd
Again, two Command Prompt windows will open: one for the resource manager and another for the node manager. Do not close these windows.
Verify the Hadoop Resource Manager by visiting http://localhost:8088 in your browser.