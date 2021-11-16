# DBZ-4232

## To Built, Run and Test

1. Changed directory to `modules`

```
mvn clean install -pl Common
mvn clean package -pl CasaService
```

2. Start the required containers:

```
# Change dir to modules

# Run docker compose
docker compose up --build

```

3. Configure Kafka Connector by posting the content in [register-debezium-casa-postgres.json](config/kafka-connector/register-debezium-casa-postgres.json) to http://localhost:9080/connectors

4. To test, post the following JSON content to http://localhost:8080/casa

```json
{
    "recipientAccountNo": "1-987654-1234-4569",
    "sourceAccountNo": "1-234567-4321-9876",
    "amount": 29.00,
    "recipientReference": "Raspberry PI 4b 4GB"
}
```

## To Change `debezium-quarkus-outbox` version

1. [pom.xml](modules/Common/pom.xml) for Common module

```
<dependency>
    <groupId>io.debezium</groupId>
    <artifactId>debezium-quarkus-outbox</artifactId>
    <version>1.8.0.Alpha1</version>
    <!--version>1.7.0.Final</version-->
</dependency>
```

2. [pom.xml](modules/CasaService/pom.xml) for CasaService module

```
<dependency>
    <groupId>io.debezium</groupId>
    <artifactId>debezium-quarkus-outbox</artifactId>
    <version>1.8.0.Alpha1</version>
    <!--version>1.7.0.Final</version-->
</dependency>
```

3. Rebuild / install both modules

# Errors Encountered

```
modules-casa-service-1   | Hibernate: 
modules-casa-service-1   |     insert 
modules-casa-service-1   |     into
modules-casa-service-1   |         payment.casa
modules-casa-service-1   |         (amount, coreProcessedTimestamp, createdTimestamp, paymentType, recipientAccountNo, recipientReference, responseMessages, responseTimestamp, sourceAccountNo, status, id) 
modules-casa-service-1   |     values
modules-casa-service-1   |         (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
modules-casa-service-1   | 2021-11-16 02:02:53,958 ERROR [blo.bra.opa.cas.CasaResource] (executor-thread-0) Error creating the Casa record in database.: org.hibernate.MappingException: Unknown entity: io.debezium.outbox.quarkus.internal.OutboxEvent
modules-casa-service-1   | 	at org.hibernate.metamodel.internal.MetamodelImpl.entityPersister(MetamodelImpl.java:704)
modules-casa-service-1   | 	at org.hibernate.internal.SessionImpl.getEntityPersister(SessionImpl.java:1646)
modules-casa-service-1   | 	at org.hibernate.event.internal.AbstractSaveEventListener.saveWithGeneratedId(AbstractSaveEventListener.java:114)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveOrUpdateEventListener.saveWithGeneratedOrRequestedId(DefaultSaveOrUpdateEventListener.java:194)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveEventListener.saveWithGeneratedOrRequestedId(DefaultSaveEventListener.java:38)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveOrUpdateEventListener.entityIsTransient(DefaultSaveOrUpdateEventListener.java:179)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveEventListener.performSaveOrUpdate(DefaultSaveEventListener.java:32)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveOrUpdateEventListener.onSaveOrUpdate(DefaultSaveOrUpdateEventListener.java:75)
modules-casa-service-1   | 	at org.hibernate.event.service.internal.EventListenerGroupImpl.fireEventOnEachListener(EventListenerGroupImpl.java:107)
modules-casa-service-1   | 	at org.hibernate.internal.SessionImpl.fireSave(SessionImpl.java:665)
modules-casa-service-1   | 	at org.hibernate.internal.SessionImpl.save(SessionImpl.java:658)
modules-casa-service-1   | 	at io.quarkus.hibernate.orm.runtime.session.TransactionScopedSession.save(TransactionScopedSession.java:732)
modules-casa-service-1   | 	at io.quarkus.hibernate.orm.runtime.session.ForwardingSession.save(ForwardingSession.java:433)
modules-casa-service-1   | 	at io.debezium.outbox.quarkus.internal.AbstractEventDispatcher.persist(AbstractEventDispatcher.java:50)
modules-casa-service-1   | 	at io.debezium.outbox.quarkus.internal.DefaultEventDispatcher.onExportedEvent(DefaultEventDispatcher.java:31)
modules-casa-service-1   | 	at io.debezium.outbox.quarkus.internal.DefaultEventDispatcher_Observer_onExportedEvent_47c43944b4c6bd3b6fd839d543b54587c6c5e6a8.notify(DefaultEventDispatcher_Observer_onExportedEvent_47c43944b4c6bd3b6fd839d543b54587c6c5e6a8.zig:196)
modules-casa-service-1   | 	at io.quarkus.arc.impl.EventImpl$Notifier.notifyObservers(EventImpl.java:307)
modules-casa-service-1   | 	at io.quarkus.arc.impl.EventImpl$Notifier.notify(EventImpl.java:285)
modules-casa-service-1   | 	at io.quarkus.arc.impl.EventImpl.fire(EventImpl.java:70)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource.add(CasaResource.java:59)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource_Subclass.add$$superforward1(CasaResource_Subclass.zig:106)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource_Subclass$$function$$1.apply(CasaResource_Subclass$$function$$1.zig:33)
modules-casa-service-1   | 	at io.quarkus.arc.impl.AroundInvokeInvocationContext.proceed(AroundInvokeInvocationContext.java:54)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorBase.invokeInOurTx(TransactionalInterceptorBase.java:132)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorBase.invokeInOurTx(TransactionalInterceptorBase.java:103)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorRequired.doIntercept(TransactionalInterceptorRequired.java:38)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorBase.intercept(TransactionalInterceptorBase.java:57)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorRequired.intercept(TransactionalInterceptorRequired.java:32)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorRequired_Bean.intercept(TransactionalInterceptorRequired_Bean.zig:340)
modules-casa-service-1   | 	at io.quarkus.arc.impl.InterceptorInvocation.invoke(InterceptorInvocation.java:41)
modules-casa-service-1   | 	at io.quarkus.arc.impl.AroundInvokeInvocationContext.perform(AroundInvokeInvocationContext.java:41)
modules-casa-service-1   | 	at io.quarkus.arc.impl.InvocationContexts.performAroundInvoke(InvocationContexts.java:32)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource_Subclass.add(CasaResource_Subclass.zig:170)
modules-casa-service-1   | 	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
modules-casa-service-1   | 	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
modules-casa-service-1   | 	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
modules-casa-service-1   | 	at java.base/java.lang.reflect.Method.invoke(Method.java:566)
modules-casa-service-1   | 	at org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:170)
modules-casa-service-1   | 	at org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:130)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.internalInvokeOnTarget(ResourceMethodInvoker.java:660)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTargetAfterFilter(ResourceMethodInvoker.java:524)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.lambda$invokeOnTarget$2(ResourceMethodInvoker.java:474)
modules-casa-service-1   | 	at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:364)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTarget(ResourceMethodInvoker.java:476)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:434)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:408)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:69)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:492)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.lambda$invoke$4(SynchronousDispatcher.java:261)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.lambda$preprocess$0(SynchronousDispatcher.java:161)
modules-casa-service-1   | 	at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:364)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.preprocess(SynchronousDispatcher.java:164)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:247)
modules-casa-service-1   | 	at io.quarkus.resteasy.runtime.standalone.RequestDispatcher.service(RequestDispatcher.java:73)
modules-casa-service-1   | 	at io.quarkus.resteasy.runtime.standalone.VertxRequestHandler.dispatch(VertxRequestHandler.java:138)
modules-casa-service-1   | 	at io.quarkus.resteasy.runtime.standalone.VertxRequestHandler$1.run(VertxRequestHandler.java:93)
modules-casa-service-1   | 	at io.quarkus.vertx.core.runtime.VertxCoreRecorder$13.runWith(VertxCoreRecorder.java:543)
modules-casa-service-1   | 	at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2449)
modules-casa-service-1   | 	at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1478)
modules-casa-service-1   | 	at org.jboss.threads.DelegatingRunnable.run(DelegatingRunnable.java:29)
modules-casa-service-1   | 	at org.jboss.threads.ThreadLocalResettingRunnable.run(ThreadLocalResettingRunnable.java:29)
modules-casa-service-1   | 	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
modules-casa-service-1   | 	at java.base/java.lang.Thread.run(Thread.java:829)
modules-casa-service-1   | 
modules-casa-service-1   | 2021-11-16 02:02:53,987 ERROR [io.qua.ver.htt.run.QuarkusErrorHandler] (executor-thread-0) HTTP Request to /casa failed, error id: 8fb9cbac-e0ac-4374-9ff1-205cb06790be-1: org.jboss.resteasy.spi.UnhandledException: org.hibernate.MappingException: Unknown entity: io.debezium.outbox.quarkus.internal.OutboxEvent
modules-casa-service-1   | 	at org.jboss.resteasy.core.ExceptionHandler.handleApplicationException(ExceptionHandler.java:106)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ExceptionHandler.handleException(ExceptionHandler.java:372)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.writeException(SynchronousDispatcher.java:218)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:519)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.lambda$invoke$4(SynchronousDispatcher.java:261)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.lambda$preprocess$0(SynchronousDispatcher.java:161)
modules-casa-service-1   | 	at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:364)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.preprocess(SynchronousDispatcher.java:164)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:247)
modules-casa-service-1   | 	at io.quarkus.resteasy.runtime.standalone.RequestDispatcher.service(RequestDispatcher.java:73)
modules-casa-service-1   | 	at io.quarkus.resteasy.runtime.standalone.VertxRequestHandler.dispatch(VertxRequestHandler.java:138)
modules-casa-service-1   | 	at io.quarkus.resteasy.runtime.standalone.VertxRequestHandler$1.run(VertxRequestHandler.java:93)
modules-casa-service-1   | 	at io.quarkus.vertx.core.runtime.VertxCoreRecorder$13.runWith(VertxCoreRecorder.java:543)
modules-casa-service-1   | 	at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2449)
modules-casa-service-1   | 	at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1478)
modules-casa-service-1   | 	at org.jboss.threads.DelegatingRunnable.run(DelegatingRunnable.java:29)
modules-casa-service-1   | 	at org.jboss.threads.ThreadLocalResettingRunnable.run(ThreadLocalResettingRunnable.java:29)
modules-casa-service-1   | 	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
modules-casa-service-1   | 	at java.base/java.lang.Thread.run(Thread.java:829)
modules-casa-service-1   | Caused by: org.hibernate.MappingException: Unknown entity: io.debezium.outbox.quarkus.internal.OutboxEvent
modules-casa-service-1   | 	at org.hibernate.metamodel.internal.MetamodelImpl.entityPersister(MetamodelImpl.java:704)
modules-casa-service-1   | 	at org.hibernate.internal.SessionImpl.getEntityPersister(SessionImpl.java:1646)
modules-casa-service-1   | 	at org.hibernate.event.internal.AbstractSaveEventListener.saveWithGeneratedId(AbstractSaveEventListener.java:114)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveOrUpdateEventListener.saveWithGeneratedOrRequestedId(DefaultSaveOrUpdateEventListener.java:194)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveEventListener.saveWithGeneratedOrRequestedId(DefaultSaveEventListener.java:38)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveOrUpdateEventListener.entityIsTransient(DefaultSaveOrUpdateEventListener.java:179)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveEventListener.performSaveOrUpdate(DefaultSaveEventListener.java:32)
modules-casa-service-1   | 	at org.hibernate.event.internal.DefaultSaveOrUpdateEventListener.onSaveOrUpdate(DefaultSaveOrUpdateEventListener.java:75)
modules-casa-service-1   | 	at org.hibernate.event.service.internal.EventListenerGroupImpl.fireEventOnEachListener(EventListenerGroupImpl.java:107)
modules-casa-service-1   | 	at org.hibernate.internal.SessionImpl.fireSave(SessionImpl.java:665)
modules-casa-service-1   | 	at org.hibernate.internal.SessionImpl.save(SessionImpl.java:658)
modules-casa-service-1   | 	at io.quarkus.hibernate.orm.runtime.session.TransactionScopedSession.save(TransactionScopedSession.java:732)
modules-casa-service-1   | 	at io.quarkus.hibernate.orm.runtime.session.ForwardingSession.save(ForwardingSession.java:433)
modules-casa-service-1   | 	at io.debezium.outbox.quarkus.internal.AbstractEventDispatcher.persist(AbstractEventDispatcher.java:50)
modules-casa-service-1   | 	at io.debezium.outbox.quarkus.internal.DefaultEventDispatcher.onExportedEvent(DefaultEventDispatcher.java:31)
modules-casa-service-1   | 	at io.debezium.outbox.quarkus.internal.DefaultEventDispatcher_Observer_onExportedEvent_47c43944b4c6bd3b6fd839d543b54587c6c5e6a8.notify(DefaultEventDispatcher_Observer_onExportedEvent_47c43944b4c6bd3b6fd839d543b54587c6c5e6a8.zig:196)
modules-casa-service-1   | 	at io.quarkus.arc.impl.EventImpl$Notifier.notifyObservers(EventImpl.java:307)
modules-casa-service-1   | 	at io.quarkus.arc.impl.EventImpl$Notifier.notify(EventImpl.java:285)
modules-casa-service-1   | 	at io.quarkus.arc.impl.EventImpl.fire(EventImpl.java:70)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource.add(CasaResource.java:72)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource_Subclass.add$$superforward1(CasaResource_Subclass.zig:106)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource_Subclass$$function$$1.apply(CasaResource_Subclass$$function$$1.zig:33)
modules-casa-service-1   | 	at io.quarkus.arc.impl.AroundInvokeInvocationContext.proceed(AroundInvokeInvocationContext.java:54)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorBase.invokeInOurTx(TransactionalInterceptorBase.java:132)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorBase.invokeInOurTx(TransactionalInterceptorBase.java:103)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorRequired.doIntercept(TransactionalInterceptorRequired.java:38)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorBase.intercept(TransactionalInterceptorBase.java:57)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorRequired.intercept(TransactionalInterceptorRequired.java:32)
modules-casa-service-1   | 	at io.quarkus.narayana.jta.runtime.interceptor.TransactionalInterceptorRequired_Bean.intercept(TransactionalInterceptorRequired_Bean.zig:340)
modules-casa-service-1   | 	at io.quarkus.arc.impl.InterceptorInvocation.invoke(InterceptorInvocation.java:41)
modules-casa-service-1   | 	at io.quarkus.arc.impl.AroundInvokeInvocationContext.perform(AroundInvokeInvocationContext.java:41)
modules-casa-service-1   | 	at io.quarkus.arc.impl.InvocationContexts.performAroundInvoke(InvocationContexts.java:32)
modules-casa-service-1   | 	at blog.braindose.opay.casa.CasaResource_Subclass.add(CasaResource_Subclass.zig:170)
modules-casa-service-1   | 	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
modules-casa-service-1   | 	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
modules-casa-service-1   | 	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
modules-casa-service-1   | 	at java.base/java.lang.reflect.Method.invoke(Method.java:566)
modules-casa-service-1   | 	at org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:170)
modules-casa-service-1   | 	at org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:130)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.internalInvokeOnTarget(ResourceMethodInvoker.java:660)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTargetAfterFilter(ResourceMethodInvoker.java:524)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.lambda$invokeOnTarget$2(ResourceMethodInvoker.java:474)
modules-casa-service-1   | 	at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:364)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTarget(ResourceMethodInvoker.java:476)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:434)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:408)
modules-casa-service-1   | 	at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:69)
modules-casa-service-1   | 	at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:492)
modules-casa-service-1   | 	... 15 more
modules-casa-service-1   | 
^C

```