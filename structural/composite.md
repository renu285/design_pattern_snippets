## Composite pattern 

The composite design pattern is a structural pattern that lets you compose objects into tree structures 
to represent part-whole hierarchies.In simpler terms, it allows you to treat individual objects and 
compositions of objects uniformly.


~~~
#include <iostream>
#include <vector>

// Component interface
class Employee {
public:
    virtual void showDetails() const = 0;
    virtual ~Employee() {}
};

// Leaf class (individual object)
class IndividualEmployee : public Employee {
private:
    std::string name;
    std::string position;

public:
    IndividualEmployee(const std::string& name, const std::string& position)
        : name(name), position(position) {}

    void showDetails() const override {
        std::cout << "Name: " << name << ", Position: " << position << std::endl;
    }
};

// Composite class (composition of objects)
class Department : public Employee {
private:
    std::string name;
    std::vector<Employee*> employees;

public:
    Department(const std::string& name) : name(name) {}

    void add(Employee* employee) {
        employees.push_back(employee);
    }

    void showDetails() const override {
        std::cout << "Department: " << name << std::endl;
        for (const auto& employee : employees) {
            employee->showDetails();
        }
    }
};

int main() {
    IndividualEmployee emp1("John Doe", "Manager");
    IndividualEmployee emp2("Jane Smith", "Developer");

    Department techDepartment("Tech Department");
    techDepartment.add(&emp1);
    techDepartment.add(&emp2);

    IndividualEmployee emp3("Alice Johnson", "HR Manager");
    Department hrDepartment("HR Department");
    hrDepartment.add(&emp3);

    Department company("Our Company");
    company.add(&techDepartment);
    company.add(&hrDepartment);

    company.showDetails();

    return 0;
}

~~~