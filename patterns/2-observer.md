## Observer definition

An Observer defines a one-to-many dependency between objects so that when one
object changes state, all of its dependents are notified and updated
automatically.


## Skeleton definition

```java
public interface Subject {
    public abstract void registerObserver();
    public abstract void removeObserver();
    public abstract void notifyObservers();
}

public class ConcreteSubject implements Subject {
    // implement the interface methods
    // may also implement its own methods to get and set state
}

public interface Observer {
    public abstract void update();
}

public class ConcreteObserver implements Observer {
    // implements update()
    // plus any other relevatn methods
}
```


## Advantages of loose coupling

* We can add new observers at any time
* We never need to modify the subject to add new types of observers
* We can reuse subjects or observers independently of each other
* Changes to subject or observer will not affect the other


## Example implementation for subject

We use the example of a weather data object that notifies a series
of displays with data changes.

```java
public class ConcreteSubject implements Subject {
    private ArrayList<Observer> observers;
    private float temperature;
    private float humidity;
    // Any other properties relevant to the subject

    public WeatherData() { /* init ArrayList, etc  */}

    public void registerObserver(Observer o) { /* add to observers */}

    public void removeObserver(Observer o) { /* find and remove from list */}

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    } 

    public void measurementsChanged() {
        notifyObservers();
    }

    public void setMeasurements(float temperature, float humidity) {
        this.temperature = temperature;
        this.humidity = humidity;
        measurementsChanged();    
    }
}
```

## Example implemntation for an observer

```java
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() { /* Do whatever */}
}
```


## Java native implementation

Java implements the Observer pattern with the Observable class (a Subject) 
and Observer. 

A notable difference in Observable is setChanged() which needs
to be called before notifications are pused out. This way we can control
when to push out notifications (for example a program with many state updates
might only want to send notifications on certain thresholds)

Check HFDP p.67 for the Java implementaiton of Observable.

Another difference is how it allows for push or pull Observer models depending
on whether we pass an argument to notifyObservers() or not. 

Check HFDP p.68 for the pull implementation, as opposed to our push 
implementation above.


