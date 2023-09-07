# Hive313、Hadoop334、Spark341

### 下载Hive313源码 `https://dlcdn.apache.org/hive/hive-3.1.3/`

### 排除spark中的hadoop依赖
```
<exclusions>
    <exclusion>
      <groupId>org.apache.hadoop</groupId>
      <artifactId>hadoop-client-api</artifactId>
    </exclusion>
    <exclusion>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client-runtime</artifactId>
    </exclusion>
</exclusions>
```

### 修改SparkCounter类的 *Accumulator<Long>为LongAccumulator*
### 修改SparkCounter构造函数
```
public SparkCounter(
    String name,
    String displayName,
    String groupName,
    long initValue,
    JavaSparkContext sparkContext) {

    this.name = name;
    this.displayName = displayName;
    String accumulatorName = groupName + "_" + name;
    this.accumulator = sparkContext.sc().longAccumulator(accumulatorName);
  }
```

### 修改ShuffleWriteMetrics类构造函数
```
  public ShuffleWriteMetrics(TaskMetrics metrics) {
    this(metrics.shuffleWriteMetrics().bytesWritten(),
      metrics.shuffleWriteMetrics().writeTime());
  }
```
### 修改TestStatsUtils类的Sets引用为org.sparkproject.guava.collect的
```
import org.sparkproject.guava.collect.Sets;
```
### 修改QueryTracker类的Log依赖为 
```
import org.slf4j.*;
```
### 以及QueryTracker类的Marker获取方式为 
```
private static final Marker QUERY_COMPLETE_MARKER = MarkerFactory.getMarker(new Log4jQueryCompleteMarker().getName());
```

### 安装maven依赖（hive编译打包依赖），注意修改后面-Dfile路径
```
mvn install:install-file -DgroupId=org.pentaho -DartifactId=pentaho-aggdesigner-algorithm -Dversion=5.1.5-jhyde -Dpackaging=jar -Dfile=/home/pan/soft/pentaho-aggdesigner-algorithm-5.1.5-jhyde.jar

mvn install:install-file -DgroupId=org.codehaus.groovy -DartifactId=groovy-all -Dversion=2.4.4 -Dpackaging=jar -Dfile=/home/pan/soft/groovy-all-2.4.4.jar

mvn install:install-file -DgroupId=com.google.code.maven-replacer-plugin -DartifactId=replacer -Dversion=1.5.3 -Dpackaging=jar -Dfile=C:\Users\34514\Desktop\replacer-1.5.3.jar
```

### 因为要运行ant程序对应的sh脚本，所以在linux环境下编译，指定Profiles为dist（打包发布）
```
mvn clean -DskipTests -Pdist -Dmaven.javadoc.skip=true install
```
