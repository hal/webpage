title: Model Based User Interface
date: 2013-09-24 08:37:43
layout: page
---

<span class="label label-info">Since 2.0</span>

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

- [The Useware Dialog Model](http://www.w3.org/wiki/images/8/80/Useware_Dialog_Modeling_%28useDM%29_Language_.pdf)
- [The W3C MBUI working group](http://www.w3.org/2011/01/mbui-wg-charter)

It breaks down into an abstract description of the structural parts (interaction types) and a mapping towards specific
behaviour (operations & data):

{% img  /images/useware-model.png %}

### Interface Kernel

The model is used by an interface kernel that generates a graphical user interface.
The kernel is an embeddable component that provides GWT bindings by default. It runs within the web console and
leverages all stock components and services to load and initialise the interfaces
for manageable resources in Wildlfy 8.

The model itself can either be provided through an API or as a XML file (remote loading):

``` xml

<dialog xmlns="http://wildfly.org/transactions" id="transaction-subsystem">

    <container id="transactionManager" operator="Concurrency" label="TransactionManager">
        <dmr address="/{selected.profile}/subsystem=transactions"/>
        <container id="configGroups" operator="Choice">

            <form id="transactionManager#basicAttributes" operator="Concurrency" label="Attributes">
                <dmr>
                    <attribute name="enable-statistics"/>
                    <attribute name="enable-tsm-status"/>
                    <attribute name="jts"/>
                    <attribute name="default-timeout"/>
                    <attribute name="node-identifier"/>
                    <attribute name="use-hornetq-store"/>
                </dmr>
            </form>

            [...]

        </container>
    </container>

</dialog>

```
<div class="alert alert-info">
We plan to provide a shared dialog repository that both users and subsystems developers can contribute to.
</div>

#### Final User Interface

Reification is the process of turning the model into a final user interface.
The kernel uses the behaviour mappings to fetch the necessary management resource meta data (attributes, operations, etc)
and generates the final, platform specific representation to be used within the web console.

{% img  /images/tx-reify.png %}

<p/>
For the sake of brevity we used a simple example (Wildfly transaction subsystem) here.
You can find more complex ones in the [repository](https://github.com/hal/core).

<div class="alert alert-info">
These examples are taken from the MBUI workbench that ships with the web console. It's enabled when running in development mode.
</div>
