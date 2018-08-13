Add a patch similar to https://github.com/oracle/graal/issues/603

Run with:

```
$ mx native-image --report-unsupported-elements-at-runtime -H:ReflectionConfigurationFiles=reflect.json -H:DynamicProxyConfigurationFiles=proxies.json -Dio.netty.noUnsafe=true  -H:EnableURLProtocols=http -Djava.net.preferIPv4Stack=true '-H:IncludeResources=META-INF/.*'  -jar redpipe-example-helloworld-0.0.4-SNAPSHOT.jar
```

Observe:

```
   classlist:  14,696.28 ms
       (cap):   1,067.55 ms
       setup:   2,040.39 ms
Warning: Detected unnecessary RecomputeFieldValue.ArrayBaseOffset com.oracle.svm.core.jdk.Target_java_nio_DirectByteBuffer.arrayBaseOffset substitution field for java.nio.DirectByteBuffer.arrayBaseOffset. The annotated field can be removed. This ArrayBaseOffset computation can be detected automatically. Use option -H:+UnsafeAutomaticSubstitutionsLogLevel=2 to print all automatically detected substitutions. 
Aug 13, 2018 12:23:30 PM com.mchange.v2.log.slf4j.Slf4jMLog$Slf4jMLogger$InfoLogger log
INFO: MLog clients using slf4j logging.
Aug 13, 2018 12:23:34 PM org.hibernate.validator.internal.util.Version <clinit>
INFO: HV000001: Hibernate Validator null
  (typeflow):  28,605.91 ms
   (objects):  18,660.03 ms
  (features):     306.79 ms
    analysis:  49,408.73 ms
    universe:   2,243.76 ms
     (parse):   3,391.34 ms
    (inline):   3,241.88 ms
   (compile):  12,961.35 ms
     compile:  22,774.56 ms
       image:   1,604.04 ms
error: Static field or an object referenced from a static field changed during native image generation?
  object:[Ljava.lang.reflect.Parameter;@677bb2ed  of class: java.lang.reflect.Parameter[]
  reachable through:
    object: public static void org.python.core.PyInstance.ensureFinalizer(org.python.core.PyObject[],java.lang.String[])  of class: java.lang.reflect.Method
    root: org.python.core.PyInstance.makeFunction__ensure_finalizer__()

Error: Image building with exit status 1
```

(Or similar)
