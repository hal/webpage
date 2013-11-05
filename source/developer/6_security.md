title: Security
date: 2013-09-24 08:37:43
layout: page
---

### Authentication

The web console uses the default [Wildfly HTTP entrypoint](https://docs.jboss.org/author/display/WFLY8/Default+HTTP+Interface+Security) to access the management layer.
As such it inherits it's authentication mechanism to secure the HTTP management API.

The management layer uses [HTTP Digest Authentication](http://tools.ietf.org/html/rfc2069), which is completely
encapsulated in the browser and beyond the control of the web console application itself.

In order to develop or work with the console, you need to [setup a management user](https://docs.jboss.org/author/display/WFLY8/add-user+utility).

The console resources (javascript, css, images) are retrieved using a non-secure connection, but the first management request (console bootstrap, XHR)
will then trigger the authentication process and prompt for a username and password:

{% img  /images/auth-prompt.png %}

#### Gotchas

The biggest problem with digest authentication is that [logout doesn't work reliably](http://www.google.com/search?q=digest%20authentication%20logout).
We tried our best to workaround around the issue and in most cases it works fine.

However if run into a browser/os combination that fails, the only thing you can do is to close the web browser itself.

<div class="alert alert-info">
Obviously this is not a desirable solution. It works and has it's benefits (out-of-the-box experience),
but we are already working on [a replacement](https://github.com/hal/docs/wiki/Keymaker).
</div>

### Authorisation

<span class="label label-info">Since 2.0</span>

Like the authentication mechanism, authorisation is driven the management layer capabilities as well. Any manageable resource
does declare a set of privileges to control how a resource, it's attributes and operations can be accessed.


<div class="alert alert-info">
Please note the security policy decision and it's enforcement are done on the server side. Authorisation checks on the client side
should merely improve the user experience, but are not meant to a security measure per se.
</div>

#### Meta Data

All interaction units need to know what data they operate on, in order to be instrumented by the security framework.
Sometimes this means removing complete widget or disabling a button if certain operations are not granted.

In order to tell the security framework what resource meta data needs to be considered, you need to declare the
access control meta data on a presenter:

``` java
public class TransactionPresenter {

 @ProxyCodeSplit
 @NameToken(NameTokens.TransactionPresenter)
 @AccessControl(resources = {"{selected.profile}/subsystem=transactions"})
 public interface MyProxy extends Proxy<TransactionPresenter>, Place {
 }
```

The `@AccessControl` annotation defines the set of resources a presenter operates on.
In this example it's just single resource, but you can declare multiple ones.

Based on that information, the security framework (built-in) creates a security context that will be queried
when authorisation decisions need to be taken on the client side.

#### Security Framework

The security framework is built into the console and operates transparently in the background. It's tied into the overall framework
mechanism of [loading and initialising views](/developer/5_Framework.html)
and cooperates with the [widget library](/developer/2_look-and-feel.html) to enforce certain decisions.

Apart from the `@AccessControl` definition you typically don't need to interact with it's API unless you run into a specific edge case.


