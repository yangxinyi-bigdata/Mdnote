# Flume 使用记录

Flume 使用记录

安装好之后 flume-ng version 查看

```shell
#Asingle-nodeFlumeconfiguration


#Namethecomponentsonthisagent

a1.sources=r1

a1.sinks=k1

a1.channels=c1



#Describe/configurethesource

a1.sources.r1.type=avro

a1.sources.r1.channels=c1

a1.sources.r1.bind=0.0.0.0

a1.sources.r1.port=4141



#Describethesink

a1.sinks.k1.type=logger



#Useachannelwhichbufferseventsinmemory

a1.channels.c1.type=memory

a1.channels.c1.capacity=1000

a1.channels.c1.transactionCapacity=100



#Bindthesourceandsinktothechannel

a1.sources.r1.channels=c1

a1.sinks.k1.channel=c1
```

未生效代码

```shell
flume-ngagent-c.-f/home/hduser/flume/conf/avro.conf-na1-Dflume.root.logger=INFO,console
```

更改后生效代码

```shell
flume-ngagent--conf~/flume/conf--conf-file~/flume/conf/avro.conf--namea1-Dflume.root.logger=INFO,console
```

发现更改内容: 

conf 之前指定的是默认目录, 后来更改为指定目录

发送文本信息代码

```shell
flume-ngavro-client--confconf-Hlocalhost-p4141-F/home/hduser/flume/log.00
```

使用NetCat作为输入源的相关操作，NetCat作为一款轻量级数据源发生装置，常用于测试数据输入管道是否通畅.

```shell
#Namethecomponentsonthisagent

a1.sources=r1

a1.sinks=k1

a1.channels=c1



#Describe/configurethesource

a1.sources.r1.type=netcat

a1.sources.r1.bind=0.0.0.0

a1.sources.r1.port=44444



#Describethesink

a1.sinks.k1.type=logger



#Useachannelwhichbufferseventsinmemory

a1.channels.c1.type=memory

a1.channels.c1.capacity=1000

a1.channels.c1.transactionCapacity=100



#Bindthesourceandsinktothechannel

a1.sources.r1.channels=c1

a1.sinks.k1.channel=c1
```

启动Flume:

```shell
flume-ngagent--conf~/flume/conf--conf-file~/flume/conf/multiplexing.conf--namea1-Dflume.root.logger=INFO,console
```

打开数据发送端

`telnet localhost 44444`

或者 netcat localhost 44444

 