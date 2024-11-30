#include <iostream>
#include <string>
#include <vector>
#include <memory>
// Klasa bazowa Log
class Log {
public:
   virtual ~Log() = default; // wirtualny destruktor
   virtual std::string getMessage() const = 0; // Pobieranie wiadomości logu
};
// Klasa reprezentująca log info
class InfoLog : public Log {
   std::string message; // treść 
public:
   explicit InfoLog(const std::string& msg) : message(msg) {}
   std::string getMessage() const override { return "INFO: " + message; }
};
// Klasa reprezentująca log error
class ErrorLog : public Log {
   std::string message; // tresc
public:
   explicit ErrorLog(const std::string& msg) : message(msg) {}
   std::string getMessage() const override { return "ERROR: " + message; }
};
// Fabryka logów
class LogFactory {
public:
   // Tworzy odpowiedni log na podstawie typu
   static std::unique_ptr<Log> createLog(const std::string& type, const std::string& message) {
       if (type == "info") {
           return std::make_unique<InfoLog>(message);
       } else if (type == "error") {
           return std::make_unique<ErrorLog>(message);
       } else {
           throw std::invalid_argument("Nie ma takiego numeru");
       }
   }
};
// Klasa Logger, Singleton
class Logger {
private:
   static Logger* instance; // wskaźnik 
   std::vector<std::unique_ptr<Log>> logs; // wektor przechowujący wszystkie logi
   Logger() = default; // konstruktor-prywatny
public:
   // Zwraca wskaźnik na jedyną instancję Loggera.
   static Logger* getInstance() {
       if (instance == nullptr) {
           instance = new Logger();
       }
       return instance;
   }
   // Dodaje nowy log, nie nadpisując istniejących logów.
   void logMessage(const std::string& type, const std::string& message) {
       logs.push_back(LogFactory::createLog(type, message)); // Dodaje nowy log do wektora.
   }
   // Wyświetla wszystkie zapisane logi w kolejności ich dodania.
   void displayLogs() const {
       if (logs.empty()) {
           std::cout << "Brak zapisanych logów." << std::endl;
       } else {
           for (const auto& log : logs) {
               std::cout << log->getMessage() << std::endl;
           }
       }
   }
};
// Inicjalizacja statycznego wskaźnika na Loggera
Logger* Logger::instance = nullptr;
// Funkcja główna
int main() {
    // Pobranie instancji Loggera
   Logger* logger = Logger::getInstance(); 
   // Dodanie przykładowych logów
   logger->logMessage("info", "Tadek ukradl motur.");
   logger->logMessage("error", "Nie mozna znalesc motura");
   logger->logMessage("info", "Tadek nie odda motura.");
   // Wyświetlenie zapisanych logów.
   std::cout << "Zapisane logi:\n";
   logger->displayLogs();
   // Test: Dodanie kolejnego logu i ponowne wyświetlenie
   logger->logMessage("error", "Tadek nie ma motura.");
   std::cout << "\nPo dodaniu nowego logu:\n";
   logger->displayLogs();
   return 0;
}