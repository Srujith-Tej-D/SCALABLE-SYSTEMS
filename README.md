# SCALABE-SYSTEMS

GitHub Link :-  https://github.com/Srujith-Tej-D/SCALABE-SYSTEMS
Data set Link :- https://www.kaggle.com/c/microsoft-malware-prediction/data

--> Installation Requirements:
       1. Hadoop 3.4.0
       2. Apache Spark
       3. JAVA , Open-JDK & default-JRE
       4. Python
       5. pip
       6. pip install pandas
       7. pip install plotly
       8. pip installpyspark

---> Setup Instructions: 

       1. Create ubuntu VM's (if cluster)

       2. sudo apt update
          sudo apt install default-jre
          sudo apt install default-jdk

      3. modify the /etc/hosts file remove the localhost add hostname beside ip 

      4. sudo groupadd hadoop
         sudo useradd -ghadoop hduser -m -s /bin/bash
         sudo passwd hduser
         sudo usermod -aG sudo hduser
         su - hduser
         ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
         cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
         ssh-copy-id username@hostname ( hostname of other vms )
         
       5. sudo vim /etc/sysctl.conf
          Add the below at the end of file

          net.ipv6.conf.all.disable_ipv6=1
          net.ipv6.conf.default.disable_ipv6=1
          net.ipv6.conf.lo.disable_ipv6=1

       6. sudo sysctl -p

       7.  cd /home/hduser
           curl -O https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
           curl -O https://www.apache.org/dyn/closer.lua/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz

       8. extract the tar files in in /usr/local/ and create symlinks as hadoop and spark

       9. sudo chown -R hduser:hadoop hadoop*
          sudo chown -R hduser:hadoop spark*

       10. vim /home/hduser/.bashrc 
           Add the following at the end of file 
           export HADOOP_CLASSPATH=/usr/lib/jvm/java-11-openjdkamd64/lib/tools.jar:/usr/local/hadoop/bin/hadoop
           export HADOOP_MAPRED_HOME=/usr/local/hadoop
           export HADOOP_HDFS_HOME=/usr/local/hadoop
           export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
           export PATH=$PATH:.:/usr/lib/jvm/java-11-openjdk-amd64/bin:/usr/local/hadoop/bin
           export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
           export LD_LIBRARY_PATH=/usr/local/hadoop/lib/native:$LD_LIBRARY_PATH
           export SPARK_HOME=/usr/local/spark
           export PYSPARK_PYTHON=python3
           export PATH=$SPARK_HOME/bin:$PATH

       11. source /home/hduser/.bashrc 

       12. refer the configuration files uploaded for both vm1 and vm2 
           in this path /usr/local/hadoop/etc/hadoop/
           Refer and modify the following
                                           * Core-site.xml
                                           * hdfs-site.xml
                                           * mapred-site.xml
          
           Note: I have uploaded all those conf file to this repo 

        13. add "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64" to hadoop-env.sh file 

        14. create required folders:
                            mkdir ~/tmp
                            mkdir ~/hdfs
                            chmod 750 ~/hdfs

        15. format name node and start dfs and yarn 
                     cd /usr/local/hadoop
                     bin/hdfs namenode -format ( Master and worker nodes )
                     sbin/start-dfs.sh ( Master node )
                     sbin/start-yarn.sh ( Master node )
                     hdfs --daemon start datanode ( Worker node )

       16. modify spark-env.sh with internal and external IP of the vm by refereeing the uploaded file
           file path /usr/local/spark/conf

       17. start spark as master and worker in vm1 and as worker in vm2
              cd /usr/local/spark
              sbin/start-master.sh ( Master node )
              sbin/start-worker.sh spark://master-vm:7077 ( Master and worker nodes )

       18. access ui's and check 

           hadoop http://external_ip:9870
           spark  http://external_ip:8080

       19. Install kagle and download the Data sets through CLI pip install kaggle kaggle --version kaggle competitions download -c microsoft-malware-prediction

Note : Modify the Vm specfic config changes carefully by referring the uploaded files 


--->  Execution Guide:
       Important Commands :

Hadoop :- 
 sbin/start-dfs.sh
 sbin/start-yarn.sh
 bin/hdfs namenode -format

 hdfs dfs -ls /
 hdfs dfs -mkdir /microsoft
 hdfs dfs -put train.csv /microsoft/
 hdfs fsck /microsoft/train.csv -files -blocks -locations

spark :- 
 sbin/start-master.sh
 sbin/start-worker.sh spark://master-vm:7077

Jupyter :- Install jupyter as a service and access  the UI in browser by using external IP


---> Troubleshooting Tips:

      1. web UI not accessible :- check vm firewall rules
      2. Telnet not happening ( no communication between the vm's ) :- check firewall and ports
      3. hadoop not starting :- check from which user trying to execute and .bashrc file and logs
      
           
             
            
            

        

       
      
           
             
            
            

        

       
