# 接口Stopable

可停止的作业或目标.

```java
public interface Stopable {

	/**
	 * 停止运行中的作业或目标.
	 */
	void stop();

	/**
	 * 中止作业（上报作业状态）.
	 */
	void forceStop();

	/**
	 * 恢复运行作业或目标.
	 */
	void resume();

	/**
	 * ZK断开、Executor停止时关闭作业（不上报状态）
	 */
	void abort();

	/**
	 * 关闭作业
	 */
	void shutdown();
}
```

TBD:

1. 是否上报状态的确认等待看具体实现。
2. stop/abort/shutdown的差异

