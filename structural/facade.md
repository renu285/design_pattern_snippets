
## Facade 

Facade is a structural design pattern that provides a simplified interface to a library, 
a framework, or any other complex set of classes.

~~~
C++
#include <iostream>
#include <string>

// Subsystems (complex classes)
class Amplifier {
public:
    void turnOn() {
        std::cout << "Amplifier on" << std::endl;
    }

    void turnOff() {
        std::cout << "Amplifier off" << std::endl;
    }

    void setVolume(int volume) {
        std::cout << "Setting amplifier volume to " << volume << std::endl;
    }
};

class Tuner {
public:
    void on() {
        std::cout << "Tuner on" << std::endl;
    }

    void off() {
        std::cout << "Tuner off" << std::endl;
    }

    void setFrequency(double frequency) {
        std::cout << "Setting tuner frequency to " << frequency << " MHz" << std::endl;
    }
};

class DvdPlayer {
public:
    void on() {
        std::cout << "DVD player on" << std::endl;
    }

    void off() {
        std::cout << "DVD player off" << std::endl;
    }

    void playMovie(const std::string& movie) {
        std::cout << "Playing movie: " << movie << std::endl;
    }
};

class Projector {
public:
    void on() {
        std::cout << "Projector on" << std::endl;
    }

    void off() {
        std::cout << "Projector off" << std::endl;
    }

    void setWideScreenMode(bool enabled) {
        std::string mode = enabled ? "on" : "off";
        std::cout << "Setting projector wide screen mode " << mode << std::endl;
    }
};

// Facade (simplified interface)
class HomeTheaterFacade {
private:
    Amplifier* amp;
    Tuner* tuner;
    DvdPlayer* dvd;
    Projector* projector;

public:
    HomeTheaterFacade(Amplifier* a, Tuner* t, DvdPlayer* d, Projector* p) :
        amp(a), tuner(t), dvd(d), projector(p) {}

    void watchMovie(const std::string& movie) {
        std::cout << "Preparing for movie: " << movie << std::endl;

        amp->turnOn();
        amp->setVolume(5);
        projector->on();
        projector->setWideScreenMode(true);
        tuner->off();
        dvd->on();
        dvd->playMovie(movie);
    }

    void endMovie() {
        std::cout << "\nEnding movie..." << std::endl;

        dvd->off();
        projector->off();
        amp->turnOff();
    }
};

int main() {
    Amplifier amp;
    Tuner tuner;
    DvdPlayer dvd;
    Projector projector;

    HomeTheaterFacade homeTheater(&amp, &tuner, &dvd, &projector);

    homeTheater.watchMovie("The Shawshank Redemption");
    homeTheater.endMovie();

    return 0;
}
~~~