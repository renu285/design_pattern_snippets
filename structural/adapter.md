## Understanding the Adapter Pattern

The Adapter pattern is a structural design pattern that allows incompatible interfaces to work together. It acts as a bridge between two classes, translating the methods of one class to the expected methods of the other. This enables you to reuse existing code that has a different interface or integrate third-party libraries seamlessly into your application.

### Common Use Cases

#### Legacy Code Integration: 
When you have existing code with a legacy interface that needs to be used in a new system with a modern interface, the Adapter pattern facilitates the integration by adapting the legacy code to the new interface.
#### Third-Party Library Compatibility: 
If you're working with a third-party library that has a different interface from the one you're using, the Adapter pattern can bridge the gap, allowing you to use the library's functionality without modifying its code.
Code Example: Shape Drawing

Here's a C++ example that demonstrates the Adapter pattern in the context of drawing different shapes:
~~~
C++
#include <iostream>
#include <string>

// Interface for drawing shapes
class Shape {
public:
    virtual void draw() const = 0;
};

// Concrete class for drawing a rectangle
class Rectangle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a rectangle..." << std::endl;
    }
};

// Concrete class for drawing a legacy circle (incompatible interface)
class LegacyCircle {
public:
    void legacyDrawCircle() const {
        std::cout << "Drawing a legacy circle (incompatible format)..." << std::endl;
    }
};

// Adapter class to make LegacyCircle compatible with the Shape interface
class CircleAdapter : public Shape {
private:
    LegacyCircle legacyCircle;
public:
    CircleAdapter(const LegacyCircle& circle) : legacyCircle(circle) {}

    void draw() const override {
        // Adapt the legacy circle's method to the Shape interface
        std::cout << "Adapting legacy circle for drawing..." << std::endl;
        legacyCircle.legacyDrawCircle();
    }
};

int main() {
    // Create shapes
    Rectangle rectangle;
    LegacyCircle legacyCircle;

    // Use the Shape interface for drawing
    Shape* shapes[] = {&rectangle, new CircleAdapter(legacyCircle)};
    for (Shape* shape : shapes) {
        shape->draw();
    }

    return 0;
}
~~~