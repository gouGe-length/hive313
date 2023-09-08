# Hive313、Hadoop336、Spark341
## 使用JDK1.8 、Maven3.8.4
## 主要目的是想替换Hive的执行引擎为Spark
## 下面是官方源码重新编译的教程（基本不用看了，源码和提供的包已经处理好了），巨多坑，反正整个整合过程不容易
需要先下载Spark的源码看看Hadoop的版本，log的版本，Scala的版本   
然后再下载对应版本的Hadoop版本，找找share包里面的列举的jar包版本：log、Guava

### 下载Spark源码 `https://archive.apache.org/dist/spark/spark-3.4.1/`
### 下载Hive313源码 `https://dlcdn.apache.org/hive/hive-3.1.3/`
### 下载Hadoop336包（下载官方编译执行包就行） `https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/`

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

## Hive On Spark
### 准备资源
* 下载处理好的Hive313源码或者编译好的包。
* 下载处理好的Spark源码或者编译好的包，得到一份不带hive的版本Spark，主要目的是将Jars/*.jar上传到HDFS中，供其余节点共享使用。
* 下载Spark官方提供的with-Hadoop版本，目的是作为运行节点。
* 下载Hadoop。
* 准备好Jdbc驱动，用于Hive存储元数据到DBMS中，如：Mysql；提供的Hive包已经包含。
### 配置
配置文件参考：`https://app.yinxiang.com/Home.action?referralCode=ebcc-organic-yxbj&login=true#n=b9d55f37-7bb5-41b5-ad68-3a39251bb459&s=s70&ses=4&sh=2&sds=5&`   
处理Spark冲突（提供的包已经处理好），用hive lib目录下面的guava替换掉spark-with-Hadoop中jars下面的guava   
处理Hive与Hadoop冲突（提供的包已经处理好）：需要删除Hive中的log4j-slf4j-impl-2.17.1.jar
