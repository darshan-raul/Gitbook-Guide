# Elastic Stack installation on AWS Ubuntu 18.04 LTS

Create a security group with port 5601 \(Kibana\) open along with ssh

![](../../../.gitbook/assets/image%20%2839%29.png)

Then go on to launch the instance. The os is Ubuntu 18.04 LTS and for the instance type I am choosing t3.medium but you can choose any instance with RAM &gt; 4GB

![](../../../.gitbook/assets/image%20%2852%29.png)

Choose our newly created security group

![](../../../.gitbook/assets/image%20%2841%29.png)

And then wait for the server to be up.

![](../../../.gitbook/assets/image%20%2868%29.png)

We are going to follow this steps :[https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html\#install-beats](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html#install-beats)

But with a few modifications to use it on a cloud server.

### Installing ElasticSearch:

After you ssh to the server and run the usual `apt-update`. Use this to install and run elasticsearch.

```text
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.3.0-amd64.deb
sudo dpkg -i elasticsearch-7.3.0-amd64.deb
sudo /etc/init.d/elasticsearch start
```

Once done you can check if its working by running. \`

```text
curl http://127.0.0.1:9200
```

You should get the following result.

![](../../../.gitbook/assets/image%20%2863%29.png)

### Installing Kibana:

Go to the home folder and run the following commands:

```text
curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-7.3.0-linux-x86_64.tar.gz
tar xzvf kibana-7.3.0-linux-x86_64.tar.gz
cd kibana-7.3.0-linux-x86_64/
```

