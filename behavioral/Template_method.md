### Template Method

#### Core Concept:

- The Template Method pattern defines the skeleton of an algorithm in a base class
- It lets subclasses override specific steps without changing the algorithm's structure

~~~
// Beverage preparation example in C++
#include <iostream>
#include <string>

class Beverage {
protected:
    // These are the "primitive" operations that can be overridden
    virtual void boilWater() {
        std::cout << "Boiling water" << std::endl;
    }
    
    virtual void brew() = 0;  // Pure virtual - must be implemented by subclasses
    
    virtual void pourInCup() {
        std::cout << "Pouring into cup" << std::endl;
    }
    
    virtual void addCondiments() = 0;  // Pure virtual - must be implemented by subclasses
    
    virtual bool customerWantsCondiments() {
        return true;  // Hook method - can be overridden
    }

public:
    // This is the template method
    void prepareBeverage() {
        boilWater();
        brew();
        pourInCup();
        if (customerWantsCondiments()) {
            addCondiments();
        }
    }
    
    virtual ~Beverage() = default;
};

class Coffee : public Beverage {
protected:
    void brew() override {
        std::cout << "Dripping coffee through filter" << std::endl;
    }
    
    void addCondiments() override {
        std::cout << "Adding sugar and milk" << std::endl;
    }
    
    bool customerWantsCondiments() override {
        std::cout << "Would you like milk and sugar with your coffee? (y/n): ";
        char response;
        std::cin >> response;
        return (response == 'y' || response == 'Y');
    }
};

class Tea : public Beverage {
protected:
    void brew() override {
        std::cout << "Steeping the tea" << std::endl;
    }
    
    void addCondiments() override {
        std::cout << "Adding lemon" << std::endl;
    }
};

int main() {
    Coffee coffee;
    Tea tea;
    
    std::cout << "\nMaking coffee..." << std::endl;
    coffee.prepareBeverage();
    
    std::cout << "\nMaking tea..." << std::endl;
    tea.prepareBeverage();
    
    return 0;
}
~~~
