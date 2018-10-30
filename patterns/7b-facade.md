## Basic Description

A Facade is a wrapper that allows to simplify an interface (for example, by
grouping multiple methods of the more complex interface into a single method).
The interface of the more complex subsystem is still accessible after implementing
the facade.

## Useful Notes

* A facade *may* add new capabilities to the wrapped subsystem.

* A system can potentially have multiple facades in front of it.

* Facades can also help decoupling clients from subsystems. If elements from a
subsystem change, but clients are coded to a Facade rather than to the changed
subsystem elements, then we would only need to change the Facade.

* The main difference with an Adapter is in their intent; the Adapter is used
when you *need* to conform to a new interface, the Facade is used to *simplify*
an existing subsystem.

## The Principle of Least Knowledge

*AKA the Law of Demeter*

"Talk only to your immediate friends", or in other words build systems that reduce
the number of dependencies between classes.

We should only invoke methods that belong to:

* The object itself

* Objects passes in as parameters to the method

* Objects the method creates or instantiates

* Components of the object