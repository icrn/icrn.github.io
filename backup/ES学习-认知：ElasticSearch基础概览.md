
## 什么是ElasticSearch

```ElasticSearch是一款非常强大的、基于Lucene的开源搜索及分析引擎；它是一个实时的分布式搜索分析引擎，它能让你以前所未有的速度和规模，去探索你的数据。```

它在用作全文检索、结构化搜索、分析以及这三个功能的组合：
* Wikipedia使用ElasticSearch提供代有高亮片段的全文搜索，还有Search-as-you-type和didi-you-mean的建议。
* 卫报使用ElasticSearch将网络社交数据结合到访客日志去，为它的编辑们提供公众对于新文章的实时反馈。
* stack Overflow将地理位置查询融入全文检索中去，并且使用more-like-this接口去查找相关的问题和回答。
* Github使用ElasticSearch对1300亿行代码进行查询
* ……

除了搜索，结合Kibana、Logstash、Beats开源产品，Elastic Stack（简称ELK）还被广泛运用在大数据近实时分析领域，包括：日志分析、指标监控、信息安全灯。它可以帮助你探索海量结构化、非结构化，按需创建可视化报表，对监控数据设置报警阈值，通过使用机器学习，自动识别异常状态。

ElasticSearch是基于Restful WebAPI，使用Java语言开发的搜索引擎库类，并作为APache许可条款下的开放源码发布，是当前流行企业级搜索引擎。其客户端在Java、C#、PHP、Python灯许多语言中都是可用的。


## ElasticSearch的由来
```ElasticSearch背后的小故事```
许多年前，一个刚结婚的名为Shay Banon的失业开发者，跟着他的妻子去了伦敦，他的妻子在那学习厨师。在寻找一个赚钱的工作的试试，为了给他的妻子做一个食谱搜索引擎，他开始使用Lucene的一个早期版本。

直接使用lucene是河南的，因为shay开始做一个抽象层，Java开发者使用它可以很简单的给他们的程序添加搜索功能。他发布了他的第一个开源项目Compass。

后来shay获得了一份工作，主要是高性能，分布式环境下的内存数据网络。这个对于高性能，实时，分布式搜索引擎的需求尤为突出，他决定重写Compass，把它变成一个独立的服务并取名为ElasticSearch。

第一个公开版本在2010年2月发布，从此以后，ElasticSearch已经成为Github上最活跃的项目之一，它拥有超过300名contributors。一家公司一开始围绕ElasticSearch提供商业服务，并开始新的特性。但是ElasticSearch将永远开源并对所有人可用。

据说，shay的妻子还在等着她的食谱搜索引擎……


## 为什么不是直接使用Lucene
```ElasticSearch是基于Lucene的，那么为什么不是直接使用Lucene呢？```
Lucene可以说是当下最先进、高性能、全功能的搜索引擎库。

但是Lucene仅仅只是一个库。为了充分发挥其功能，你需要使用Java并将Lucene直接集成到应用程序中。更糟糕的是，您可能需要获得信息检索学位才能了解其工作原理。Lucene非常复杂。

ElasticSearch也是使用Java编写的，它的内部使用Lucene做索引和搜索，但是它的目的是使全文检索变得简单，通过隐藏Lucene的复杂性，取而代之的是提供一套简单一致的RESTful API。

然而，ElasticSearch不仅仅是Lucene，并且也不仅仅是一个全文搜索引擎。他可以被下面准确的形容：

* 一个分布式的实时文档存储，每个字段可以被索引和检索
* 一个分布式实时分析搜索引擎
* 能胜任上百个服务节点的扩展，并支持PB级别的结构化或者非结构数据

ElasticSearch的主要功能及应用场景
```我们在哪些场景下可以使用ES呢```
* 主要功能：
    *  海量数据的分布式存储以及集群管理，达到了服务与数据的高可用以及水平扩展：
    * 近实时搜索，性能卓越。对结构化、全文、地理位置等类型数据的处理；
    * 海量数据的近实时分析（聚合功能）
* 应用场景：
    * 网络搜索、垂直搜索、代码搜索；
    * 日志管理与分析、安全指标监控、应用性能监控、Web抓取舆情分析；

## ElasticSearch的基础概念
```我们还需要对比结构化数据库，看看ES的基础概念，为我们后面学习做铺垫```
* Near Realtime(NRT)近实时。数据提交索引后，立马就可以搜索到。
* Cluster集群，一个集群由一个唯一的名字标识，默认为”elasticsearch“。集群名称非常重要，具有相同集群名的节点才会组成一个集群。集群名称在配置文件中指定。
* Node节点：存储集群的数据，参与集群的索引和搜索功能。像集群有名字，节点也有名字，节点也有自己的名称，默认在启动时会以一个随机UUID的前七个字符作为节点的名称，你以为其指定任意的名字。通过集群名在网络中发现同伴组成集群。一个节点也可以是集群。
* Index索引：一个索引是一个文档的集合（等同于solr中的集合）。每一个索引都有唯一的名字，通过这个名字来操作它。一个急群众可以有任意多的索引。
*Type类型：指在一个索引中，可以索引不同类型的文档，如用户数据、博客数据。从6.0.0版本起被废弃，一个索引中只存放一类数据。
*  Document文档：被索引的一条数据，索引的基本信息单元，以JSON格式来表示。
* Shard分片：在创建一个索引时可以指定分成多少个分片存储。每个分片本身也是一个功能完善且独立的”索引“，可以被放置在集群的任意节点上。
* Replication备份：一个分片可以有多个备份（副本）。


为了方便理解，做一个ES和数据库的对比：
| RDBMS | ElasticSearch | 
| ------ | ------ | 
| 数据库（database) | 索引（Index） | 
| 表（table） | 类型（type） | 
| 行（row） | 文档（document） | 
| 列（column） | 字段（field） | 
| 表结构（scheme） | 映射（mapping） | 
| 索引 | 反向索引 | 
| SQL | 查询DSL | 
| select * from table | GET http://…… | 
| update table set | PUT http://…… | 
| delete from table | DELETE http://…… | 


## 参考文章
[你知道的, 为了搜索…​](https://www.elastic.co/guide/cn/elasticsearch/guide/current/intro.html)
[基础入门](https://www.elastic.co/guide/cn/elasticsearch/guide/current/getting-started.html)
[elasticsearch系列一：elasticsearch（ES简介、安装&配置、集成Ikanalyzer）](https://www.cnblogs.com/leeSmall/p/9189078.html)

