# Hive313、Hadoop334、Spark341

## 主要目的是想替换Hive的执行引擎为Spark
需要先下载Spark的源码看看Hadoop的版本，log的版本，Scala的版本   
然后再下载对应版本的Hadoop版本，找找share包里面的列举的jar包版本：log、Guava

### 下载Spark源码 `https://archive.apache.org/dist/spark/spark-3.4.1/`
### 下载Hive313源码 `https://dlcdn.apache.org/hive/hive-3.1.3/`
### 下载Hadoop334包（下载官方编译执行包就行） `https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/`

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
