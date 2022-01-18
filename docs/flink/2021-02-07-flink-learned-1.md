---
layout: default
title: What I learned from a crash of a Yarn application
parent: Flink
nav_order: 1
---
I came across this error when our Flink application deployed in Yarn was killed. 
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

The first line tells that there is an exception when calling the method **ApplicationMasterProtocolPBClientImpl.allocate**. **ApplicationMasterProtocolPBClientImpl** is actually
the implementation of AM (Application Master). The AM sent a request to **rm0** (Resource Manager) asking for some resources, but it didn't make it for some reason. 

This can be reflected by this line of stacks **org.apache.hadoop.yarn.client.api.impl.AMRMClientImpl.allocate(AMRMClientImpl.java:277)**.

**AMRMClientImpl** implements an abstract method **AMRMClient.allocate**, whose goal is to request additional containers and receive new container allocations. When the job
is submitted to yarn, firstly, an AM will be created; and then AM asks RM for some containers to execute its tasks in parallel; and if having yes to be attributed
some containers, NM (Node Manager) will launch them for AM. So here, the reason why AM doesn't manage to have new containers is that we are running out of resources.
And the AM keeps asking, until it receives an invalid token to be forced to stop itself. (See the first line of logs below)

```
2021-04-21 13:22:23,838 WARN  org.apache.hadoop.io.retry.RetryInvocationHandler             - Exception while invoking ApplicationMasterProtocolPBClientImpl.allocate over rm1. Not retrying because Invalid or Cancelled Token
org.apache.hadoop.security.token.SecretManager$InvalidToken: Invalid AMRMToken from appattempt_1618921753713_26041_000001
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at org.apache.hadoop.yarn.ipc.RPCUtil.instantiateException(RPCUtil.java:53)
	at org.apache.hadoop.yarn.ipc.RPCUtil.unwrapAndThrowException(RPCUtil.java:104)
	at org.apache.hadoop.yarn.api.impl.pb.client.ApplicationMasterProtocolPBClientImpl.allocate(ApplicationMasterProtocolPBClientImpl.java:79)
	at sun.reflect.GeneratedMethodAccessor28.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:288)
	at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:206)
	at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:188)
	at com.sun.proxy.$Proxy15.allocate(Unknown Source)
	at org.apache.hadoop.yarn.client.api.impl.AMRMClientImpl.allocate(AMRMClientImpl.java:277)
	at org.apache.hadoop.yarn.client.api.async.impl.AMRMClientAsyncImpl$HeartbeatThread.run(AMRMClientAsyncImpl.java:224)
Caused by: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.security.token.SecretManager$InvalidToken): Invalid AMRMToken from appattempt_1618921753713_26041_000001
	at org.apache.hadoop.ipc.Client.call(Client.java:1474)
	at org.apache.hadoop.ipc.Client.call(Client.java:1411)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:231)
	at com.sun.proxy.$Proxy14.allocate(Unknown Source)
	at org.apache.hadoop.yarn.api.impl.pb.client.ApplicationMasterProtocolPBClientImpl.allocate(ApplicationMasterProtocolPBClientImpl.java:77)
```

Finally, we make it running again by asking infrastructure team to raise our team's quota to be able to have more resources on Yarn.
