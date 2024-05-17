젠킨스를 2.361.1 버전으로 업그레이드했다.

이 경우 기존에 사용하던 자바8에서 자바17로 버전 업그레이드 요구하게 되어 메인 컨트롤러를 업그레이드하게 되었는데 이후, 젠킨스 에이전드들에서 다음 에러 메시지가 나타났다.

Connection was broken

java.nio.channels.ClosedChannelException
	at org.jenkinsci.remoting.protocol.impl.ChannelApplicationLayer.onReadClosed(ChannelApplicationLayer.java:241)
	at org.jenkinsci.remoting.protocol.ApplicationLayer.onRecvClosed(ApplicationLayer.java:221)
	at org.jenkinsci.remoting.protocol.ProtocolStack$Ptr.onRecvClosed(ProtocolStack.java:825)
	at org.jenkinsci.remoting.protocol.FilterLayer.onRecvClosed(FilterLayer.java:289)
	at org.jenkinsci.remoting.protocol.impl.SSLEngineFilterLayer.onRecvClosed(SSLEngineFilterLayer.java:177)
	at org.jenkinsci.remoting.protocol.impl.SSLEngineFilterLayer.switchToNoSecure(SSLEngineFilterLayer.java:279)
	at org.jenkinsci.remoting.protocol.impl.SSLEngineFilterLayer.processWrite(SSLEngineFilterLayer.java:501)
	at org.jenkinsci.remoting.protocol.impl.SSLEngineFilterLayer.processQueuedWrites(SSLEngineFilterLayer.java:244)
	at org.jenkinsci.remoting.protocol.impl.SSLEngineFilterLayer.doSend(SSLEngineFilterLayer.java:196)
	at org.jenkinsci.remoting.protocol.impl.SSLEngineFilterLayer.doCloseSend(SSLEngineFilterLayer.java:209)
	at org.jenkinsci.remoting.protocol.ProtocolStack$Ptr.doCloseSend(ProtocolStack.java:793)
	at org.jenkinsci.remoting.protocol.ApplicationLayer.doCloseWrite(ApplicationLayer.java:172)
	at org.jenkinsci.remoting.protocol.impl.ChannelApplicationLayer$ByteBufferCommandTransport.closeWrite(ChannelApplicationLayer.java:343)
	at hudson.remoting.Channel.close(Channel.java:1494)
	at hudson.remoting.Channel.close(Channel.java:1447)
	at jenkins.slaves.DefaultJnlpSlaveReceiver.afterChannel(DefaultJnlpSlaveReceiver.java:181)
	at org.jenkinsci.remoting.engine.JnlpConnectionState.fire(JnlpConnectionState.java:337)
	at org.jenkinsci.remoting.engine.JnlpConnectionState.fireAfterChannel(JnlpConnectionState.java:428)
	at org.jenkinsci.remoting.engine.JnlpProtocol4Handler$Handler.lambda$onChannel$0(JnlpProtocol4Handler.java:334)
	at jenkins.util.ContextResettingExecutorService$1.run(ContextResettingExecutorService.java:30)
	at jenkins.security.ImpersonatingExecutorService$1.run(ImpersonatingExecutorService.java:70)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
	at java.base/java.lang.Thread.run(Thread.java:833)


해결방법 : 에이전트들도 자바17로 업그레이드 한다.

JDK 17 로 업그레이드하고 환경설정 xml 파일을 수정한 후 다시 에이전트들을 작동시켜서 해결.