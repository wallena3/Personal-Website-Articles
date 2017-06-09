在我的毕业设计中用到的两个Shell



The linux shell file that can let the searching engine run or stop automatically.

start.sh

#!/bin/bash
#
# script_tool_for_linux
#
# WARING:if you want to stop the environment,use the shell stop.sh fist
# then stop the start.sh shell
#
# Use command: `bash script_tool_for_linux.sh`

/home/walle/Documents/hadoop-2.5.2/sbin/start-dfs.sh
 echo '**********Successfully start dfs system**********'
 /home/walle/Documents/hadoop-2.5.2/sbin/start-yarn.sh

echo '**********Successfully start yarn system**********'
 /home/walle/Documents/hbase-0.98.18-hadoop2/bin/start-hbase.sh

echo '**********Successfully start hbase**********'
 /home/walle/Documents/elasticsearch-1.4.4/bin/elasticsearch -d

echo '**********Successfully start elasticsearch**********'
 sleep 5

echo '**********Start Nutch**********'
 echo 'Inject URLS'
 /home/walle/Documents/apache-nutch-2.3.1/runtime/deploy/bin/nutch inject urls

 for ((i=1;;++i))
 do
 echo 'Generate a new set of URLS'
 /home/walle/Documents/apache-nutch-2.3.1/runtime/deploy/bin/nutch generate -topN 10
 echo 'Fetch the URLS'
 /home/walle/Documents/apache-nutch-2.3.1/runtime/deploy/bin/nutch fetch -all
 echo 'Parse pages'
 /home/walle/Documents/apache-nutch-2.3.1/runtime/deploy/bin/nutch parse -all
 echo 'Update databse'
 /home/walle/Documents/apache-nutch-2.3.1/runtime/deploy/bin/nutch updatedb -all
 # sleep 600
 #wait 10 minutes to let the generate/fetch/update loop done
 echo 'Elasticsearch indexing'
 /home/walle/Documents/apache-nutch-2.3.1/runtime/deploy/bin/nutch index -all
 sleep 10800
 #refresh each 3 hours
 done
stop.sh

#!/bin/bash
#
# script_tool_for_linux
#
# WARING:if you want to stop the environment,use the shell stop.sh fist
# then stop the start.sh shell
#
# Use command: `bash script_tool_for_linux.sh`

/home/walle/Documents/hbase-0.98.18-hadoop2/bin/stop-hbase.sh
 echo '**********Successfully stop hbase.**********'
 /home/walle/Documents/hadoop-2.5.2/sbin/stop-yarn.sh
 echo '**********Successfully stop yarn system.**********'
 /home/walle/Documents/hadoop-2.5.2/sbin/stop-dfs.sh
 echo '**********Success stop dfs system.**********'
 curl -XPOST http://localhost:9200/_cluster/nodes/_shutdown
 echo '
 **********Successfully stop elasticsearch.**********'
