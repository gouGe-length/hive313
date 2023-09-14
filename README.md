# Hive313、Hadoop336、Spark341
## 使用JDK1.8 、Maven3.8.4
## 主要目的是想替换Hive的执行引擎为Spark
## 下面是源码重新编译的教程（基本不用看了，供的包已经处理好了），巨多坑，反正整个整合过程不容易

### 安装maven依赖（hive编译打包依赖），注意修改后面-Dfile路径
```
mvn install:install-file -DgroupId=org.pentaho -DartifactId=pentaho-aggdesigner-algorithm -Dversion=5.1.5-jhyde -Dpackaging=jar -Dfile=/home/pan/soft/pentaho-aggdesigner-algorithm-5.1.5-jhyde.jar

mvn install:install-file -DgroupId=org.codehaus.groovy -DartifactId=groovy-all -Dversion=2.4.4 -Dpackaging=jar -Dfile=/home/pan/soft/groovy-all-2.4.4.jar

mvn install:install-file -DgroupId=com.google.code.maven-replacer-plugin -DartifactId=replacer -Dversion=1.5.3 -Dpackaging=jar -Dfile=C:\Users\34514\Desktop\replacer-1.5.3.jar
```

### 因为要运行ant程序对应的sh脚本，所以最好在linux环境下编译，指定Profiles为dist（打包发布），windows环境可以用git bash试试
```
mvn clean -DskipTests -Pdist -Dmaven.javadoc.skip=true install
```
## 最后做拷贝
* （hive-storage-api小于2.7.3版本需要）把打包好的hive-storage-api.jar中的org.apache.hadoop.hive.ql.exec.vector包根路径下面的所有class拷贝到hive-exec.jar对应目录下；
* 再删除hive-storage-api.jar中的org.apache.hadoop.hive.ql.exec.vector包根路径下面的所有class；
* 得到最终的两个jar：hive-storage-api.jar和hive-exec.jar。

## Hive On Spark
### 准备资源
* 下载处理好的Hive313源码或者编译好的包。
* 下载Spark官方提供的with-Hadoop版本。
* 下载Hadoop。
* 下载Spark-Jars.zip。
* 准备好Jdbc驱动，用于Hive存储元数据到DBMS中，如：Mysql；提供的Hive包已经包含。

### 配置
* 配置文件参考：`https://app.yinxiang.com/Home.action?referralCode=ebcc-organic-yxbj&login=true#n=b9d55f37-7bb5-41b5-ad68-3a39251bb459&s=s70&ses=4&sh=2&sds=5&`   
* 处理Spark冲突（提供的包已经处理好），用hive lib目录下面的guava替换掉spark-with-Hadoop中jars下面的guava   
* 处理Hive与Hadoop冲突（提供的包已经处理好）：需要删除Hive中的log4j-slf4j-impl-2.17.1.jar  
* 解压Spark-Jars.zip并上传到Hdfs共享Jar给所有容器使用。   

### 注意事项
如果是Spark官方提供的with-Hadoop版本执行包，那么Hive On Spark配置时上传Jars到hdfs时需要几点注意：
* 拷贝jars目录所有jar到新的目录，然后删除所有关于Hive的包；
* 删除orc-mapreduce-1.8.4-shaded-protobuf.jar、orc-core-1.8.4-shaded-protobuf.jar、orc-shims-1.8.4.jar；
* 通过提供的Hive源码重新编译打包，按照上面提到的`最后做拷贝` 步骤得到的两个Jar拷贝替换到hive/lib目录和刚刚拷贝出来的新Jars目录中；
* 拷贝（hive编译打包依赖）目录中的orc-core-1.5.13.jar、orc-mapreduce-1.5.13.jar、orc-shims-1.5.13.jar到刚刚拷贝出来的新Jars目录中；