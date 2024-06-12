# kafka-Ubuntu


Kafka install
=====================

Step 1: Update your System

sudo apt update && sudo apt upgrade –y

Step 2: Install Stable Java Version

sudo apt install openjdk-11-jre-headless -y

java -version

Step 3: Download latest Apache Kafka

wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz


Step 4: Extract Kafka

sudo mkdir /usr/local/kafka-server
sudo tar -xzf kafka_2.13-3.7.0.tgz
sudo mv kafka_2.13-3.7.0/* /usr/local/kafka-server/

Step 5: Configure Zookeeper

sudo nano /etc/systemd/system/zookeeper.service
--------------------
[Unit]


Description=Apache Zookeeper Server


Requires=network.target remote-fs.target


After=network.target remote-fs.target


[Service]


Type=simple


ExecStart=/usr/local/kafka-server/bin/zookeeper-server-start.sh /usr/local/kafka-server/config/zookeeper.properties


ExecStop=/usr/local/kafka-server/bin/zookeeper-server-stop.sh


Restart=on-abnormal


[Install]


WantedBy=multi-user.target
--------------------------

Step 5: Configure Kafka

sudo nano /etc/systemd/system/kafka.service

-----------------------
Unit]


Description=Apache Kafka Server


Documentation=http://kafka.apache.org/documentation.html


Requires=zookeeper.service


After=zookeeper.service


[Service]


Type=simple


Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"


ExecStart=/usr/local/kafka-server/bin/kafka-server-start.sh /usr/local/kafka-server/config/server.properties


ExecStop=/usr/local/kafka-server/bin/kafka-server-stop.sh


Restart=on-abnormal


[Install]


WantedBy=multi-user.target-
-----------------------

sudo systemctl daemon-reload

Step 6: Start Zookeeper and Kafka service

sudo systemctl enable --now zookeeper.service
sudo systemctl enable --now kafka.service
sudo systemctl status kafka zookeeper

Create a Kafka Topic
==========================================

cd /usr/local/kafka-server

bin/kafka-topics.sh --create --topic test_topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1


Publish Messages to the Kafka Topic
====================================

bin/kafka-console-producer.sh --topic test_topic --bootstrap-server localhost:9092

*This command starts a producer console where you can type messages. Type your messages and press Enter after each message to publish them to the test_topic topic.


Consume Messages from the Kafka Topic

bin/kafka-console-consumer.sh --topic test_topic --from-beginning --bootstrap-server localhost:9092
