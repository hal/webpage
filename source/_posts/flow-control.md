title: Flow Control
date: 2013-09-25 12:10:28
tags:
---

Within Ajax applications you need to manage a lot of interaction and RPC related asynchronous events. The control flow structures become hard to understand and yield unpredicatable results.
We created a re-usable GWT module that provides some higher order control structures that you can use to tame the asynchronous callback beast. It’s been inspired by Async.js, but aligns with the core GWT API’s more naturally.

<!-- more -->

{% gist 6086876 %}


## Example: Waterfall Execution

Runs an array of functions in series, working on a shared context. However, if any of the functions pass an error to the callback, the next function is not executed and the outcome is immediately called with the error.

{% gist 6087016 %}

**Step by Step**

1. Create some executable command (aka Function)
2. Create a command that should be run once the first one finishes
3. The final callback to be invoked when the waterfall execution finishes. The StringBuffer acts as a shared context object
4. Invoke the waterfall execution, providing a shared context (StringBuffer) and a number of function to be executed

**Sources & Maven Coordinates**

The sources for the control API can be found at github:

https://github.com/hal/core/tree/master/flow/core

The maven artefacts resides with the JBoss repositories:

```
<dependency>
<groupId>org.jboss.as</groupId>
<artifactId>jboss-as-console-flow</artifactId>
<version>1.6.3.Final</version>
</dependency>
```

**GWT Module Setup**

```
<module rename-to="your_module">
[...]
<inherits name="org.jboss.gwt.flow.Flow"/>
</module>
```
