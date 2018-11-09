SQL数据查询：使用sql查询编辑次数最多的10个page

![](/assets/sqlimply.png)

HTTP POST数据查询

命令：curl -L -H’Content-Type: application/json’ -XPOST –data-binary @quickstart/wikiticker-top-pages.json [http://localhost:8082/druid/v2?pretty](http://localhost:8082/druid/v2?pretty)

结果：

\[ {

"timestamp" : "2016-06-27T00:00:11.080Z",

"result" : \[ {

```
"edits" : 29,

"page" : "Copa América Centenario"
```

}, {

```
"edits" : 16,

"page" : "User:Cyde/List of candidates for speedy deletion/Subpage"
```

},

..........

{

```
"edits" : 8,

"page" : "World Deaf Championships"
```

} \]

} \]

可视化控制台

overlord 控制页面：http://10.5.24.138:8090/console.html.

druid集群页面：http://10.5.24.138:8081

数据可视化页面：http://10.5.24.139:9095

