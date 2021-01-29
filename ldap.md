## LDAP服务器，以前在开发过程中陆续听过LDAP服务器，但是一直都没有使用过。这次英文工作的需要，大致的了解了一下。
# LDAP
 ## 目录服务
目录是一个为查询、浏览和搜索而优化的专业分布式数据库，它呈树状结构组织数据，就好象Linux/Unix系统中的文件目录一样。目录数据库和关系数据库不同，它有优异的读性能，但写性能差，并且没有事务处理、回滚等复杂功能，不适于存储修改频繁的数据。所以目录天生是用来查询的，就好象它的名字一样。

目录服务是由目录数据库和一套访问协议组成的系统。类似以下的信息适合储存在目录中：
   - 企业员工信息，如姓名、电话、邮箱等；
   - 公用证书和安全密钥
   - 公司的物理设备信息，如服务器，它的IP地址、存放位置、厂商、购买时间等；
 ## LDAP的特点
   - LDAP的结构用树来表示，而不是用表格。正因为这样，就不能用SQL语句了
   - LDAP可以很快地得到查询结果，不过在写方面，就慢得多
   - LDAP提供了静态数据的快速查询方式
   - Client/server模型，Server 用于存储数据，- Client提供操作目录信息树的工具
 ## LDAP的数据组织方式
- ![数据表示](./images/ldap_01.PNG)

 ## LDAP数据基本概念
 - ### 条目 Entry
    条目，也叫记录项，是LDAP中最基本的颗粒，就像字典中的词条，或者是数据库中的记录。通常对LDAP的添加、删除、更改、检索都是以条目为基本对象的。

    - dn 每一个条目都有一个唯一的标识名（distinguished Name)上图中的dn可以表示为(uid=babs,ou=People,dc=example,dc=com)
    - rdn 一般只dn逗号最左边的位置（uid=babs）
    - Base DN LDAP目录树的最顶部就是根，也就是所谓的“Base DN" "dc=net,dc=com,dc=DE"
  
- ### schema
    schema 用于控制目录树中各种条目所拥有的对象类以及各种属性的定义，并通过自身内部规范机制限定目录树条目所遵循的逻辑结构以及定义规范，保证整个目录树没有非法条目数据。schema 用来指定一个条目所包含的对象类（objectClass）以及每一个对象类所包含的属性值（attribute value）

- ###  objectClass 
    每个条目必须包含一个属于自身条件的对象类，然后再定义其条目属性及对应的值。


- ### 属性（Attribute）
    在目录树中主要用于描述条目相关信息，属性由objectClass 所控制，一个objectClass 的节点具有一系列Attribute

    常见的属性
    - c：国家。
    - cn：common name，指一个对象的名字。如果指人，需要使用其全名。
    - dc：domain Component，常用来指一个域名的一部分。
    - givenName：指一个人的名字，不能用来指姓。
    - l：指一个地名，如一个城市或者其他地理区域的名字。
    - mail：电子信箱地址。
    - o：organizationName，指一个组织的名字。
    - ou：organizationalUnitName，指一个组织单元的名字。
    - sn：surname，指一个人的姓。
    - telephoneNumber：电话号码，应该带有所在的国家的代码。
    - uid：userid，通常指某个用户的登录名，与Linux系统中用户的uid不同。

## LDIF（LDAP Data Interchanged Format）数据交换格式
 LDIF（LDAP Data Interchanged Format）的轻量级目录访问协议数据交换格式。通常用来交换数据并在OpenLDAP服务器之间互相交换数据，并且可以通过LDIF 实现数据文件的导入、导出以及数据文件的添加、修改、重命名等操作，这些信息需要按照LDAP 中schema 的规范进行操作，并会接受schema 的检查，如果不符合OpenLDAP schema 规范要求，则会提示相关语法错误。

 ```
 dn: uid=water,ou=people,dc=wzlinux,dc=com     #DN 描述项，在整个目录树上为唯一的  
objectClass: top 
objectClass: posixAccount  
objectClass: shadowAccount  
objectClass: person  
objectClass: inetOrgPerson  
objectClass: hostObject  
sn: zuo  
cn: zuoyang  
telephoneNumber：xxxxxxxxxxx  
mail: xxxx@126.com
 ```
## LDAP一些产品

|厂商|产品|特点|
| --- | --- | --- |
| sun | sunone directory server | 基于文本数据库的存储，速度快 |
| ibm | ibm directory server | 基于db2数据库存储，速度一般 |
| oracle | oracle internet directory | 基于oracle数据库存储，速度一般 |
| microsoft | microsoft active directory | 基于windows系统用户，数据管理和权限管理不灵活 |

## LDAP环境的搭建
  ### openLdap服务的安装
  demo采用docker安装openLdap的方式，如果没有安装docker，实现安装docker
  
  ```
    # 安装 docker 和 docker-compose
    sudo apt-get remove docker docker-engine docker.io containerd runc
    sudo apt-get update
    sudo apt-get install  -y \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io -y
 ```

pull openLdap镜像

```
docker pull osixia/openldap:1.2.2
```

启动容器，初始化数据，初始化数据的ldif在assert/data.ldif

启动镜像
```
# 创建并进入映射目录
mkdir -p /usr/local/ldap && cd /usr/local/ldap

# 启动容器
docker run  -d -p 389:389  -p 636:636  -v /usr/local/ldap:/usr/local/ldap  --name ldap  osixia/openldap:1.2.2

docker exec -it ldap /bin/bash  

添加数据 

ldapadd -x -H ldap://127.0.0.1:389 -D "cn=admin,dc=example,dc=org" -w admin -f /home/dengrixiong/ldap/data.ldif 

```
