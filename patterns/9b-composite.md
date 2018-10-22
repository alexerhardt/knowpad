## Composite Pattern Definition

The Composite Pattern allows to compose objects into tree structures to 
represent part-whole hierarchies. Composite lets clients treat individual
objects and compositions of objects uniformly.

We essentially organize classes as trees, with leaves and nodes. Nodes contain
other subobjects; leaves are terminal objects.

The definition of our composite class is *recursive*


## Example Implementation

Taken from HFDP p.368-369, this is a reduced implementation:

```java
public abstract class MenuComponent {
    // We provide default implementations that throw 
    // so that subclasses do not need to implement
    public void add(MenuComponent menuComponent) {
        throw new UnsupportedOperationException();
    }

    // Same here, add Exceptions
    public void remove(MenuComponent menuComponent) {}
    public MenuComponent getChild(int i) {}
    public String getName() {}
    public String getDescription() {}
    public double getPrice() {}
    public boolean isVegetarian() {}
    public void print() {}
}

// This is a "leaf"
public class MenuItem extends MenuComponent {
```