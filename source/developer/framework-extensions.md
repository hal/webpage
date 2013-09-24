title: Framework & Extensions
date: 2013-09-24 08:37:43
layout: developer-doc
---


## Framework & Extensions

### GWTP

The console builds on the GWTP framework and inherits most it's features and framework services. 

Most notable are the MVP design pattern [1] and dependency injection using Gin [2]:
 
These two along with the GWTP Placemanager [3] create the foundation for loading, initialising and rendering dialogs. 

1. http://www.gwtproject.org/articles/mvp-architecture.html
2. http://code.google.com/p/google-gin/
3. https://github.com/ArcBees/GWTP/wiki/PlaceManager


### Extensions

The web console can extended using a compile time mechanism. Extensions are regular GWT modules that implement the SPI and follow some packaging conventions. Extension are provided as maven dependencies and pulled into the hal/release-stream build [1].

Every extension has access to the regular framework contract, all of it's services and the full dependency injection scope.

In order the get an idea how extensions are setup, build and integrated it's best to look at an example extension [2] build and hal/release-stream, which acts as an aggregator for extension that can be used across the board.

1. https://github.com/hal/release-stream/
2. https://github.com/teiid/teiid-web-console/tree/teiid-console-parent-1.1.0.Final

#### Extension SPI

The extension SPI has two main building blocks: 

- The extension point
- Gin mixin definitions

**The Extension Point**

In order to provide an extension point you need to implement a presenter-view (as described in the MVP section). For the extension to be discovered you need to annotate the Presenter proxy with a '@SubsystemExtension' declaration.

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

- 'name' & 'group' declarations: The link name and the group for the left hand side navigation.
- 'key': the subsystem as it is referred to in the DMR model. If the subsystem doesn't exist, the dialog will not be shown.

**Gin Mixins**

The mixins are needed to extend the dependency injection scope and wire up your presenters-views couples and other utility classes (if needed).

A mixin is declared both as a binding and model extension. The '@GinExtension' value refers the GWT module descriptor used with the extension.

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

**Packaging Conventions**

Due to the peculiarities of how the GWT compiler works, we need to enforce some constraints on how you can package your extension code:

- Extension must reside below the package ```org.jboss.as.console.client.*```. 

