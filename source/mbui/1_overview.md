title: Model Based User Interface
date: 2013-09-24 08:37:43
layout: page
---

### Background

The first iteration of the console did focus on providing the main building blocks (components, services, etc)
but did require a lot of boilerplate to implement the use cases.

In terms of reliability and maintenance this was a reasonable approach, but it did fall short in several categories:

- Steep ramp up curve (GWT knowledge, etc)
- Lack of runtime extensions (all changes at compile time)
- Did not adapt to context of use (static interface)
- Limited vertical reach (reusing dialogs, i.e. JON)

For the second iteration we've been looking for an alternative that does fill these gaps, but under certain conditions:

- Don't sacrifice the level of detail we already have in the UI (prevent over simplification)
- Build on the current set of components and services (complement, not replace)

### Model 'based' vs. model 'driven'

The approach we take in the second iteration, is based on an abstract interface model. It's platform independent model
that describes both the structure and the behaviour of the interface.

We say 'model based', not 'model driven', because in our case the model acts as a blueprint, but not the ultimate source of information.
Unlike an model driven approach, that transforms a PIM into a PSM, we use the PIM in conjunction with programmatic approaches and rely on
one part or the other wherever it makes sense.

### Structure and Behaviour

The model we currently employ is influenced by two major sources:

- The DFKI Useware Model
- The W3C MBUI working group

It breaks down into an abstract description of the structural parts (interaction types) and a mapping towards specific
behaviour (operations & data):

{% img  /images/useware-model.png %}


### Further Reading

1. [Useware Dialog Model](http://www.w3.org/wiki/images/8/80/Useware_Dialog_Modeling_%28useDM%29_Language_.pdf)
2. [Model Based User Interfaces](http://www.w3.org/2011/01/mbui-wg-charter)
3. [MBUI Introduction](https://docs.google.com/document/d/1Xp50GZ8EfY017AT_pCMBq5PeK8cwNEZi1a8hXexJkCc/edit)
