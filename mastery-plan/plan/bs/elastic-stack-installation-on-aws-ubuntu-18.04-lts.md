# Elastic Stack installation on AWS Ubuntu 18.04 LTS

Create a security group with port 5601 (Kibana) open along with ssh

![](<../../../.gitbook/assets/image (200).png>)

Then go on to launch the instance. The os is Ubuntu 18.04 LTS and for the instance type I am choosing t3.medium but you can choose any instance with RAM > 4GB

![](<../../../.gitbook/assets/image (179).png>)

Choose our newly created security group

![](<../../../.gitbook/assets/image (209).png>)

And then wait for the server to be up.

![](<../../../.gitbook/assets/image (147).png>)

We are going to follow this steps :[https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html#install-beats](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html#install-beats)

But with a few modifications to use it on a cloud server.

### Installing ElasticSearch:

After you ssh to the server and run the usual `apt-update`. Use this to install and run elasticsearch. Prefer not to use the root account to run all the commands below.

```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.3.0-amd64.deb
sudo dpkg -i elasticsearch-7.3.0-amd64.deb
sudo /etc/init.d/elasticsearch start
```

Once done you can check if its working by running. \`

```
curl http://127.0.0.1:9200
```

You should get the following result.

![](<../../../.gitbook/assets/image (162).png>)

### Installing Kibana:

Go to the home folder and run the following commands:

```
curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-7.3.0-linux-x86_64.tar.gz
tar xzvf kibana-7.3.0-linux-x86_64.tar.gz
cd kibana-7.3.0-linux-x86_64/
```

Once done run `vi config/kibana.yml` and uncomment and change the serverhost to 0.0.0.0 instead of localhost. Keep the rest as is.

![](<../../../.gitbook/assets/image (20).png>)

Now run `./bin/kibana`

![](<../../../.gitbook/assets/image (211).png>)

Once you see `status changed from yellow to green- Ready` message. Go to your browser and enter `<public ip>:5601`

![](<../../../.gitbook/assets/image (120).png>)

### Installing Filebeat to get the logs data

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.3.0-amd64.deb
sudo dpkg -i filebeat-7.3.0-amd64.deb
```

Define the path (or paths) to your log files.

![](<../../../.gitbook/assets/image (52).png>)

![](<../../../.gitbook/assets/image (112).png>)
