title: Data Model
date: 2013-09-24 08:37:43
layout: page
---

### The DMR Model

The [DMR Model](https://docs.jboss.org/author/display/AS71/Management+resources) provides the core data format and
RPC capabilities to integrate with the wildfly management layer. Any management use case implemented in HAL leverages DMR for data retrieval and execution of management operations.
<p/>
Before you get started it's good to make yourself familiar with the [core management concepts](https://docs.jboss.org/author/display/AS71/Core+management+concepts)
and the characteristics of [management resources](https://docs.jboss.org/author/display/AS71/Management+resources) in general.

### Dispatcher Service

The [dispatcher library](https://github.com/hal/core/tree/master/dmr) provides the API and services
to execute management operations against the wildfly management layer. It uses HTTP as the transport and DMR as the wire format.

The dispatcher is provided through the [core IOC facilities](https://github.com/ArcBees/GWTP/wiki/GIN-Binding):

``` java
@Inject
public TransactionPresenter(DispatchAsync dispatcher)
{
    this.dispatcher = dispatcher;
}
```

Once you get hold of a dispatcher instance, you need to wrap the DMR representation (aka ModelNode) in an action
and provide an async callback to retrieve the results:

``` java
 ModelNode operation = new ModelNode();
 operation.get(ADDRESS).add("subsystem", "transactions");
 operation.get(OP).set(READ_RESOURCE_OPERATION);
 operation.get(INCLUDE_RUNTIME).set(true);

 dispatcher.execute(new DMRAction(operation), new AsyncCallback<DMRResponse>() {
    @Override
    public void onSuccess(DMRResponse dmrResponse) {
        ModelNode response = dmrResponse.get();
        // process the response
       }
    }
 );
```


### ModelNode API

In addition to the HTTP wrapper and marshalling facilities, the dispatcher library also contains a ModelNode API,
to interact with the DMR representations at hand.

``` java

// create a model node
ModelNode operation = new ModelNode();
operation.get(OP).set(READ_ATTRIBUTE_OPERATION);
operation.get(NAME).set("process-type");
operation.get(ADDRESS).setEmptyList();

// read a model node
ModelNode response = result.get();

if(response.isFailure()) {
    // process error
    Console.error(...);

} else {
    // all good : parse response
    ModelNode payload= response.get(RESULT);
    boolean isStandalone = payload.asString().equals("Server");
}

```

### AutoBeans and EntityAdapter

Many of the GWT widgets work on [AutoBean's](http://code.google.com/p/google-web-toolkit/wiki/AutoBean).
Since the web console does rely on the default widgets, we did chose it as the default data-model object representation.
This way we can leverage all the default GWT API (i.e. [Cell Widgets](http://www.gwtproject.org/doc/latest/DevGuideUiCellWidgets.html)) without going through
extra hoops&loops.
<p/>
The downside, however is that you need to to convert the DMR wire format into AutoBeans at some point. This typically happens before the data
is passed into the view. We do provide an `EntityAdapter` to do the conversion DMR to AutoBean and vice versa.

``` java
ModelNode response = dmrResponse.get();
TransactionManager transactionManager = entityAdapter.fromDMR(response.get(RESULT));
getView().setTransactionManager(transactionManager);
```

<div class="alert alert-info">
In future versions thr AutoBean mapping will not be required anymore.
We are already working on a different design that fully leverages the DMR meta data and provides
transparent mappings to default GWT widgets.
<p/>
For further information please refer to the [Interface Kernel Documentation](/mbui/1_overview.html)
</div>

#### Entity Meta Data

In to leverage the EntityAdapter, you need to provide some meta data to bridge the gap between the wireformat
and the AutoBean framework:

``` java
@Address("/subsystem=transactions")
public interface TransactionManager {

    @Binding(detypedName = "default-timeout", expr = true)
    int getDefaultTimeout();
    void setDefaultTimeout(int t);

    @Binding(detypedName = "enable-statistics", expr = true)
    boolean isEnableStatistics();
    void setEnableStatistics(boolean b);

    @Binding(detypedName = "enable-tsm-status", expr = true)
    boolean isEnableTsmStatus();
    void setEnableTsmStatus(boolean b);

    @Binding(detypedName = "jts", expr = true)
    boolean isJts();
    void setJts(boolean b);

    [...]
}

```

For full documentation please refer to the API docs of the `@Binding` annotation.

#### AutoBean Factory

The central entry point for the GWT compiler to create the boilerplate for the AutoBean's is the AutoBean factory.
Any data object that you want to use needs to be registered with the global factory:

``` java

package org.jboss.as.console.client.shared;

@BeanFactoryExtension
public interface CoreBeanFactory {

    AutoBean<TransactionManager> transactionManager();
    [...]
}
```

<div class="alert alert-info">
[Extension providers](/developer/4_console-extensions.html) can rely on the @BeanFactoryExtension declaration to have separate AutoBean factory definitions
 outside the scope of the core console.
</div>

