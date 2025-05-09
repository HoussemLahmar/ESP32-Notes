# Système d'alerte d'arrosage pour plantes avec ESP32

Ce projet utilise un ESP32 combiné à un capteur DHT22 pour surveiller les conditions environnementales de vos plantes et vous alerter lorsqu'un arrosage est nécessaire.

## Composants utilisés
- ESP32 DevKit C
- Capteur de température et d'humidité DHT22
- LED verte
- Buzzer

## Fonctionnement

Le système surveille continuellement la température et l'humidité ambiantes. Lorsque les conditions deviennent défavorables pour les plantes (température supérieure à 30°C et humidité inférieure à 50%), il déclenche une série d'alertes :

1. Affichage du message "ARROSEZ VOS PLANTES" sur le moniteur série
2. Activation du buzzer pendant 4 secondes pour émettre une alerte sonore
3. Activation du niveau haut du buzzer pendant 4 secondes supplémentaires
4. Clignotement de la LED verte 5 fois (allumée 0,5 seconde, éteinte 0,5 seconde)

## Schéma de connexion
- Le capteur DHT22 est connecté à la broche 2 de l'ESP32
- La LED est connectée à la broche 4
- Le buzzer est connecté à la broche 5

## Code expliqué

```cpp
// Inclusion de la bibliothèque pour le capteur DHT
#include <DHT.h>

// Configuration des broches et du type de capteur
#define DHTPIN 2       // Broche de données du DHT22
#define DHTTYPE DHT22  // Type de capteur (DHT22/AM2302)
DHT dht(DHTPIN, DHTTYPE);

// Définition des broches pour les périphériques de sortie
#define BUZZER_PIN 5
#define LED_PIN 4

void setup() {
  Serial.begin(9600);    // Initialisation de la communication série
  dht.begin();           // Initialisation du capteur DHT
  pinMode(BUZZER_PIN, OUTPUT);  // Configuration du buzzer en sortie
  pinMode(LED_PIN, OUTPUT);     // Configuration de la LED en sortie
}

void loop() {
  delay(2000);  // Attente de 2 secondes entre les lectures
  
  // Lecture des valeurs du capteur
  float temperature = dht.readTemperature();
  float humidite = dht.readHumidity();
  
  // Vérification des conditions d'alerte (température > 30°C ET humidité < 50%)
  if (temperature > 30 && humidite < 50) {
    // Affichage du message d'alerte
    Serial.println("ARROSEZ VOS PLANTES");
    
    // Activation du buzzer avec une tonalité de 1000 Hz pendant 4 secondes
    tone(BUZZER_PIN, 1000);
    delay(4000);
    noTone(BUZZER_PIN);
    
    // Activation directe du buzzer pendant 4 secondes supplémentaires
    digitalWrite(BUZZER_PIN, HIGH);
    delay(4000);
    digitalWrite(BUZZER_PIN, LOW);
    
    // Clignotement de la LED 5 fois
    for (int i = 0; i < 5; i++) {
      digitalWrite(LED_PIN, HIGH);  // Allumage de la LED
      delay(500);
      digitalWrite(LED_PIN, LOW);   // Extinction de la LED
      delay(500);
    }
  }
}
```

## Simulation
Ce projet est entièrement simulable sur la plateforme Wokwi. Vous pouvez accéder à la simulation à l'adresse suivante : [https://wokwi.com/projects/386748951564342273](https://wokwi.com/projects/386748951564342273)

## Applications pratiques
- Surveillance des plantes d'intérieur
- Systèmes d'irrigation automatisés
- Stations météo domestiques
- Projets éducatifs sur l'électronique et l'IoT

Ce système simple mais efficace peut être étendu avec des fonctionnalités supplémentaires comme la connexion WiFi pour envoyer des notifications sur smartphone ou l'ajout d'un écran LCD pour afficher les valeurs en temps réel.