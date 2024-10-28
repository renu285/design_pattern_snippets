### Mediator

Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. 
The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

~~~
#include <iostream>
#include <string>

class User {
public:
    virtual void sendMessage(const std::string& message) = 0;
    virtual void receiveMessage(const std::string& message) = 0;
};

class ChatMediator {
public:
    virtual void sendMessage(User* user, const std::string& message) = 0;
};

class UserImpl : public User {
public:
    UserImpl(ChatMediator* mediator, const std::string& name) : _mediator(mediator), _name(name) {}

    void sendMessage(const std::string& message) override {
        std::cout << _name << " says: " << message << std::endl;
        _mediator->sendMessage(this, message);
    }

    void receiveMessage(const std::string& message) override {
        std::cout << _name << " received: " << message << std::endl;
    }

private:
    ChatMediator* _mediator;
    std::string _name;
};

class ChatMediatorImpl : public ChatMediator {
public:
    void sendMessage(User* user, const std::string& message) override {
        // Logic to forward the message to other users
        for (auto& u : _users) {
            if (u != user) {
                u->receiveMessage(message);
            }
        }
    }

    void addUser(User* user) {
        _users.push_back(user);
    }

private:
    std::vector<User*> _users;
};

int main() {
    ChatMediatorImpl* mediator = new ChatMediatorImpl();
    UserImpl* user1 = new UserImpl(mediator, "User1");
    UserImpl* user2 = new UserImpl(mediator, "User2");

    mediator->addUser(user1);
    mediator->addUser(user2);

    user1->sendMessage("Hello, User2!");
    user2->sendMessage("Hi, User1!");

    delete user2;
    delete user1;
    delete mediator;

    return 0;
}

~~~