title: DMR Scala
date: 2013-10-21 10:30:06
categories: [dmr,scala]
---
We made some progress with the Scala client libraries for DMR:

- [DMR.scala](https://github.com/hal/dmr.scala)
- [DMR.repl](https://github.com/hal/dmr.repl)<!--more-->

### DMR.scala

DMR.scala is a wrapper around the [DMR Java](https://docs.jboss.org/author/display/WFLY8/Detyped+management+and+the+jboss-dmr+library) library
and features a DSL for DMR operations. Using DMR.scala you can use all those neat Scala features when working with
model nodes:

- Operators to access child nodes
- Model nodes are collection of keys / child nodes (enables you to filter, map, ...)
- Pattern matching and extractors

### DMR.repl

DMR.repl is based on DMR.scala and leverages the Scala REPL shell to work with the management model in a very
interactive way. You can connect to an arbitrary WildFly instance, execute single DMR operations or write scripts
to accomplish more complex tasks. See the DMR.repl website for more infos.

### Upcoming Talk

There will be a short introduction of both libraries at the JBoss [OneDayTalk](http://onedaytalk.org/) as part of
Emanuel Muckenhubers talk about the [WildFly 8 CLI](http://onedaytalk.org/index.php/program?id=168).