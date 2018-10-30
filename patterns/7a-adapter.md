## Basic Purpose

The Adapter serves as a bridge between a system and an interface, allowing them
to communicate without having to change either. An example would be an existing
system that expects a certain interface from a vendor, but in a re-release,
the vendor changes the interface. By inserting an Adapter between the two,
we can allow the systems to interoperate without having to change their respective
codebases.

## Some Useful Notes

* Creating Adapters can be a lot of work - as big as the interface we are trying
to support. However, it can still be significantly less than reworking an entire
client.

* An Adapter may need to wrap several classes. 

* In multiple-inheritance languages we have object adapters on top of class adapters.

* Although both an Adapter and a Decorator wrap objects, the crucial difference
is that the former conforms to an interface whereas the latter adds new responsibilities
to the decorated object. 

## An example

Here's an example of an Adapter that wraps Enumerations, a deprecated precursor
to Java Iterators.

```java
public class EnumerationIterator implements Iterator<Object> {
    Enumeration<?> enumeration;

    public EnumerationIterator(Enumeration<?> enumeration) {
        this.enumeration = enumeration;
    }

    public boolean hasNext() {
        return enumeration.hasMoreElements();
    }

    public Object next() {
        return enumeration.nextElement();
    }

    // This did not exist with Enumerations
    public void remove() {
        throw new UnsupportedOperationException();
    }
}
```