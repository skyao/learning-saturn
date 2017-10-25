# 类AbstractSaturnJob

类AbstractSaturnJob继承自AbstractElasticJob

```java
public abstract class AbstractSaturnJob extends AbstractElasticJob {}
```

## 主要方法

### executeJob()方法

去除细节，主要代码如下：

```java
protected final void executeJob(......) {
	Map<Integer, SaturnJobReturn> handleJobMap = handleJob(saturnContext);

    for (int item : saturnContext.getShardingItems()) {
		updateExecuteResult(retMap.get(item), saturnContext, item);
    }
}
```

就是调用handleJob(),得到处理结果，最后调用updateExecuteResult()方法更新处理结果。

### doExecution()方法

doExecution()方法在AbstractSaturnJob中只有定义，没有调用。

```java
	public abstract SaturnJobReturn doExecution(String jobName, Integer key, String value,
			SaturnExecutionContext shardingContext, JavaShardingItemCallable callable) throws Throwable;
```
