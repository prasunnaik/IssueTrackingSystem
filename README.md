
java.net.UnknownHostException: Failed to resolve 'UI-5CG44846VT.us.deloitte.com' [A(1), AAAA(28)] after 4 queries 
	at io.netty.resolver.dns.DnsResolveContext.finishResolve(DnsResolveContext.java:1139) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
	Suppressed: The stacktrace has been enhanced by Reactor, refer to additional information below: 
Error has been observed at the following site(s):
	*__checkpoint ⇢ org.springframework.cloud.gateway.filter.WeightCalculatorWebFilter@5d1d9d73
	*__checkpoint ⇢ HTTP GET "/api/users" [ExceptionHandlingWebHandler]
Original Stack Trace:
		at io.netty.resolver.dns.DnsResolveContext.finishResolve(DnsResolveContext.java:1139) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.tryToFinishResolve(DnsResolveContext.java:1086) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.query(DnsResolveContext.java:443) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.onResponse(DnsResolveContext.java:668) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.lambda$query$0(DnsResolveContext.java:499) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListener0(DefaultPromise.java:604) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListeners0(DefaultPromise.java:597) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListenersNow(DefaultPromise.java:573) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListeners(DefaultPromise.java:506) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.setValue0(DefaultPromise.java:650) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.setSuccess0(DefaultPromise.java:639) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.trySuccess(DefaultPromise.java:119) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsQueryContext.trySuccess(DnsQueryContext.java:304) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsQueryContext.finishSuccess(DnsQueryContext.java:295) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsNameResolver$DnsResponseHandler.channelRead(DnsNameResolver.java:1558) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.handler.codec.MessageToMessageDecoder.channelRead(MessageToMessageDecoder.java:107) ~[netty-codec-base-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.DefaultChannelPipeline$HeadContext.channelRead(DefaultChannelPipeline.java:1429) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.DefaultChannelPipeline.fireChannelRead(DefaultChannelPipeline.java:918) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.AbstractNioMessageChannel$NioMessageUnsafe.read(AbstractNioMessageChannel.java:100) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.AbstractNioChannel$AbstractNioUnsafe.handle(AbstractNioChannel.java:445) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler$DefaultNioRegistration.handle(NioIoHandler.java:388) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.processSelectedKey(NioIoHandler.java:596) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.processSelectedKeysOptimized(NioIoHandler.java:571) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.processSelectedKeys(NioIoHandler.java:512) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.run(NioIoHandler.java:484) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.SingleThreadIoEventLoop.runIo(SingleThreadIoEventLoop.java:225) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.SingleThreadIoEventLoop.run(SingleThreadIoEventLoop.java:196) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.SingleThreadEventExecutor$5.run(SingleThreadEventExecutor.java:1195) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at java.base/java.lang.Thread.run(Thread.java:1583) ~[na:na]
Caused by: io.netty.resolver.dns.DnsErrorCauseException: Query failed with NXDOMAIN
	at io.netty.resolver.dns.DnsResolveContext.onResponse(..)(Unknown Source) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]

[2m2026-08-11T21:25:42.000+05:30[0;39m [31mERROR[0;39m [35m4752[0;39m [2m--- [api-gateway] [ctor-http-nio-2] [0;39m[36mb.w.a.e.AbstractErrorWebExceptionHandler[0;39m [2m:[0;39m [e3644de3-2]  500 Server Error for HTTP GET "/api/users"

java.net.UnknownHostException: Failed to resolve 'UI-5CG44846VT.us.deloitte.com' [A(1), AAAA(28)] after 4 queries 
	at io.netty.resolver.dns.DnsResolveContext.finishResolve(DnsResolveContext.java:1139) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
	Suppressed: The stacktrace has been enhanced by Reactor, refer to additional information below: 
Error has been observed at the following site(s):
	*__checkpoint ⇢ org.springframework.cloud.gateway.filter.WeightCalculatorWebFilter@5d1d9d73
	*__checkpoint ⇢ HTTP GET "/api/users" [ExceptionHandlingWebHandler]
Original Stack Trace:
		at io.netty.resolver.dns.DnsResolveContext.finishResolve(DnsResolveContext.java:1139) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.tryToFinishResolve(DnsResolveContext.java:1086) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.query(DnsResolveContext.java:443) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.onResponse(DnsResolveContext.java:668) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsResolveContext.lambda$query$0(DnsResolveContext.java:499) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListener0(DefaultPromise.java:604) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListeners0(DefaultPromise.java:597) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListenersNow(DefaultPromise.java:573) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.notifyListeners(DefaultPromise.java:506) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.setValue0(DefaultPromise.java:650) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.setSuccess0(DefaultPromise.java:639) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.DefaultPromise.trySuccess(DefaultPromise.java:119) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsQueryContext.trySuccess(DnsQueryContext.java:304) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsQueryContext.finishSuccess(DnsQueryContext.java:295) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.resolver.dns.DnsNameResolver$DnsResponseHandler.channelRead(DnsNameResolver.java:1558) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.handler.codec.MessageToMessageDecoder.channelRead(MessageToMessageDecoder.java:107) ~[netty-codec-base-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.DefaultChannelPipeline$HeadContext.channelRead(DefaultChannelPipeline.java:1429) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.DefaultChannelPipeline.fireChannelRead(DefaultChannelPipeline.java:918) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.AbstractNioMessageChannel$NioMessageUnsafe.read(AbstractNioMessageChannel.java:100) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.AbstractNioChannel$AbstractNioUnsafe.handle(AbstractNioChannel.java:445) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler$DefaultNioRegistration.handle(NioIoHandler.java:388) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.processSelectedKey(NioIoHandler.java:596) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.processSelectedKeysOptimized(NioIoHandler.java:571) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.processSelectedKeys(NioIoHandler.java:512) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.nio.NioIoHandler.run(NioIoHandler.java:484) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.SingleThreadIoEventLoop.runIo(SingleThreadIoEventLoop.java:225) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.channel.SingleThreadIoEventLoop.run(SingleThreadIoEventLoop.java:196) ~[netty-transport-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.SingleThreadEventExecutor$5.run(SingleThreadEventExecutor.java:1195) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30) ~[netty-common-4.2.15.Final.jar:4.2.15.Final]
		at java.base/java.lang.Thread.run(Thread.java:1583) ~[na:na]
Caused by: io.netty.resolver.dns.DnsErrorCauseException: Query failed with NXDOMAIN
	at io.netty.resolver.dns.DnsResolveContext.onResponse(..)(Unknown Source) ~[netty-resolver-dns-4.2.15.Final.jar:4.2.15.Final]

