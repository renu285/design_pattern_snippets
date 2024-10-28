## Decorator

Decorator is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

~~~
#include <iostream>
#include <memory>
#include <string>

// Component interface
class Coffee {
public:
    virtual std::string getDescription() const = 0;
    virtual double cost() const = 0;
    virtual ~Coffee() {}
};

// Concrete component
class SimpleCoffee : public Coffee {
public:
    std::string getDescription() const override {
        return "Simple Coffee";
    }

    double cost() const override {
        return 1.0;
    }
};

// Decorator base class
class CoffeeDecorator : public Coffee {
protected:
    std::unique_ptr<Coffee> decoratedCoffee;
public:
    CoffeeDecorator(std::unique_ptr<Coffee> coffee) : decoratedCoffee(std::move(coffee)) {}
    std::string getDescription() const override {
        return decoratedCoffee->getDescription();
    }
    double cost() const override {
        return decoratedCoffee->cost();
    }
};

// Concrete decorators
class MilkDecorator : public CoffeeDecorator {
public:
    MilkDecorator(std::unique_ptr<Coffee> coffee) : CoffeeDecorator(std::move(coffee)) {}
    std::string getDescription() const override {
        return decoratedCoffee->getDescription() + ", Milk";
    }
    double cost() const override {
        return decoratedCoffee->cost() + 0.5;
    }
};

class SugarDecorator : public CoffeeDecorator {
public:
    SugarDecorator(std::unique_ptr<Coffee> coffee) : CoffeeDecorator(std::move(coffee)) {}
    std::string getDescription() const override {
        return decoratedCoffee->getDescription() + ", Sugar";
    }
    double cost() const override {
        return decoratedCoffee->cost() + 0.2;
    }
};

int main() {
    // Original coffee
    std::unique_ptr<Coffee> coffee = std::make_unique<SimpleCoffee>();
    std::cout << "Description: " << coffee->getDescription() << ", Cost: $" << coffee->cost() << std::endl;

    // Decorated coffees
    std::unique_ptr<Coffee> coffeeWithMilk = std::make_unique<MilkDecorator>(std::make_unique<SimpleCoffee>());
    std::cout << "Description: " << coffeeWithMilk->getDescription() << ", Cost: $" << coffeeWithMilk->cost() << std::endl;

    std::unique_ptr<Coffee> coffeeWithMilkAndSugar = std::make_unique<SugarDecorator>(std::make_unique<MilkDecorator>(std::make_unique<SimpleCoffee>()));
    std::cout << "Description: " << coffeeWithMilkAndSugar->getDescription() << ", Cost: $" << coffeeWithMilkAndSugar->cost() << std::endl;

    return 0;
}


~~~
