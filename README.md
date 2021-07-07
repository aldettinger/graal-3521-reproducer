# code-with-quarkus Project

This project reproduces graal issue https://github.com/oracle/graal/issues/3521.

## Reproducing the issue

Simply run mvn clean package -P native, the stack trace below should be logged:
```
Fatal error:java.lang.NullPointerException
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:490)
	at java.base/java.util.concurrent.ForkJoinTask.getThrowableException(ForkJoinTask.java:603)
	at java.base/java.util.concurrent.ForkJoinTask.get(ForkJoinTask.java:1006)
	at com.oracle.svm.hosted.NativeImageGenerator.run(NativeImageGenerator.java:499)
	at com.oracle.svm.hosted.NativeImageGeneratorRunner.buildImage(NativeImageGeneratorRunner.java:370)
	at com.oracle.svm.hosted.NativeImageGeneratorRunner.build(NativeImageGeneratorRunner.java:531)
	at com.oracle.svm.hosted.NativeImageGeneratorRunner.main(NativeImageGeneratorRunner.java:119)
	at com.oracle.svm.hosted.NativeImageGeneratorRunner$JDK9Plus.main(NativeImageGeneratorRunner.java:568)
Caused by: java.lang.NullPointerException
	at java.base/java.util.concurrent.ConcurrentHashMap.putVal(ConcurrentHashMap.java:1011)
	at java.base/java.util.concurrent.ConcurrentHashMap.putIfAbsent(ConcurrentHashMap.java:1541)
	at com.oracle.svm.reflect.serialize.SerializationSupport.addConstructorAccessor(SerializationSupport.java:119)
	at com.oracle.svm.reflect.serialize.hosted.SerializationBuilder.addConstructorAccessor(SerializationFeature.java:313)
	at com.oracle.svm.reflect.serialize.hosted.SerializationFeature.lambda$duringSetup$1(SerializationFeature.java:115)
	at com.oracle.svm.core.configure.SerializationConfigurationParser.parseAndRegister(SerializationConfigurationParser.java:54)
	at com.oracle.svm.hosted.config.ConfigurationParserUtils.doParseAndRegister(ConfigurationParserUtils.java:133)
	at com.oracle.svm.hosted.config.ConfigurationParserUtils.lambda$parseAndRegisterConfigurations$3(ConfigurationParserUtils.java:120)
	at java.base/java.util.stream.ReferencePipeline$4$1.accept(ReferencePipeline.java:212)
	at com.oracle.svm.hosted.config.ConfigurationParserUtils$1.tryAdvance(ConfigurationParserUtils.java:109)
	at java.base/java.util.Spliterator.forEachRemaining(Spliterator.java:326)
	at java.base/java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:658)
	at java.base/java.util.stream.ReferencePipeline$7$1.accept(ReferencePipeline.java:274)
	at java.base/java.util.ArrayList$ArrayListSpliterator.forEachRemaining(ArrayList.java:1655)
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:484)
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:474)
	at java.base/java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:312)
	at java.base/java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:734)
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:484)
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:474)
	at java.base/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:913)
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base/java.util.stream.IntPipeline.reduce(IntPipeline.java:491)
	at java.base/java.util.stream.IntPipeline.sum(IntPipeline.java:449)
	at com.oracle.svm.hosted.config.ConfigurationParserUtils.parseAndRegisterConfigurations(ConfigurationParserUtils.java:126)
	at com.oracle.svm.reflect.serialize.hosted.SerializationFeature.duringSetup(SerializationFeature.java:122)
	at com.oracle.svm.hosted.NativeImageGenerator.lambda$setupNativeImage$18(NativeImageGenerator.java:915)
	at com.oracle.svm.hosted.FeatureHandler.forEachFeature(FeatureHandler.java:71)
	at com.oracle.svm.hosted.NativeImageGenerator.setupNativeImage(NativeImageGenerator.java:915)
	at com.oracle.svm.hosted.NativeImageGenerator.doRun(NativeImageGenerator.java:580)
	at com.oracle.svm.hosted.NativeImageGenerator.lambda$run$2(NativeImageGenerator.java:495)
	at java.base/java.util.concurrent.ForkJoinTask$AdaptedRunnableAction.exec(ForkJoinTask.java:1407)
	at java.base/java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:290)
	at java.base/java.util.concurrent.ForkJoinPool$WorkQueue.topLevelExec(ForkJoinPool.java:1020)
	at java.base/java.util.concurrent.ForkJoinPool.scan(ForkJoinPool.java:1656)
	at java.base/java.util.concurrent.ForkJoinPool.runWorker(ForkJoinPool.java:1594)
	at java.base/java.util.concurrent.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:183)
Error: Image build request failed with exit status 1
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
```
