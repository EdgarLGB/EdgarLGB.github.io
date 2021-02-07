---
layout: post
title: What I learned about Flink
---
![alt text](../images/statue-Christ-the-Redeemer-Rio-de-Janeiro.jpg)

I came across this error when our Flink application was down. 
```
2021-02-01 16:01:14,708 INFO  org.apache.hadoop.io.retry.RetryInvocationHandler             - Exception while invoking ApplicationMasterProtocolPBClientImpl.allocate over rm0. Trying to failover immediately.
java.io.EOFException: End of File Exception between local host is: "48-df-37-50-53-30/10.188.101.18"; destination host is: "48-df-37-4f-dd-20.am6.hpc.criteo.prod":8030; : java.io.EOFException; For more details see:  http://wiki.apache.org/hadoop/EOFException
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at org.apache.hadoop.net.NetUtils.wrapWithMessage(NetUtils.java:791)
	at org.apache.hadoop.net.NetUtils.wrapException(NetUtils.java:764)
	at org.apache.hadoop.ipc.Client.call(Client.java:1478)
	at org.apache.hadoop.ipc.Client.call(Client.java:1411)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:231)
	at com.sun.proxy.$Proxy14.allocate(Unknown Source)
	at org.apache.hadoop.yarn.api.impl.pb.client.ApplicationMasterProtocolPBClientImpl.allocate(ApplicationMasterProtocolPBClientImpl.java:77)
	at sun.reflect.GeneratedMethodAccessor29.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:288)
	at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:206)
	at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:188)
	at com.sun.proxy.$Proxy15.allocate(Unknown Source)
	at org.apache.hadoop.yarn.client.api.impl.AMRMClientImpl.allocate(AMRMClientImpl.java:277)
	at org.apache.hadoop.yarn.client.api.async.impl.AMRMClientAsyncImpl$HeartbeatThread.run(AMRMClientAsyncImpl.java:224)
Caused by: java.io.EOFException
	at java.io.DataInputStream.readInt(DataInputStream.java:392)
	at org.apache.hadoop.ipc.Client$Connection.receiveRpcResponse(Client.java:1083)
	at org.apache.hadoop.ipc.Client$Connection.run(Client.java:978)
```

The important thing is not the error itself but what I learnt from it.

First thing, this exception _java.io.EOFException_ tells that the socket thread reading the input stream actually got nothing to read. Here this stack _java.io.DataInputStream.readInt(DataInputStream.java:392)_ means the thread which is supposed to 
receive any byte as an integer didn't manage to read any byte. What caused this situation is that server is suddenly down and it is not able to send any bytes to the client through the socket.

Second thing, _ApplicationMasterProtocolPBClientImpl_ is a class implementing _ApplicationMasterProtocol_ which is the implementation of the protocol between ApplicationManager (AM) and ResourceManager (RM). RM, just like its name, is responsable for all
the things around the resource in Yarn cluster. AM is the first container launched by RM after users submitting the application. RM needs to know if the AM is still alive by keeping heartbeat between them, otherwise it will
try to restart a new AM. In fact, _org.apache.hadoop.yarn.client.api.async.impl.AMRMClientAsyncImpl$HeartbeatThread.run(AMRMClientAsyncImpl.java:224)_ shows the heartbeat has 
failed for some reason. AM didn't manage to send a heartbeat to its RM which is the root cause for this failure.

Third thing, some points about how a Yarn application is started. When users submit an application via YarnClient, they have to submit with some information to start the ApplicationMaster (AM) such as the details about the jar of
the application needed to tell how to reach the jar file, the command to execute the jar, some OS environment settings, etc. Then a container is launched to deploy the AM. AM acts like a main thread executing
a task but its job is to fork some subroutines to finish the task in parallel way. AM will ask ResourceManager (RM) for a list of available containers to launch the subroutines. After receiving 
the list, AM will ask NodeManager (NM) to launch the containers with some process settings. If everything goes well, the tasks will be run in those containers and the result be returned to
AM at last.

Bonus:

* What happens if AM is down?
=> Another AM will be launched by RM.
* What happens if RM is down?
=> The high availability of RM is guaranteed by Active/StandBy architecture.
