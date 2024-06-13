#include <SFML/Graphics.hpp>
#include <iostream>
#include <string>

sf::Color parseColor(const std::string& colorStr) {
    int r, g, b;
    sscanf(colorStr.c_str(), "%d %d %d", &r, &g, &b);
    return sf::Color(r, g, b);
}

int main() {
    std::string windowColorStr, textColorStr, text;
    int width = 800, height = 600;

    std::cout << "Enter window color (R G B): ";
    std::getline(std::cin, windowColorStr);

    std::cout << "Enter text color (R G B): ";
    std::getline(std::cin, textColorStr);

    std::cout << "Enter text to display: ";
    std::getline(std::cin, text);

    sf::Color windowColor = parseColor(windowColorStr);
    sf::Color textColor = parseColor(textColorStr);

    sf::RenderWindow window(sf::VideoMode(width, height), "SFML Example");

    sf::Font font;
    if (!font.loadFromFile("path/to/font.ttf")) {
        std::cerr << "Failed to load font!" << std::endl;
        return -1;
    }

    sf::Text sfText;
    sfText.setFont(font);
    sfText.setString(text);
    sfText.setCharacterSize(24);
    sfText.setFillColor(textColor);

    sf::FloatRect textRect = sfText.getLocalBounds();
    sfText.setOrigin(textRect.left + textRect.width / 2.0f,
                     textRect.top + textRect.height / 2.0f);
    sfText.setPosition(sf::Vector2f(width / 2.0f, height / 2.0f));

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
                window.close();
            }
        }

        window.clear(windowColor);
        window.draw(sfText);
        window.display();
    }

    return 0;
}
