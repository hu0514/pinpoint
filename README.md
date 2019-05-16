本实例依据官网所提供源码及docker镜像运行

1下载docker-compose文件

  git clone https://github.com/naver/pinpoint-docker.git
2 运行docker容器

  cd pinpoint-docker
  
  docker-compose pull
  
  docker-compose up -d


服务端容器

pinpoint-web pinpoint服务器ui界面 入口：ip:8079

pinpoint-collector pinpoint数据收集器 收集客户端数据

pinpoint-hbase pinpoint数据存储器 存储数据 ui界面：ip:16010

客户端容器

pinpoint-agent 客户端工具

pinpoint-quickstart java测试实例 ui界面：ip：8000

报警必须容器

pinpoint-mysql

聚合图形必须容器

pinpoint-flink-jobmanager ui界面：ip：8081

pinpoint-flink-taskmanager

pinpoint-zookeeper

3要成功生成聚合图形，还需手动编译jar包并上传到flink(编译需提前配置环境 java6 java7 java8 java9+maven)

下载pinpoint源码

git clone https://github.com/naver/pinpoint.git

cd pinpoint-master/flink/src/main/resources

修改pinpoint-flink.properties


flink.cluster.enable=true

flink.cluster.zookeeper.address=zoo1

flink.cluster.zookeeper.sessiontimeout=3000

flink.cluster.zookeeper.retry.interval=5000
flink.cluster.tcp.port=19994



flink.StreamExecutionEnvironment=server


修改hbase-properties



hbase.client.host=pinpoint-hbase

hbase.client.port=2181


编译jar包 

回到pinpoint-master主目录

./mvnw install -DskipTests=true

编译成功后在pinpoint-master/flink/target/会生成pinpoint-flink-job-$(version).jar包 

上传jar包到flink

浏览器打开ip:8081

点击左下角 Submit new job 然后点击add new 选中编译生成的pinpoint-flink-job-$(version).jar 点击upload

上传成功后 选中上传的jar包前的选项框 点击出现的submit绿色按钮 

操作完成后左边框running jobs后会显示运行中的job 至此 图像聚合操作完成











