## The problem with inheritance

In the following pattern, RubberDuck inherits a fly method from Duck.
But RubberDuck doesn't fly.

```java
public class Duck {
    public void quack() { /* Do some quacking */ }
    public void swim() { /* Do some swimming */ }
    public void fly() { /* Do some flying*/ }
}

public class MallardDuck extends Duck { /* MallardDuck methods */ }
public class RubberDuck extends Duck { /* RubberDuck methods */ }

```

Creating a "Flyable" interface wouldn't solve the problem either - this would
mean that we would need to re-write the fly() method in every subclass.


## The principle of encapsulation

Parts of an application that vary should be separated from those that
stay the same.


## Programming to an interface

We abstract flying and quacking behavior to two interfaces, like so:

```java
public interface FlyBehavior {
    public abstract void fly();
}

public class FlyWithWings implements FlyBehavior { 
    public void fly() { /* Do some flying */}
}

public class DoNotFly implements FlyBehavior {
    public void fly() { /* Do not fly */}
}

// Same with quacking, and any other abstractable behavior

```

Note how we have classes that implement behavior, but this doesn't mean
that they will only have methods - they might have properties related
to that behavior, too.


## Implementing behavior

We can put together the duck like so:

```java
public abstract class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    public void performFly() {
        flyBehavior.fly();
    }
    
    // same with quack
}
```

FlyBehavior and QuackBehavior would then be instantiated in the Duck subclasses;
the relevant class conforming to that interface will be instantiated, ex:

`java flyBehavior = new doNotFly();`


## Setting behavior dynamically

We could further change an object at runtime by doing:

```java
public void setFlyBehavior(FlyBehavior fb) {
    flyBehavior = fb;
}
```


## The Strategy pattern

What we have seen is a Strategy pattern:

"Define a family of algorithms, encapsulate each one, and make them 
interchangeable. Strategy lets algorithms vary independently from clients that
use them"
