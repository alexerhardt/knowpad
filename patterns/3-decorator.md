## Sub-class proliferation

If we have a class with many possible variations, for example, a Coffee class
which can have milk, sugar and all kinds of condiments in different permutations,
we can end up with a clusterfuck of inheritance.

Instead, we should try to modify the objects at runtime.


## The Open-Closed Principle

"Classes should be open for extension, but closed for modification"
Largely means that we shouldn't need to recode a class to extend it - we should
be able to do it by attaching new classes.


## Decorator Definition

The Decorator Pattern attaches additional responsibilities to an object
dynamically. Decorators provide a flexible alternative to subclassing for 
extending functionality.


## Example

Taken from Wikipedia:
https://en.wikipedia.org/wiki/Decorator_pattern

```java
public interface Window {
    void draw();
    String getDescription();
}

class SimpleWindow implements Window {
    public void draw() { /* draw window */ }

    public String getDescription() { return "simple window"; }
}

abstract class WindowDecorator implements Window {
    protected Window windowToeDecorate;

    public WindowDecorator (Window windowToDecorate) {
        this.windowToDecorate = windowToDecorate;
    }

    public void draw() {
        windowToDecorate.draw();
    }

    public String getDescription() {
        return windowToDecorate.getDescription();
    }
}

public class VerticalScrollbarDecorator extends WindowDecorator {
    public VerticalScrollbarDecorator (Window windowToDecorate) {
        super(windowToDecorate);
    }

    public void draw() {
        super.draw();
        drawVerticalScrollBar();
    }

    private void drawVerticalScrollBar() {
        // draw the vertical scrollbar
    }

    public String getDescription() {
        return super.getDescription() + ", with vertical scrollbars";
    }
}

public class HorizontalScrollbarDecorator extends WindowDecorator {
    // Analogous to VerticalScrollBarDecorator
}

public class DecoratedWindowTest {
    public static void main(String[] args) {
        Window decorated = new HorizontalScrollBarDecorator (
            new VerticalScrollbarDecorator ( new SimpleWindow() )
        );
    }
}
```