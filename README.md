
java.lang.IllegalArgumentException: Invalid character found in method name [0x160x030x010x000xf70x010x000x000xf30x030x03f0x93.0x020x1f0xbf0xe90xfa^0x0990x12B0x880xfaI60xe7,^0xf50xaf[0x1f0xea0xd80x9f0xcf0xe50x870xe80xbc ]. HTTP method names must be tokens
	at org.apache.coyote.http11.Http11InputBuffer.parseRequestLine(Http11InputBuffer.java:409) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:270) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:63) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:905) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1743) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:52) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1190) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:63) ~[tomcat-embed-core-10.1.36.jar:10.1.36]
	at java.base/java.lang.Thread.run(Thread.java:833) ~[na:na]
