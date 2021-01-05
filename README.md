1,picocli-shell-jline3 java实现CLI工具 https://github.com/remkop/picocli/tree/master/picocli-shell-jline3

2,openoffice java实现office文件的在线预览 https://gitee.com/kekingcn/file-online-preview
oshi+hutool 获取计算机硬件的信息，包括cpu的使用率，内存使用率，cpu温度等。

3,PlantUML+Graphviz 画UML图 1，idea安装PlantUML integration插件，2，安装Graphviz，很多版本不支持，使用2.38版本 http://www.softpedia.com/dyn-postdownload.php/a338a4f848658e9aa4711a778f4ee4f0/5ab4c732/1e6eb/4/1 3，配置系统变量 添加系统变量
变量名：GRAPHVIZ_DOT 
变量值：D:\Application\graphviz-2.38\release\bin\dot.exe 

4,开源对象存储，类似aws s3，MinIO  https://gitee.com/mirrors/minio?utm_source=alading&utm_campaign=repo，在项目中用来做分布式文件服务器

5, hutool 工具包 https://www.hutool.cn/docs/#/文档说明 基本上java开发过程中会用到的工具类在这个里面都能够得到。

6，mapstruct dto po对象之前的相互转化

7，对性能要求比较高的队列可以使用disruptor

8, Knife4j 和swagger整合，提供了页面话的api管理工具，可以和spring boot很好的整合。 https://doc.xiaominfo.com/knife4j/documentation/get_start.html

9，可以内嵌到项本身，无需额外的处理的k-v数据库 JE https://blog.csdn.net/liuzhixiong_521/article/details/78839182

10， 在使用spring boot的时候在一些场景中我们需要在容器初始化的时候做一些事情，这个我们实现的方法有，1，增加ApplicationRunner，CommandLineRunner对象 2，监听ApplicationReadyEvent的事件

11,在一些邮件，html获取其他的文件中，要想起使用模板 freemarker

12，jdk1.8以后提供了CompletableFuture用于异步编程的方法，使用起来确实非常的方便 主要的用法有 supplyAsync/runAsync用于构造，thenApplyAsync用于对结构进行转化
thenAccept 用于消费，thenCombine用于对多个结果进行整合。

13，在实际协同开发的过程中使用ecolinker管理api，在ecolinker中可以管理，和mockapi接口。

14，在这个地址可以测试是否支持IPV6 http://test-ipv6.com/

15,在pox的导入和解析的过程中，对大型的文件我们往往内存溢出，我们可以使用SXSSFWorkbook读写
16，尝试使用gitbook来写笔记和写书，这是一个非常好的方式 https://www.jianshu.com/p/0388d8bb49a7



