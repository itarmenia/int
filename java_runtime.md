```java
Runtime.getRuntime().addShutdownHook(new Thread(){
                ///...
        });

Runtime.getRuntime().gc();

System.gc();

```