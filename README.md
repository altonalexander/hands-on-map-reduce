https://github.com/altonalexander/hands-on-map-reduce
--------

Download and install the following files:
-----

Download and install oracle VirtualBox: https://www.virtualbox.org/wiki/Downloads 

Download the sandbox http://hortonworks.com/hdp/downloads/


Installation
-----
Sandbox Installation guide (windows) http://hortonworks.com/wp-content/uploads/unversioned/pdfs/InstallingHortonworksSandbox2onWindowsusingVB.pdf 

Start the sandbox vm and then go to the gui http://127.0.0.1:8888/


Lets explore files on the file system
------

Lets try access via SSH and create a local file (skip to next section if you don't have an SSH client)

    ssh root@127.0.0.1 -p 2222

    mkdir /home/hduser
    echo foo foo quux labs foo bar quux > /home/hduser/words.txt
    
Copy that file from the local system to hdfs

    /usr/bin/hadoop dfs -copyFromLocal /home/hduser/words.txt /user/hdfs/words.txt

Lets go look at it on hdfs in the http://127.0.0.1:8000/filebrowser/


Lets play with map reduce!
--------

About map reduce: http://hortonworks.com/hadoop/mapreduce/

Using command line download python map and reduce files from this repository

    cd /home/hduser/
    wget https://raw.githubusercontent.com/altonalexander/hands-on-map-reduce/master/mapper.py
    wget https://raw.githubusercontent.com/altonalexander/hands-on-map-reduce/master/reducer.py
    wget https://raw.githubusercontent.com/altonalexander/hands-on-map-reduce/master/words.txt

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

Check the job in the sandbox 'Job Browser' under the username root

Check in the file browser for the output file: /user/hdfs/words-output


Map reduce with the sandbox gui
-----------

If you didn't upload the files in the previous section then use the sandbox “file manager” download the zip or git clone the github repository and upload it to hdfs and modify the locations of the following job.

    git clone https://github.com/altonalexander/hands-on-map-reduce.git

Lets also try running it with the sandbox gui
go to ‘Job Design’
- mapper: /home/hduser/mapper.py
- reducer: /home/hduser/reducer.py
- properties: name: mapred.output.dir value: /user/hue/words-output-gui
- properties: name: mapred.input.dir value: /user/hdfs/words.txt

Check the job in the sandbox 'Job Browser' under the username root

Check in the file browser that it completed
/user/hue/words-output-gui



Map reduce on more complex data
----------

Lets also try running it with the sandbox gui
go to ‘Job Design’
- mapper: /home/hduser/get_twitter_followers_locations/map.py
- reducer: /home/hduser/get_twitter_followers_locations/reduce.py
- properties: name: mapred.output.dir value: /user/hue/json-output-gui
- properties: name: mapred.input.dir value: /user/hue/new_tweets_tiny/new_tweets_tiny/15031046.tweets.json



Finally taking it to AWS
---------











