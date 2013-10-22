## Useware Kernel

### Background
The strategy for managing Wildfly installations is moving to a "core distribution plus add-ons" strategy.
This basically means that we get away from "static" HAL builds to a process that allows manageable "components" to be added at anytime along the value chain.
<p/>
It can be a user that adds specific functionality to a stock installation or a productization team that creates a specific product distribution.
This requires us to take a different approach to distributing the web management interface as well.
<p/>
In HAL terms, the new strategy means that subsystems can come and go at anytime and in different combinations. Either representing main product distributions or customisation by end users.


### Goals
The Useware Kernel works on an abstract interface model.
The abstract model includes both a ***structural*** and ***behavioural*** description. It's a platform independent description of the user interface that should be put into place when managing a data model.
<p/>
The abstract model is intended to move with, and share the lifecycle of the components to be managed (i.e. subsystems).
The interface model will be distributed as markup language documents and interpreted at runtime by the interface in question (i.e. Wildfly web management).

<p/>
The Kernel will provide a specific reification layer, that can create a graphical interface based on the abstract model. Initially this will focus on the requirements for the Wildfly web interface (GWT, web technology), but alternate implementations can be provided for different target platforms (i.e. eclipse).

Although primarily being developed to accommodate Wildfly requirements, The Useware Kernel might be applicable to larger audience. By providing a baseline for migration of the interface to different platforms it can help bridging the gap between products that need to address the same use cases (Wildfly, JON, Eclipse). 

Being split into the PIM parts (markup language, tooling, etc) and concrete implementations for specific platforms, we will ensure that The Useware Kernel's core concepts and deliverables are applicable to other projects as well. 

### Scope of work

The main area of work is an actual implementation of a GUI kernel that can execute the models. The primary target platform would be web interfaces, with a strong focus on the Wildfly requirements.
An eclipse based implementation would be a desirable POC.
<p/>
Last but not least, The Useware Kernels value proposition isn't restricted to solving our software engineering problems alone. It fits nicely with usability engineering as well and can help to improve our shortcomings in this area too.

### Further Reading

1. [Useware Dialog Model](http://www.w3.org/wiki/images/8/80/Useware_Dialog_Modeling_%28useDM%29_Language_.pdf)
2. [Model Based User Interfaces](http://www.w3.org/2011/01/mbui-wg-charter)
3. [MBUI Introduction](https://docs.google.com/document/d/1Xp50GZ8EfY017AT_pCMBq5PeK8cwNEZi1a8hXexJkCc/edit)
