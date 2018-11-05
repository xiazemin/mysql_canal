整个canal流程处理binlog，分为这么5步操作\(4+1\)，整个优化和评测对这5步分别进行.

![](/assets/canal构架.png)

构造了两个测试场景，批量Insert/Update/Delete 和 普通业务DB\(单条操作为主\)

