# metals-issue-1
VSCode + Metals issue nr1

The issue was first encountered on windows 11 2022-11-12, after creating a new scala 2 (2.13.10) project

Is it normal for metals (vscode) to be logging the following every time I define a method with an output type I have not yet imported?
```
[Error - 10:52:51] Request textDocument/codeAction failed.
  Message: Internal error.
  Code: -32603 
java.util.concurrent.CompletionException: java.lang.NullPointerException
    at java.base/java.util.concurrent.CompletableFuture.encodeThrowable(CompletableFuture.java:331)
    at java.base/java.util.concurrent.CompletableFuture.completeThrowable(CompletableFuture.java:346)
    at java.base/java.util.concurrent.CompletableFuture$UniAccept.tryFire(CompletableFuture.java:704)
    at java.base/java.util.concurrent.CompletableFuture.postComplete(CompletableFuture.java:506)
    at java.base/java.util.concurrent.CompletableFuture.completeExceptionally(CompletableFuture.java:2088)
    at scala.meta.internal.metals.CancelTokens$.$anonfun$future$1(CancelTokens.scala:40)
    at scala.meta.internal.metals.CancelTokens$.$anonfun$future$1$adapted(CancelTokens.scala:38)
    at scala.concurrent.impl.Promise$Transformation.run(Promise.scala:484)
    at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
    at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
    at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: java.lang.NullPointerException```
Seems like it is triggered only if the type in question actually exists, but you have not imported it yet.

Example code from a minimal scala 2.13.10 project triggering it
```
object Main extends App {
  def foofoo: AtomicLong = ???
  println("Hello, World!")
}
```
However the following will not trigger it
```
object Main extends App {
  def foofoo: Blah = ???
  println("Hello, World!")
}
```
