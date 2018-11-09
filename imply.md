druid的核心成员成立了一个叫imply.io的公司。我就直接下载了imply.io。

它是四个软件的集成。包括druid,plywood,tranquility,pivot.安装过程比较简单。很好的。

[https://imply.io/get-started](https://imply.io/get-started)

$ bin/supervise  -c conf/supervise/quickstart.conf

导入数据，quickstart/wikipedia-index.json这个文件是自带的，可能名字不相同，该文件中的数据，可以打开看看

$ ./bin/post-index-task --file quickstart/wikipedia-index.json

可以看到，没找到服务service,过了一会又找到了，此时打开[http://localhost:9095/datasets/，可以看到](http://localhost:9095/datasets/，可以看到)

![](/assets/imply.png)Imply提供了一套完整的部署方式，包括依赖库，Druid，图形化的数据展示页面，SQL查询组件等。

目录说明如下： 

- bin/ - run scripts for included software. 

- conf/ - template configurations for a clustered setup. 

- conf-quickstart/\* - configurations for the single-machine quickstart. 

- dist/ - all included software. 

- quickstart/ - files related to the single-machine quickstart.

导入测试数据

安装包中包含一些测试的数据,可以通过执行预先定义好的数据说明文件进行导入



bin/post-index-task --file quickstart/wikiticker-index.json

1

可视化控制台

overlord 控制页面：http://localhost:8090/console.html.

druid集群页面：http://localhost:8081

数据可视化页面：http://localhost:9095

数据展示与查询

数据展示：对渠道进行统计的柱状图



