SQL数据查询：使用sql查询编辑次数最多的10个page





HTTP POST数据查询

命令：curl -L -H’Content-Type: application/json’ -XPOST –data-binary @quickstart/wikiticker-top-pages.json http://localhost:8082/druid/v2?pretty 

结果：



\[ {

  "timestamp" : "2016-06-27T00:00:11.080Z",

  "result" : \[ {

    "edits" : 29,

    "page" : "Copa América Centenario"

  }, {

    "edits" : 16,

    "page" : "User:Cyde/List of candidates for speedy deletion/Subpage"

  },

  ..........

  {

    "edits" : 8,

    "page" : "World Deaf Championships"

  } \]

} \]

