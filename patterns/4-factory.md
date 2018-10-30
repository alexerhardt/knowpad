## The problem with instantiation

We sometimes need to instantiate objects at runtime, such as:

```java
Duck duck;

if (picnic) {
    duck = new MallardDuck();
}
else if (hunting) {
    duck = new DecoyDuck();
}
else if (inBathTub) {
    duck = new RubberDuck();
}
```

The problem here is if we want to add or modify conditions and classes - it 
can get very messy.


## A "simple factory" example

Note how this is not really a design pattern - it's more of an idiom.

```java
Public class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza = null;

        if (type.equals("cheese")) {
            pizza = new CheesePizza();
        }
        else if (type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        }
        // etc.

        return pizza;
    }
}

Public class PizzaStore {
    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
}
```


## A real factory

If we want to centralize certain procedures, but allow subclasses to establish
some procedures of their own, we need a slightly more complex pattern.

Following with the above, imagine that we want to standardize basic pizza
making operations in the name of quality control, but we still wanted to allow
restaurants to define their own pizzas:

```java
Public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
        // This is now back to being a class method
        Pizza pizza = createPizza(type);

        // These procedures are standardized
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    // Each subclass decides
    abstract Pizza createPizza(String type);
}
```

"A factory method handles object creation and encapsulates it in a subclass.
This decouples the client code in the superclass from the object creation
code in the subclass."


## The Dependency Inversion Principle

"Depend upon abstractions. Do not depend upon concrete classes."

A PizzaStore class that relies on dozens of concrete classes is going to be 
messy and rigid. The PizzaStore should only depend on a Pizza abstraction,
and the concrete classes program to that abstraction (p. 141 and onward
explain this very well)

Principle guidelines:

* No variable should hold a reference to a concrete class
* No class should derive from a concrete class
* No method should override an implemented method of any of its base classes
