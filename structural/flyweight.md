## Flyweight 

The Flyweight design pattern is a structural pattern that aims to minimize memory usage or computational expenses by sharing 
as much as possible between similar objects. It's especially useful when you need to create a large number of similar 
objects that share common characteristics.

~~~
#include <iostream>
#include <map>
#include <string>

// Flyweight interface
class Character {
public:
    virtual void print() = 0;
};

// Concrete Flyweight
class ConcreteCharacter : public Character {
private:
    char symbol;

public:
    ConcreteCharacter(char symbol) : symbol(symbol) {}

    void print() override {
        std::cout << symbol;
    }
};

// Flyweight Factory
class CharacterFactory {
private:
    std::map<char, Character*> characters;

public:
    Character* getCharacter(char symbol) {
        if (characters.find(symbol) == characters.end()) {
            characters[symbol] = new ConcreteCharacter(symbol);
        }
        return characters[symbol];
    }

    ~CharacterFactory() {
        for (auto& pair : characters) {
            delete pair.second;
        }
        characters.clear();
    }
};

// Client code
int main() {
    CharacterFactory characterFactory;

    // Text to print
    std::string text = "Hello World!\n";

    // Print each character using flyweight objects
    for (char c : text) {
        if (c != ' ' && c != '\t' && c != '\n') {
            Character* character = characterFactory.getCharacter(c);
            character->print();
        } else {
            std::cout << c;
        }
    }

    return 0;
}

~~~