## azkaban

### version  
- 3.0.0  
- 该版本增加了对服务器资源的控制，定时读取/proc/meminfo  

### 问题
- 执行任务出错  
```
java.lang.Exception: java.lang.Exception: Cannot request memory (Xms 64 kb, Xmx 65536 kb) from system for job testjava
	at azkaban.jobtype.HadoopJavaJob.run(HadoopJavaJob.java:190)
	at azkaban.execapp.JobRunner.runJob(JobRunner.java:590)
	at azkaban.execapp.JobRunner.run(JobRunner.java:443)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
Caused by: java.lang.Exception: Cannot request memory (Xms 64 kb, Xmx 65536 kb) from system for job testjava
	at azkaban.jobExecutor.ProcessJob.run(ProcessJob.java:86)
	at azkaban.jobtype.HadoopJavaJob.run(HadoopJavaJob.java:187)
	... 7 more
```  
azkaban-exe 要求服务器内存空闲必须大于3G 否则任何任务执行出错，并且不知此参数配置   
```
public synchronized static boolean canSystemGrantMemory(long xms, long xmx, long freeMemDecrAmt) {
    if (!memCheckEnabled) {
      return true;
    }

    //too small amount of memory left, reject
    if (freeMemAmount < LOW_MEM_THRESHOLD) {
      logger.info(String.format("Free memory amount (%d kb) is less than low mem threshold (%d kb),  memory request declined.",
              freeMemAmount, LOW_MEM_THRESHOLD));
      return false;
    }
```  
```
boolean isMemGranted =
          SystemMemoryInfo.canSystemGrantMemory(memPair.getFirst(),
              memPair.getSecond(), freeMemDecrAmt);
      if (!isMemGranted) {
        throw new Exception(
            String
                .format(
                    "Cannot request memory (Xms %d kb, Xmx %d kb) from system for job %s",
                    memPair.getFirst(), memPair.getSecond(), getId()));
      }
```  


