title: Security
date: 2013-09-24 08:37:43
layout: page
---

### Authentication

The web console uses the default [Wildfly HTTP entrypoint](https://docs.jboss.org/author/display/WFLY8/Default+HTTP+Interface+Security) to access the management layer.
As such it inherits it's authentication mechanism from there.

Currently that means [HTTP Digest Authentication](http://tools.ietf.org/html/rfc2069), which is completely
encapsulated in the browser runtime and beyond the control of the web console application logic.

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

Like the authentication mechanism, authorisation is driven the management layer capabilities as well.

#### Meta Data

#### Security Framework
