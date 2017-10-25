# 类AbstractElasticJob

弹性化分布式作业的基类.

## 类定义

```java
public abstract class AbstractElasticJob implements Stopable {}
```

## 类属性

### 状态值

Job的四个状态：

```java
private boolean stopped = false;

private boolean forceStopped = false;

private boolean aborted = false;

protected boolean running;
```

以及重置状态的reset()方法，将状态重置为running=true：

```java
private void reset() {
    stopped = false;
    forceStopped = false;
    aborted = false;
    running = true;
}
```

Stopable接口定义的几个方法实现，都是简单设置这几个属性：

```java
@Override
public void stop() {
    stopped = true;
}

@Override
public void forceStop() {
    forceStopped = true;
}

@Override
public void abort() {
    aborted = true;
}

@Override
public void resume() {
    stopped = false;
}
```

### 作业信息

```java
protected String jobName;
protected String namespace;
protected String jobVersion;
```

## 主要方法

### execute()方法

去掉细节看主流程：

```java
JobExecutionMultipleShardingContext shardingContext = null;
	shardingContext = executionContextService.getJobExecutionShardingContext();
	executeJobInternal(shardingContext);
```

基本上就一个事情，拿到shardingContext之后调用executeJobInternal()，完成一次Job的调用。

继续看executeJobInternal()方法：

```java
try {
    executeJob(shardingContext);
} finally {
	......
}
```

最后调用到抽象方法executeJob()，由子类实现。

### execute()前后作业状态变化

running状态在reset时设置为true，然后通过finally设置为false。

```java
reset();

try {
	shardingContext = executionContextService.getJobExecutionShardingContext();
	executeJobInternal(shardingContext);
} finally {
    running = false;
}
```

### execute()前后服务器状态变化

如果需要汇报服务器状态，则在Job执行前，设置服务器状态为RUNNING:

```java
executionService.registerJobBegin(shardingContext);

public void registerJobBegin(......) {
	serverService.updateServerStatus(ServerStatus.RUNNING);
}
```

执行作业之后，设置服务器状态为READY:

```java
serverService.updateServerStatus(ServerStatus.READY);
```

