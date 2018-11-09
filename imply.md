druid的核心成员成立了一个叫imply.io的公司。我就直接下载了imply.io。

它是四个软件的集成。包括druid,plywood,tranquility,pivot.安装过程比较简单。很好的。

https://imply.io/get-started

$ bin/supervise  -c conf/supervise/quickstart.conf

导入数据，quickstart/wikipedia-index.json这个文件是自带的，可能名字不相同，该文件中的数据，可以打开看看

$ ./bin/post-index-task --file quickstart/wikipedia-index.json

可以看到，没找到服务service,过了一会又找到了，此时打开http://localhost:9095/datasets/，可以看到

![](/assets/imply.png)

