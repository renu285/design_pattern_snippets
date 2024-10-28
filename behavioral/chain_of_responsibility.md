
## Chain of Responsibility

The Chain of Responsibility design pattern is a behavioral pattern that allows you to pass requests along a chain of handlers. 
Each handler decides either to process the request or to pass it to the next handler in the chain.

Conceptual Example: Let's consider a simple customer support system. When a customer raises an issue, it goes through different levels of support:

1.  Automated Support (handles common issues)
2.  Level 1 Support (handles basic technical issues)
3.  Level 2 Support (handles more complex issues)
4.  Manager (handles escalated or unresolved issues)

Each level tries to resolve the issue. If it can't, it passes the issue to the next level.

Here's a simple C++ code snippet demonstrating this concept:
~~~
#include <iostream>
#include <string>

class SupportHandler {
protected:
    SupportHandler* nextHandler;

public:
    SupportHandler() : nextHandler(nullptr) {}

    void setNext(SupportHandler* handler) {
        nextHandler = handler;
    }

    virtual void handleRequest(const std::string& issue) = 0;
};

class AutomatedSupport : public SupportHandler {
public:
    void handleRequest(const std::string& issue) override {
        if (issue == "password reset") {
            std::cout << "Automated Support: Password reset instructions sent.\n";
        } else if (nextHandler) {
            nextHandler->handleRequest(issue);
        }
    }
};

class Level1Support : public SupportHandler {
public:
    void handleRequest(const std::string& issue) override {
        if (issue == "basic technical issue") {
            std::cout << "Level 1 Support: Basic technical issue resolved.\n";
        } else if (nextHandler) {
            nextHandler->handleRequest(issue);
        }
    }
};

class Level2Support : public SupportHandler {
public:
    void handleRequest(const std::string& issue) override {
        if (issue == "complex technical issue") {
            std::cout << "Level 2 Support: Complex technical issue resolved.\n";
        } else if (nextHandler) {
            nextHandler->handleRequest(issue);
        }
    }
};

class Manager : public SupportHandler {
public:
    void handleRequest(const std::string& issue) override {
        std::cout << "Manager: Issue escalated and will be investigated.\n";
    }
};

int main() {
    AutomatedSupport automated;
    Level1Support level1;
    Level2Support level2;
    Manager manager;

    automated.setNext(&level1);
    level1.setNext(&level2);
    level2.setNext(&manager);

    automated.handleRequest("password reset");
    automated.handleRequest("basic technical issue");
    automated.handleRequest("complex technical issue");
    automated.handleRequest("very complex issue");

    return 0;
}
~~~

Another Example. Let's consider a document approval system in a corporate setting. In this scenario, different levels of management need to approve expenses based on the amount.

Conceptual Example: Imagine an expense approval system with the following chain:

1.  Team Lead (can approve expenses up to $1000)
2.  Manager (can approve expenses up to $5000)
3.  Director (can approve expenses up to $20000)
4.  CEO (can approve any amount)

Each level checks if they have the authority to approve the expense. If not, they pass it up the chain.

Here's a C++ code snippet demonstrating this concept:
~~~
#include <iostream>
#include <string>

class ApprovalHandler {
protected:
    ApprovalHandler* nextHandler;
    int approvalLimit;

public:
    ApprovalHandler(int limit) : nextHandler(nullptr), approvalLimit(limit) {}

    void setNext(ApprovalHandler* handler) {
        nextHandler = handler;
    }

    virtual void processRequest(int amount) = 0;
};

class TeamLead : public ApprovalHandler {
public:
    TeamLead() : ApprovalHandler(1000) {}

    void processRequest(int amount) override {
        if (amount <= approvalLimit) {
            std::cout << "Team Lead approved $" << amount << " expense.\n";
        } else if (nextHandler) {
            std::cout << "Team Lead passes request to Manager.\n";
            nextHandler->processRequest(amount);
        }
    }
};

class Manager : public ApprovalHandler {
public:
    Manager() : ApprovalHandler(5000) {}

    void processRequest(int amount) override {
        if (amount <= approvalLimit) {
            std::cout << "Manager approved $" << amount << " expense.\n";
        } else if (nextHandler) {
            std::cout << "Manager passes request to Director.\n";
            nextHandler->processRequest(amount);
        }
    }
};

class Director : public ApprovalHandler {
public:
    Director() : ApprovalHandler(20000) {}

    void processRequest(int amount) override {
        if (amount <= approvalLimit) {
            std::cout << "Director approved $" << amount << " expense.\n";
        } else if (nextHandler) {
            std::cout << "Director passes request to CEO.\n";
            nextHandler->processRequest(amount);
        }
    }
};

class CEO : public ApprovalHandler {
public:
    CEO() : ApprovalHandler(std::numeric_limits<int>::max()) {}

    void processRequest(int amount) override {
        std::cout << "CEO approved $" << amount << " expense.\n";
    }
};

int main() {
    TeamLead teamLead;
    Manager manager;
    Director director;
    CEO ceo;

    teamLead.setNext(&manager);
    manager.setNext(&director);
    director.setNext(&ceo);

    teamLead.processRequest(800);
    teamLead.processRequest(4800);
    teamLead.processRequest(12000);
    teamLead.processRequest(30000);

    return 0;
}
~~~