# openshift mongodb 群集

### 一、replica-set模式：
```
cd replica-set
docker build -t registry.dataos.io/liuliu/mongo-replica-20161010 .
```
 
##### 1.修改mongo-replica-rs1.yaml中镜像地址(改成dockerfile的镜像地址)

##### 2.创建mongodb replica-set群集编排

```
oc create -f mongo-replica-rs1.yaml
```
#### 以下是三种测试方案：

##### 3-1.直接每个pod登录测试
```
oc rsh <podID1> bash
oc rsh <podID2> bash
oc rsh <podID3> bash
```

##### 3-2.创新一个mongo客户端进行测试
```
oc create -f mongo-client.yaml

oc rsh <podID> bash

mongo  --host my_replica_set1/mongo-replica-nodea-0:27017,mongo-replica-nodea-1:27017,mongo-replica-nodea-2:27017 admin     #连接测试replica-set
```

##### 3-3. 下面我就以node.js利用rrestjs框架 和 node-mongodb-native 模块进行mongodb副本集的操作（未实验）

https://github.com/christkv/node-mongodb-native/blob/master/docs/replicaset.md


### 会使用到的测试命令：

```
#先进入设置账号密码的库
use admin;
#创建账号密码
db.createUser({user:'mongo',pwd:'mongodbpass',roles:['userAdminAnyDatabase','dbAdminAnyDatabase']})
#查看创建的用户：
show users;

#插入数据
db.test.insert({Name: "test"})
db.test.find();

# 连接从节点时，需要先
rs.slaveOk();

#如果设置了密码，需要验证一下
db.auth('mongo','mongodbpass')

```

二、mongodb sharding模式（正在测试中）

