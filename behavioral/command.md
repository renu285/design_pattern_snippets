
## Command design pattern

- Turns a request or a behavior into stand-alone object that contains everything about that request
- Encapsulates all relevant information required to perform that action or trigger an event
- It separates the object that invokes the operation from the object that actually performs the operation.

~~~
#include <iostream>
#include <vector>
#include <memory>

// Command interface
class Command {
public:
    virtual void execute() = 0;
    virtual void undo() = 0;
    virtual ~Command() = default;
};

// Receiver
class Light {
public:
    void turnOn() { std::cout << "Light is on\n"; }
    void turnOff() { std::cout << "Light is off\n"; }
};

// Concrete Command for turning on the light
class LightOnCommand : public Command {
    Light& light;
public:
    LightOnCommand(Light& l) : light(l) {}
    void execute() override { light.turnOn(); }
    void undo() override { light.turnOff(); }
};

// Concrete Command for turning off the light
class LightOffCommand : public Command {
    Light& light;
public:
    LightOffCommand(Light& l) : light(l) {}
    void execute() override { light.turnOff(); }
    void undo() override { light.turnOn(); }
};

// Invoker
class RemoteControl {
    std::vector<std::unique_ptr<Command>> commands;
    std::vector<std::unique_ptr<Command>> undoCommands;
public:
    void setCommand(std::unique_ptr<Command> cmd) {
        commands.push_back(std::move(cmd));
    }
    void pressButton(int slot) {
        if (slot < commands.size()) {
            commands[slot]->execute();
            undoCommands.push_back(std::move(commands[slot]));
            commands.erase(commands.begin() + slot);
        }
    }
    void pressUndo() {
        if (!undoCommands.empty()) {
            undoCommands.back()->undo();
            undoCommands.pop_back();
        }
    }
};

int main() {
    Light light;
    RemoteControl remote;

    remote.setCommand(std::make_unique<LightOnCommand>(light));
    remote.setCommand(std::make_unique<LightOffCommand>(light));

    remote.pressButton(0);  // Turn on
    remote.pressButton(0);  // Turn off
    remote.pressUndo();     // Undo (turn on)

    return 0;
}
~~~
