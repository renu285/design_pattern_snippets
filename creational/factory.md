## Factory 

Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

The Factory Method pattern offers several benefits:

-   **Loose coupling:** Client code doesn't need to know the specific concrete shape classes.
-   **Flexibility:** New shapes can be added without modifying the client code or existing factories.
-   **Centralized control:** Shape creation logic is centralized in the factory, making it easier to control and manage.

~~~
#include <iostream>
#include <memory>

// Abstract product (base class for shapes)
class Shape {
public:
    virtual ~Shape() = default;
    virtual void draw() const = 0;
};

// Concrete products (derived classes for specific shapes)
class Circle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a circle..." << std::endl;
    }
};

class Square : public Shape {
public:
    void draw() const override {  
        std::cout << "Drawing a square..." << std::endl;
    }
};

class Triangle : public Shape {
public:
    void draw() const override {  
        std::cout << "Drawing a triangle..." << std::endl;
    }
};

// Creator/Factory interface
class ShapeFactory {
public:
    virtual std::unique_ptr<Shape> createShape(const std::string& type) const = 0;
};

// Concrete creator/factory (factory for specific shapes)
class ShapeFactoryImpl : public ShapeFactory {
public:
    std::unique_ptr<Shape> createShape(const std::string& type) const override {
        if (type == "circle") {
            return std::make_unique<Circle>();
        } else if (type == "square") {
            return std::make_unique<Square>();
        } else if (type == "triangle") {
            return std::make_unique<Triangle>();
        } else {
            throw std::invalid_argument("Invalid shape type");
        }
    }
};

// Client code
int main() {
    ShapeFactory* factory = new ShapeFactoryImpl(); // Create a concrete factory

    std::unique_ptr<Shape> shape1 = factory->createShape("circle");
    shape1->draw();

    std::unique_ptr<Shape> shape2 = factory->createShape("square");
    shape2->draw();

    // Add more shapes here...

    delete factory;
    return 0;
}

~~~