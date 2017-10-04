1: Overview

NOTICE!

Elastic Stack and Beats are in high-speed iterations now.The following article is based on the newest Elastic Stack version:

Oracle JDK 1.8.0_111
ElasticSearch 5.0.1
Logstash 5.0.1
Kibana 5.0.1
FileBeat 5.0.1
Platform: Ubuntu server 16.04.1

1.1: Brife Introduction

The Beats are open source data shippers that you install as agents on your servers to send different types of operational data to Elasticsearch. Beats can send data directly to Elasticsearch or send it to Elasticsearch via Logstash, which you can use to enrich or archive the data.

Packetbeat, Topbeat, and Filebeat are a few examples of Beats. Packetbeat is a network packet analyzer that ships information about the transactions exchanged between your application servers. Topbeat is a server monitoring agent that periodically ships system-wide and per-process statistics from your servers. And Filebeat ships log files from your servers.

Elastic provide a simple way to build yourself's beats if you have specific use case.That's libbeat.The libbeat library, written entirely in Golang, offers the API that all Beats use to ship data to Elasticsearch, configure the input options, implement logging, and more.

beats-platform
beats-platform
1.2: Architecture&&Function

A regular Elastic Stack with Beats consists of:

One or more Beats. You install the Beats on your servers to capture operational data.
Elasticsearch for storage and indexing.
Logstash (optional) for inserting data into Elasticsearch.
Kibana for the UI.
Kibana dashboards for visualizing the data.
I use my laptop for kibana.One virtual machine for elasticsearch,one for logstash,one for filebeat.In the future,I can also 'elastic' them to clusters.



Laptop 192.168.86.1 kibana
virtual machine 192.168.86.129 elasticsearch
virtual machine 192.168.86.130 logstash
virtual machine 192.168.86.128 filebeat
2: Installation Step

In this part, I will describe the installation step.It has two parts: master machine and beats machine.

2.1: Install ElasticSearch

ElasticSearch need 2GB RAM

Notice:The following command need root privilege

Download the deb package 5.0.1 to your machine,and install it.

sudo service elasticsearch start
To test that the Elasticsearch daemon is up and running, try sending an HTTP GET request on port 9200.

sudo curl http://127.0.0.1:9200
NOTICE:You may want to see the result on your own laptop.But when you type http://vm_ip_address:9200 in your browser,you may not to see the result.In this case,you should change the configuration file of elasticsearch.

vim /etc/elasticsearch/elasticsearch.yml
change the property 'network.host:' to 0.0.0.0

2.2: Install Logstash

The simplest architecture for the Beats platform setup consists of one or more Beats, Elasticsearch, and Kibana. This architecture is easy to get started with and sufficient for networks with low traffic. It also uses the minimum amount of servers: a single machine running Elasticsearch and Kibana. The Beats insert the transactions directly into the Elasticsearch instance.

If you want to perform additional processing or buffering on the data, however, you’ll want to install Logstash.

An important advantage to this approach is that you can use Logstash to modify the data captured by Beats in any way you like. You can also use Logstash’s many output plugins to integrate with other systems.

Elastic Stack with Beats
Elastic Stack with Beats
1:To download deb package and install Logstash.

sudo service logstash start
2:In this setup, the Beat sends events to Logstash. Logstash receives these events by using the Beats input plugin for Logstash and then sends the transaction to Elasticsearch by using the Elasticsearch output plugin for Logstash. The Elasticsearch output plugin uses the bulk API, making indexing very efficient.

NOTICE!!!!!

In logstash5.0.1,logstash-input-beats and logstash-output-elasticsearch are been installed by default,you dont need to install them manually anymore.The offical gudie give the wrong guidence.You can use the following command to check out how many plugins you have installed

When you install Logstash with deb package,the directory is in /usr/share/logstash,but not the directory which the offical said '/opt/logstash'.They are wrong again.

3:Configure Logstash to listen on port 5044 for incoming Beats connections and to index into Elasticsearch. You configure Logstash by creating a configuration file. For example, you can save the following example configuration to a file called logstash.conf.logstash.conf is located at/etc/logstash/conf.d/logstash.conf

input {
 beats {
 port => 5044
 }
}

output {
 elasticsearch {
 hosts => "elasticsearch_ip:9200"
 manage_template => false
 index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
 document_type => "%{[@metadata][type]}"
 }
}
To use this setup, you’ll also need to configure your Beat to use Logstash. For more information, see the following guide.

4:Now you can start Logstash. Use the command that works with your system. If you installed Logstash as a deb or rpm package, make sure the config file is in the expected directory.

sudo service logstash start
P.S: ruby's offical site is under GFW,we can hardly connect to this site in China.So we can use ruby china to instead.Under the Logstash directory ,change the Gemfile

vim Gemfile

# This is a Logstash generated Gemfile.
# If you modify this file manually all comments and formatting will be lost.

#source "https://rubygems.org"
source "https://gems.ruby-china.org"
In this way ,we can install logstash's plugin now.

2.3: Install kibana

Kibana is a visualization application that gets its data from Elasticsearch. It provides a customizable and user-friendly UI in which you can combine various widget types to create your own dashboards. The dashboards can be easily saved, shared, and linked.

For getting started, we recommend installing Kibana on the same server as Elasticsearch, but it is not required. If you install the products on different servers, you’ll need to change the URL (IP:PORT) of the Elasticsearch server in the Kibana configuration file, config/kibana.yml(or /etc/kibana/kibana.yml   use deb package), before starting Kibana.

Download the deb package and install it ,then run Kibana.

sudo service kibana start
To launch the Kibana web interface, point your browser to port 5601. For example,

http://127.0.0.1:5601
2.4: Install Filebeat

Each Beat is a separately installable product. We use the Filebeat.

Download the deb package and install it.

2.4.1: Configure Filebeat

To configure Filebeat, you edit the configuration file. For rpm and deb, you’ll find the configuration file at /etc/filebeat/filebeat.yml.

You can change the log path of your server.Edit the following code segments.

filebeat.prospectors:
- input_type: log
 paths:
 - /var/log/*.log
Because we use Logstash,to do this, you edit the Filebeat configuration file to disable the Elasticsearch output by commenting it out and enable the Logstash output by uncommenting the logstash section:

#----------------------------- Logstash output --------------------------------
output.logstash:
 hosts: ["127.0.0.1:5044"]
The abrove ip address is the Logstash machine address.You should also comment the elasticsearch part to disable the elasticsearch function.

Now you need to  load the index template into Elasticsearch manually .You can load the template by running the following command:

curl -XPUT 'http://localhost:9200/_template/filebeat' -d@/etc/filebeat/filebeat.template.json
where localhost:9200 is the IP and port where Elasticsearch is listening.

2.4.2: RUN!

sudo service filebeat start
Filebeat is now ready to send log files to your defined output.

2.4.3：Loading the Kibana Index Pattern

Elastic don’t offer prebuilt dashboards for visualizing Filebeat data. However, to make it easier for you to explore Filebeat data in Kibana, Elastic have created a Filebeat index pattern: filebeat-*. To load this pattern, you can use the script that’s provided for loading dashboards.

After you’ve created the index pattern, you can select the filebeat-* index pattern in Kibana to explore Filebeat data.

filebeat directory is /usr/share/filebeat



go to the script directory

./import_dashboards -dir /home/walle/beats-dashboards-5.0.1/filebeat/ -es http://192.168.86.129:9200 -only-index
more details see the official guide

3:SUMMARY

Now, after few seconds,you can see the filebeat-* pattern on Kibana.Until now, Elastic Stack with beats platform has been successfully built.


Reference:

1:https://www.elastic.co/guide/en/beats/libbeat/1.3/index.html

2:https://www.elastic.co/guide/en/beats/filebeat/1.3/index.html

3:http://www.jianshu.com/p/4fe495639a9a

4:http://gems.ruby-china.org/

5:https://ruby.taobao.org/
