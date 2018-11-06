mysql&gt; 2018-11-06T02:48:33.284644Z 139 \[Note\] Aborted connection 139 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-06T02:48:33.336231Z 140 \[Note\] Start binlog\_dump to master\_thread\_id\(140\) slave\_server\(1778384897\), pos\(mysql-bin.000003, 8143\)

在MySQL中sleep状态数百秒的而且经常重复连接是应用程序在工作后没有关闭连接的症状之一，而是依靠数据库wait\_timeout来关闭它们。强烈建议在操作结束时更改应用程序逻辑以正确关闭连接；



检查以确保max\_allowed\_packet的值足够高，并且客户端没有收到“数据包太大”消息。 这种情况他会中止连接，而不正确关闭它；



另一种可能性是TIME\_WAIT。建议您确认连接被妥善管理并且是在应用端正常关闭；



确保事务正确提交（开始和提交），以便一旦应用程序“完成”连接，它将处于“clean”的状态；



您应该确保客户端应用程序不中止连接。 例如，如果PHP的选项max\_execution\_time设置为5秒，增加connect\_timeout是没用的，因为PHP会杀死脚本。 其他编程语言和环境也有类似的选项；



连接延迟的另一个原因是DNS问题。 检查是否启用了skip-name-resolve，检查主机根据其IP地址而不是其主机名进行身份验证；



尝试增加MySQL的net\_read\_timeout和net\_write\_timeout值，看看是否减少了错误的数量。











