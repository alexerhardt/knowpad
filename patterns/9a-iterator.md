## Iterator principles

Imagine we have two different menu classes, a breakfast menu and a lunch menu.
One might implement them with two different collections, for example the lunch
menu might be based on an Array, while the breakfast menu is based on an
ArrayList. With an iterator, clients can iterate through the menus regardless
of the underlying implementation:

```java
Iterator iterator = breakfastMenu.createIterator();

while (iterator.hasNext()) {
    MenuItem menuItem = iterator.next();
}

// Same for the lunch menu
```


## Implementing an Iterator

Here's a minimal implementation:

```java
public interface Iterator {
    boolean hasNext();
    Object next();
}

public class LunchMenuIterator implements Iterator {
    MenuItem[] items;
    int position = 0;

    public DinerMenuIterator(MenuItem[] items) {
        this.items = items;
    }

    public MenuItem next() { /* return element at position, and increment it */ }

    public boolean hasNext() { /* checks if we've arrived at end-bound */ }
}

public class LunchMenu {
    MenuItem[] menuItems;

    public Iterator createIterator() {
        // Note how we pass our internal, concrete array
        return new LunchMenuIterator(menuItems);
    }
}
```

Implementing the same in a BreakfastMenu class, a client class (ex: a waitress)
could iterate through both:

```java
public class Waitress {
    BreakfastMenu breakfastMenu;
    LunchMenu lunchMenu;

    public Waitress(BreakfastMenu b, LunchMenu l) { /* ... */ }

    public void printMenu() {
        Iterator breakfastIterator = breakfastMenu.createIterator();
        Iterator lunchIterator = lunchMenu.createIterator();
        printMenu(breakfastIterator);
        printMenu(lunchIterator);
    }

    private void printMenu(Iterator it) {
        while(it.hasNext()) { /* iterate */
        }
        
    }
}
```

## Improving the design

We can further improve the design by using Java's own Iterator interface, 
and having a very simple interface that allows clients to get an iterator:

```java
public interface Menu {
    public Iterator<MenuItem> createIterator();
}
```

Now clients do not depend on the concrete subclasses, but on a
generalized interface.


### Iterator definition

The Iterator Pattern provides a way to access the elements of an aggregate object
sequentially without exposing its underlying representation.