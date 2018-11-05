## A gumball machine example

We create a finite-state machine based on a gumball machine, like so:

```java
public class GumballMachine {
    final static int SOLD_OUT = 0;
    final static int NO_QUARTER = 1;
    final static int HAS_QUARTER = 2;
    final static int SOLD = 3;

    public void insertQuarter() {
        // depends on state
    }

    public void ejectQuarter() {
        // depends on state
    }

    public void turnCrank() {
        // depends on state
    }

    public void dispense() {
        // depends on state
    }
}
```

The problem is that this class depends heavily on state. If we want to add a
new state (for example, a lottery mechanism such that 10% of purchases
get a new ball) things get messy.


## Refactoring into a State pattern

We are going to encapsulate each state into its own class, and each class will
implement a common interface.

An example state:

```java
public class NoQuarterState implements State {
    GumballMachine gumballMachine;

    public NoQuarterState(GumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
        System.out.println("You inserted a quarter");
        gumballMachine.setState(gumballMachine.getHasQuarterState());
    }

    public void ejectQuarter() {
        System.out.println("You haven't inserted a quarter");
    }

    public void turnCrank() {
        System.out.println("You turned, but there's no quarter");
    }

    public void dispense() {
        System.out.println("You need to pay first");
    }
}

public class HasQuarterState implements State {
    GumballMachine gumballMachine;

    public HasQuarterState(GumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
        System.out.println("You can't insert another quarter.");
    }

    public void ejectQuarter() {
        System.out.println("Quarter returned");
        gumballMachine.setState(gumballMachine.getNoQuarterState());
    }

    public void turnCrank() {
        System.out.println("You turned...");
        gumballMachine.setState(gumballMachine.getSoldState());
    }

    public void dispense() {
        System.out.println("No gumball dispensed");
    }
}

public class SoldState implements State {
    public void insertQuarter() {
        System.out.println("Please wait, we're already giving you a gumball");
    }

    public void ejectQuarter() {
        System.out.println("Sorry, you already turned the crank.");
    }

    public void turnCrank() {
        System.out.printl("Turning twice doesn't get you another gumball!");
    }

    public void dispense() {
        gumballMachine.releaseBall();
        if (gumballMachine.getCount() > 0) {
            gumballMachine.setState(gumballMachine.getNoQuarterState());
        }
        else {
            System.out.println("Oops, out of gumballs!");
            gumballMachine.setState(gumballMachine.getSoldOutState());
        }
    }
}

// Missing sold out state

```

Now we can implement the GumballMachine class. Note how it has members for each
of the states, and it simply calls methods on the current state. The current
state (which holds a reference to the GumballMachine) is in charge of performing
the transitions.

```java
public class GumballMachine {
    State soldOutState;
    State noQuarterState;
    State hasQuarterState;
    State soldState;

    State state; // current state
    int count = 0;

    public GumballMachine(int numberGumballs) {
        soldOutState = new SoldOutState(this);
        noQuarterState = new NoQuarterState(this);
        hasQuarterState = new HasQuarterState(this);
        soldState = new SoldState(this);

        this.count = numberGumballs;
        if (numberGumballs > 0) {
            state = NoQuarterState;
        }
        else {
            state = soldOutState;
        }
    }

    public void insertQuarter() {
        state.insertQuarter();
    }

    public void ejectQuarter() {
        state.ejectQuarter();
    }
    
    public void turnCrank() {
        state.turnCrank();
        state.dispense();
    }

    void setState(State state) {
        this.state = state;
    }

    void releaseBall() {
        System.out.println("A gumball comes rolling out of the slot");
        if (count != 0) {
            count = count = -1;
        }
    }
    
    // getters, including those for state
}
```

## Extending the GumballMachine with a new state

The GumballMachine can be extended by adding a new State class and modifying
the Context class.

```java
public class GumballMachine {
    // ... previous states ...
    State winnerState;

    // add new getter
}

public class WinnerState implements State {
    // all methods are similar to SoldState
    
    public void dispense() {
        // We dispense 2 if there are enough balls
    }
}
```

## More notes on the pattern

#### State transition implementation

State transitions do not necessarily belong in the concrete state classes;
sometimes we will want to implement them in the context class. 

Implementing transitions in the state classes creates dependencies between them.

If you implement transitions on the Context, then the concrete state classes
should be closed for modification and vice versa.

#### Other notes

- Clients should never interact with states

- States can be shared across Context instances. Context state instance variables
can be made static to this effect.
