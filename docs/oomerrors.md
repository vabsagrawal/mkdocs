# Understanding the OutOfMemory Errors

Whenever you find yourself staring at a stacktrace with an OutOfMemoryError in it, it should all be crystal-clear. The program has got no more elbow room and is dying simply because of the lack of it. From 10,000 feet or an executive chair this might already contain too much information. But for those of you who have to build or maintain the applications and figure out why a particular error is created – we can share a bit more insight into the issue.

In this post we will take a look what do different OutOfMemoryError messages actually mean. We start with the most common cases and move forward to the more interesting situations.

*  java.lang.OutOfMemoryError: Java heap space
*  java.lang.OutOfMemoryError: PermGen space
*  java.lang.OutOfMemoryError: GC overhead limit exceeded
*  java.lang.OutOfMemoryError: unable to create new native thread
*  java.lang.OutOfMemoryError: nativeGetNewTLA
*  java.lang.OutOfMemoryError: Requested array size exceeds VM limit
*  java.lang.OutOfMemoryError: request <size> bytes for <reason>. Out of swap space?
*  java.lang.OutOfMemoryError: <reason> <stack trace> (Native method)

### java.lang.OutOfMemoryError: Java heap space
We start with the one you have all seen more than you would actually  like. This is the Java Virtual Machine’s way to announce you that there is no more room in the virtual machine heap area. You are trying to create a new object, but the amount of memory this newly created structure is about to consume is more than the JVM has in the heap. The JVM has tried to free memory by calling full GC before throwing in the towel, but without any success. The fastest way to get rid of the symptoms is to increase the heap via -Xmx parameter. Note that both this and other recommendations in the article should be taken with a grain of salt. More often than not you just end up hiding the symptoms of the underlying problem.

The next suspect is also quite common. 

### java.lang.OutOfMemoryError: PermGen space

I guess most of you have seen the java.lang.OutOfMemoryError: PermGen space during redeploys. It is pretty much the same message as the first one, but instead of the heap you are now trying to allocate memory in thePermanent Generation area. And again, you do not have enough room, so the JVM native code is kind enough to let you know about it. This message tends to disappear (for awhile) if you increase the -XX:MaxPermSize parameter.

### java.lang.OutOfMemoryError: GC overhead limit exceeded

The third one – the java.lang.OutOfMemoryError: GC overhead limit exceeded – is a bit of a different beast. Instead of the missing heap / permgen the JVM is signaling that your application is spending too much time in garbage collection with little to show for it. By default the JVM is configured to throw this error if you are spending more than 98% of the total time in GC and less than 2% of the heap is recovered after the GC. Sounds like a perfectly good place to have the “fail fast” safeguard at place. In the rare cases where it makes sense to disable it, add -XX:-UseGCOverheadLimit to your startup scripts.
The above three OutOfMemoryError messages make up to 98% of the cases Plumbr detects. So there is a strong chance that the remaining quintet is somewhat unknown to you.
java.lang.OutOfMemoryError: unable to create new native thread is the  message you will receive if the JVM is asking a new thread from the OS and the underlying OS cannot allocate a new thread anymore. This limit is very platform-dependent, so if you are curious to find out your limitations then run your own little experiment using the following code snippet. On my 64bit MacOS X running a latest JDK 7 I run into troubles when creating thread #2032.

	while(true){
	    new Thread(new Runnable(){
	        public void run() {
	            try {
	                Thread.sleep(10000000);
	            } catch(InterruptedException e) { }        
			}    
		}).start();
	}

### java.lang.OutOfMemoryError: nativeGetNewTLA 

is the symptom where the JVM cannot allocate new Thread Local Area. This is something you only encounter on jRockit virtual machine. If you recall, the Thread Local Area is the buffer used to efficiently allocate memory in a multi-threaded application. Each thread has its own pre-allocated buffer where all the objects instantiated by this thread are born. You will run into problems when you are creating a vast amount of objects in a heavily multi-threaded application, in case of which you might turn to tweaking the -XXtlaSizeparameter .

### java.lang.OutOfMemoryError: Requested array size exceeds VM limit 

is the message you find yourself staring at when you are trying to create an array larger than your VM limitations allow. On my 64bit Mac OS X with a recent JDK 7 build I find myself acknowledging the fact that arrays with Integer.MAX_INT-2 elements are OK, but just one more straw, namely Integer.MAX_INT-1, breaks the camel’s back. On older 32-bit machines it had its benefits, limiting the array sizes to fit into the tiny heaps available back then. On modern 64bit machines it seems to create more confusion than to actually help in solving anything.

### java.lang.OutOfMemoryError: request <size> bytes for <reason>. Out of swap space? 

This error message is thrown when the JVM fails to allocate native memory from the OS. Note that it is completely different from the standard cases where you have exhausted the heap or permgen spaces. This message tends to be displayed when you are operating close to the platform limits. As the message itself is stating you might have exceeded the amount of physical and virtual memory available. As the latter is often implemented via swapping memory to the disk, the first thing you might think of as a quick fix would be to increase the size of the swap file. But I am yet to see an application which would behave normally while swapping, so most likely this quick fix won’t help you much.

### java.lang.OutOfMemoryError: <reason> <stack trace> (Native method) 

Now it is time to beg for help from your fellow C developers. As the message states, you are facing problems with the native code, but – unlike in the last case – the allocation failure was detected in a JNI or native method instead of the JVM code.
