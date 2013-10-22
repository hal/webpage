title: Console Extensions
date: 2013-09-24 08:37:43
layout: page
---

### Extensions

The console provides a compile time extension mechanism. Extensions are regular GWT modules that implement the SPI and follow certain packaging conventions.
Extension are provided as maven dependencies and distributed through the [hal/release-stream build](https://github.com/hal/release-stream/).
<p/>
Any extension has access to the regular framework contract, all of it's services and the full dependency injection scope.
In order the get an idea how extensions are setup, build and integrated it's best to look at an [example project](https://github.com/teiid/teiid-web-console/tree/teiid-console-parent-1.1.0.Final)
and [hal/release-stream](https://github.com/hal/release-stream/), which acts as an aggregator build.

### Contract

The extension SPI has two main building blocks: 

- The extension point
- Gin mixin definitions

#### Extension Point

In order to provide an extension point you need to implement a presenter-view (as described in the [Framework section](/developer/5_Framework.html)).
For the extension to be discovered you need to annotate the Presenter proxy with a `@SubsystemExtension` declaration.
This instructs the SPI processor to include another place that represents your subsystem specific dialog:

``` java
public class TransportPresenter {

@ProxyCodeSplit
@NameToken("teiid-transports")
@SubsystemExtension(name="Transports", group = "Teiid", key="teiid")
public interface MyProxy 
		extends Proxy<TransportPresenter>,Place {
    }

}
```

The extension will be loaded, initialised and revealed automatically by the core framework according to the following criteria:

- `name` & `group` declarations: The link name and the group for the left hand side navigation.
- `key`: the subsystem as it is referred to in the DMR model. If the subsystem doesn't exist, the dialog will not be shown.

#### IOC Mixins

The mixins are needed to extend the dependency injection scope and wire up your presenters-views couples and other utility classes (if needed).
A mixin is declared both as a binding and model extension. The `@GinExtension` value refers the GWT module descriptor used with the extension.

The injection points:

``` java
@GinExtension("org.jboss.as.console.TeiidExtension")
public interface Extension {
    AsyncProvider<TransportPresenter> getTransportPresenter();
}
```

The actual binding:

``` java
@GinExtensionBinding
public class ExtensionBinding extends AbstractPresenterModule {

    @Override
    protected void configure() {
        bindPresenter(TransportPresenter.class,
        		TransportPresenter.MyView.class,
                TransportView.class,
                TransportPresenter.MyProxy.class);
    }
}
```

#### Data Objects (aka AutoBeans)

Extension most often provide their own data objects. Take a look at the [Data Model Section](/developer/3_data-model.html).
It describes how custom AutoBean's can be provided and used.


#### Gotchas

<div class="alert">
**Packaging Conventions**<br/>

Due to the peculiarities of how the GWT compiler works, we need to enforce some constraints on how you can package your extension code:

- Extension must reside below the package ```org.jboss.as.console.client.*```.
</div>


