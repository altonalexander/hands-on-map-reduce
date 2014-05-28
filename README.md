hands-on-hadoop-training
========================
Download and install the following files:

Download and install oracle VirtualBox
https://www.virtualbox.org/wiki/Downloads 

Download the sandbox
http://hortonworks.com/hdp/downloads/


Installation

Sandbox Installation guide (windows)
http://hortonworks.com/wp-content/uploads/unversioned/pdfs/InstallingHortonworksSandbox2onWindowsusingVB.pdf 

Start the sandbox vm and then go to the gui
http://127.0.0.1:8888/


Lets run a map reduce!

    About map reduce
    http://hortonworks.com/hadoop/mapreduce/

    Lets try access via SSH
    ssh root@127.0.0.1 -p 2222

    mkdir /home/hduser
    echo foo foo quux labs foo bar quux > /home/hduser/words.txt
    
Copy that files from the local system to hdfs cluster
/usr/bin/hadoop dfs -copyFromLocal /home/hduser/words.txt /user/hdfs/words.txt

Lets go look at it on hdfs in the http://127.0.0.1:8000/filebrowser/

using command line download python map and reduce files
cd 
wget https://raw.githubusercontent.com/altonalexander/hands-on-map-reduce/master/mapper.py
wget https://raw.githubusercontent.com/altonalexander/hands-on-map-reduce/master/reducer.py
wget https://raw.githubusercontent.com/altonalexander/hands-on-map-reduce/master/words.txt


or upload the map and reduce files to hdfs
using the sandbox “file manager” download the zip of the github repository and upload it to hdfs

Try just passing the file to the map and reduce scripts to test your code
chmod +x /home/hduser/mapper.py
chmod +x /home/hduser/reducer.py
echo "foo foo quux labs foo bar quux" | /home/hduser/mapper.py
echo "foo foo quux labs foo bar quux" | /home/hduser/mapper.py | sort -k1,1 | /home/hduser/reducer.py

Command line map reduce using hadoop
/usr/bin/hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar \
-file /home/hduser/mapper.py    -mapper /home/hduser/mapper.py \
-file /home/hduser/reducer.py   -reducer /home/hduser/reducer.py \
-input /user/hdfs/words.txt -output /user/hdfs/words-output

Check in the file browser that it completed
/user/hdfs/words-output

Lets also try running it with the sandbox gui
go to ‘Job Design’
mapper: /home/hduser/mapper.py
reducer: /home/hduser/reducer.py
properties: name: mapred.output.dir value: /user/hue/words-output
properties: name: mapred.input.dir value: /user/hdfs/words.txt

    Check in the file browser that it completed
/user/hue/words-output




