---
layout:     post
title:      "Weblogic - Server subsystem failed. Reason: A MultiException has 6 exceptions"
subtitle:   "java.lang.OutOfMemoryError: Requested array size exceeds VM limit"
date:       2019-01-28 12:00:00
author:     "Sri Battina"
header-img: "img/post-bg-01.jpg"
---

Recently while trying to debug JMS message bridge, we enabled debugging for MessagingBridge, JMS, etc. We also updated the message bridge configuration so we had to restart all servers in the domain. During this restart, one of the managed servers is failing to start with the following exception.


```

  <Jan 26, 2019, 2:24:01,674 AM CST> <Warning> <JMX> <localhost> <msosb2> <[ACTIVE] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'> <<WLS Kernel>> <> <> <1548491041674> <[severity-value: 16] [partition-id: 0] [partition-name: DOMAIN] > <BEA-149513> <JMX Connector Server stopped at service:jmx:iiop://localhost:7101/jndi/weblogic.management.mbeanservers.runtime.>
  <Jan 26, 2019, 2:24:01,733 AM CST> <Critical> <WebLogicServer> <localhost> <msosb2> <main> <<WLS Kernel>> <> <> <1548491041733> <[severity-value: 4] [partition-id: 0] [partition-name: DOMAIN] > <BEA-000386> <Server subsystem failed. Reason: A MultiException has 6 exceptions.  They are:
  1. java.lang.OutOfMemoryError: Requested array size exceeds VM limit
  2. java.lang.IllegalStateException: Unable to perform operation: post construct on weblogic.servlet.internal.WebService
  3. java.lang.IllegalArgumentException: While attempting to resolve the dependencies of weblogic.management.deploy.internal.RegisterInternalApps errors were found
  4. java.lang.IllegalStateException: Unable to perform operation: resolve on weblogic.management.deploy.internal.RegisterInternalApps
  5. java.lang.IllegalArgumentException: While attempting to resolve the dependencies of weblogic.management.deploy.internal.DeploymentPreStandbyServerService errors were found
  6. java.lang.IllegalStateException: Unable to perform operation: resolve on weblogic.management.deploy.internal.DeploymentPreStandbyServerService
   
  A MultiException has 6 exceptions.  They are:
  1. java.lang.OutOfMemoryError: Requested array size exceeds VM limit
  2. java.lang.IllegalStateException: Unable to perform operation: post construct on weblogic.servlet.internal.WebService
  3. java.lang.IllegalArgumentException: While attempting to resolve the dependencies of weblogic.management.deploy.internal.RegisterInternalApps errors were found
  4. java.lang.IllegalStateException: Unable to perform operation: resolve on weblogic.management.deploy.internal.RegisterInternalApps
  5. java.lang.IllegalArgumentException: While attempting to resolve the dependencies of weblogic.management.deploy.internal.DeploymentPreStandbyServerService errors were found
  6. java.lang.IllegalStateException: Unable to perform operation: resolve on weblogic.management.deploy.internal.DeploymentPreStandbyServerService
   
          at org.jvnet.hk2.internal.Collector.throwIfErrors(Collector.java:89)
          at org.jvnet.hk2.internal.ClazzCreator.resolveAllDependencies(ClazzCreator.java:249)
          at org.jvnet.hk2.internal.ClazzCreator.create(ClazzCreator.java:357)
          at org.jvnet.hk2.internal.SystemDescriptor.create(SystemDescriptor.java:471)
          at org.glassfish.hk2.runlevel.internal.AsyncRunLevelContext.findOrCreate(AsyncRunLevelContext.java:232)
          at org.glassfish.hk2.runlevel.RunLevelContext.findOrCreate(RunLevelContext.java:85)
          at org.jvnet.hk2.internal.Utilities.createService(Utilities.java:2020)
          at org.jvnet.hk2.internal.ServiceHandleImpl.getService(ServiceHandleImpl.java:114)
          at org.jvnet.hk2.internal.ServiceHandleImpl.getService(ServiceHandleImpl.java:88)
          at org.glassfish.hk2.runlevel.internal.CurrentTaskFuture$QueueRunner.oneJob(CurrentTaskFuture.java:1213)
          at org.glassfish.hk2.runlevel.internal.CurrentTaskFuture$QueueRunner.run(CurrentTaskFuture.java:1144)
          at org.glassfish.hk2.runlevel.internal.CurrentTaskFuture$UpOneLevel.run(CurrentTaskFuture.java:762)
          at weblogic.work.SelfTuningWorkManagerImpl$WorkAdapterImpl.run(SelfTuningWorkManagerImpl.java:666)
          at weblogic.invocation.ComponentInvocationContextManager._runAs(ComponentInvocationContextManager.java:348)
          at weblogic.invocation.ComponentInvocationContextManager.runAs(ComponentInvocationContextManager.java:333)
          at weblogic.work.LivePartitionUtility.doRunWorkUnderContext(LivePartitionUtility.java:54)
          at weblogic.work.PartitionUtility.runWorkUnderContext(PartitionUtility.java:41)
          at weblogic.work.SelfTuningWorkManagerImpl.runWorkUnderContext(SelfTuningWorkManagerImpl.java:640)
          at weblogic.work.ExecuteThread.execute(ExecuteThread.java:406)
          at weblogic.work.ExecuteThread.run(ExecuteThread.java:346)
  Caused By: java.lang.OutOfMemoryError: Requested array size exceeds VM limit
          at java.util.Arrays.copyOf(Arrays.java:3332)
          at java.lang.AbstractStringBuilder.expandCapacity(AbstractStringBuilder.java:137)
          at java.lang.AbstractStringBuilder.ensureCapacityInternal(AbstractStringBuilder.java:121)
          at java.lang.AbstractStringBuilder.append(AbstractStringBuilder.java:569)
          at java.lang.StringBuffer.append(StringBuffer.java:369)
          at java.io.BufferedReader.readLine(BufferedReader.java:370)
          at java.io.BufferedReader.readLine(BufferedReader.java:389)
          at weblogic.servlet.logging.ELFLogger.readELFFields(ELFLogger.java:140)
          at weblogic.servlet.logging.ELFLogger.findFieldsDirective(ELFLogger.java:68)
          at weblogic.servlet.logging.ELFLogger.<init>(ELFLogger.java:59)
          at weblogic.servlet.logging.LogManagerHttp.initLoggers(LogManagerHttp.java:58)
          at weblogic.servlet.logging.LogManagerHttp.start(LogManagerHttp.java:42)
          at weblogic.servlet.internal.HttpServer.start(HttpServer.java:334)
          at weblogic.servlet.internal.HttpServerManagerImpl.startWebServers(HttpServerManagerImpl.java:50)
          at weblogic.servlet.internal.WebService.start(WebService.java:112)
          at weblogic.server.AbstractServerService.postConstruct(AbstractServerService.java:76)
          at sun.reflect.GeneratedMethodAccessor2.invoke(Unknown Source)
          at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
          at java.lang.reflect.Method.invoke(Method.java:498)
          at org.glassfish.hk2.utilities.reflection.ReflectionHelper.invoke(ReflectionHelper.java:1262)
          at org.jvnet.hk2.internal.ClazzCreator.postConstructMe(ClazzCreator.java:332)
          at org.jvnet.hk2.internal.ClazzCreator.create(ClazzCreator.java:374)
          at org.jvnet.hk2.internal.SystemDescriptor.create(SystemDescriptor.java:471)
          at org.glassfish.hk2.runlevel.internal.AsyncRunLevelContext.findOrCreate(AsyncRunLevelContext.java:232)
          at org.glassfish.hk2.runlevel.RunLevelContext.findOrCreate(RunLevelContext.java:85)
          at org.jvnet.hk2.internal.Utilities.createService(Utilities.java:2020)
          at org.jvnet.hk2.internal.ServiceHandleImpl.getService(ServiceHandleImpl.java:114)
          at org.jvnet.hk2.internal.ServiceLocatorImpl.getService(ServiceLocatorImpl.java:693)
          at org.jvnet.hk2.internal.ThreeThirtyResolver.resolve(ThreeThirtyResolver.java:78)
          at org.jvnet.hk2.internal.ClazzCreator.resolve(ClazzCreator.java:211)
          at org.jvnet.hk2.internal.ClazzCreator.resolveAllDependencies(ClazzCreator.java:234)
          at org.jvnet.hk2.internal.ClazzCreator.create(ClazzCreator.java:357)
  >
  <Jan 26, 2019, 2:24:01,748 AM CST> <Error> <WebLogicServer> <localhost> <msosb2> <main> <<WLS Kernel>> <> <> <1548491041748> <[severity-value: 8] [partition-id: 0] [partition-name: DOMAIN] > <BEA-000383> <A critical service failed. The server will shut itself down.>

```

Looking at the exception stack trace, I identified that server is failing because of some logging issue. My initial thought was something got corrupted when we enabled debug logging, so we reverted the debug logging configuration and tried to restart. Again same issue.

Next we cleaned the "tmp", "cache", etc. and tried to start but the server wouldn't come up.

Next we shutdown the whole domain (admin and managed servers) and cleaned "tmp", "cache", etc. and tried to restart. That one managed server wouldn't start.

At this point we were desperate, so we paged storage team to revert all file system changes back by one day. I was also frantically searching the internet and Oracle support site. Finally there was a hit on Oracle support site, with exactly same problem and a [solution](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=6097784253443&id=2240131.1&displayIndex=2&_afrWindowMode=0&_adf.ctrl-state=8dzc50cad_77#SYMPTOM).

Apparently Weblogic reads access log during startup (who would have thought about it). The access log of the failing managed server is well over 1G, since the default size of array is 1G the server is failing with outOfMemoryError. We renamed the access log and the server came up without a glitch.

**Lesson Learned:**
> If you want to do a clean restart of Weblogic domain/server, back-up and delete the logs too.

Thanks for entertaining my ramble. Hope this has been a productive experience.
