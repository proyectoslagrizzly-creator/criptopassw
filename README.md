# criptopassw
# Secure Password Analyzer &amp; Generator
#include <iostream>
#include <string>
#include <random>
#include <cctype>

class SecurityManager {
public:
    // Evalúa la fortaleza de una contraseña
    std::string analyzePassword(const std::string& password) {
        int score = 0;
        bool hasLower = false, hasUpper = false, hasDigit = false, hasSpecial = false;

        if (password.length() >= 8) score++;
        if (password.length() >= 12) score++;

        for (char ch : password) {
            if (islower(ch)) hasLower = true;
            else if (isupper(ch)) hasUpper = true;
            else if (isdigit(ch)) hasDigit = true;
            else hasSpecial = true;
        }

        if (hasLower) score++;
        if (hasUpper) score++;
        if (hasDigit) score++;
        if (hasSpecial) score++;

        if (score < 3) return "Débil (Vulnerable a ataques de fuerza bruta)";
        if (score < 5) return "Moderada (Mejorable)";
        return "Fuerte (Segura)";
    }

    // Genera una contraseña segura
    std::string generatePassword(int length) {
        const std::string chars = 
            "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_=+";
        std::random_device rd;
        std::mt19937 generator(rd());
        std::uniform_int_distribution<> distribution(0, chars.size() - 1);

        std::string password;
        for (int i = 0; i < length; ++i) {
            password += chars[distribution(generator)];
        }
        return password;
    }
};

int main() {
    SecurityManager secManager;
    int choice;

    std::cout << "=== Herramienta de Seguridad de Contraseñas ===\n";
    std::cout << "1. Analizar fortaleza de contraseña\n";
    std::cout << "2. Generar contraseña segura\n";
    std::cout << "Elige una opción: ";
    std::cin >> choice;

    if (choice == 1) {
        std::string pwd;
        std::cout << "Ingresa la contraseña a evaluar: ";
        std::cin >> pwd;
        std::cout << "Resultado: " << secManager.analyzePassword(pwd) << "\n";
    } else if (choice == 2) {
        int len;
        std::cout << "Longitud deseada (min 8): ";
        std::cin >> len;
        if (len < 8) len = 8;
        std::cout << "Contraseña generada: " << secManager.generatePassword(len) << "\n";
    } else {
        std::cout << "Opción no válida.\n";
    }

    return 0;
}
