# Programmation ESP32
## Analogique et PWM

*Par Houssem-eddine LAHMER*  
*Houssemeddine.lahmer@outlook.com*

## Table des matières

1. [Introduction](#introduction)
2. [Entrées/Sorties Analogiques](#entréessorties-analogiques)
3. [Modulation de Largeur d'Impulsion (PWM)](#modulation-de-largeur-dimpulsion-pwm)
4. [Applications Avancées](#applications-avancées)
5. [Bonnes Pratiques](#bonnes-pratiques)
6. [Ressources et Conclusion](#ressources-et-conclusion)

## Introduction

### Introduction à l'ESP32

- L'ESP32 est un microcontrôleur avec Wi-Fi et Bluetooth intégrés
- Développé par Espressif Systems
- Puissant et économe en énergie
- Architecture double cœur Xtensa LX6 32 bits
- Fréquence d'horloge jusqu'à 240 MHz
- Parfait pour les projets IoT et les applications embarquées

## Entrées/Sorties Analogiques

### Entrées Analogiques - ADC

- ADC (Analog-to-Digital Converter)
- ESP32 a 18 canaux ADC
- Résolution configurable (9-12 bits)
- Plage de tension d'entrée : 0 à 3.3V (par défaut)
- Fonction principale : `analogRead()`
- Applications : capteurs analogiques, potentiomètres, photorésistances

### Entrées Analogiques - Caractéristiques

- 2 unités ADC indépendantes (ADC1 et ADC2)
- ADC1 : Peut être utilisé à tout moment
- ADC2 : Ne peut pas être utilisé si WiFi est actif
- Résolution configurable:
  - 9 bits (0-511)
  - 10 bits (0-1023)
  - 11 bits (0-2047)
  - 12 bits (0-4095)

### Configuration de l'ADC

```cpp
// Configuration de la résolution ADC
analogReadResolution(12);  // 12 bits (0-4095)

// Lire une valeur analogique
int sensorValue = analogRead(34);  // Pin GPIO34

// Atténuation (adapte la plage d'entrée)
analogSetAttenuation(ADC_11db);  // Plage 0-3.3V
```

### Projet 1: Lecture d'un potentiomètre

**Objectif:** Lire et afficher la valeur d'un potentiomètre

**Matériel simulé:**
- ESP32
- Potentiomètre
- Moniteur série

**Concepts:**
- Configuration de l'ADC
- Lecture de valeurs analogiques
- Affichage des résultats
- Mappage de valeurs

### Projet 2: Capteur de lumière

**Objectif:** Créer un système qui réagit à la luminosité

**Matériel simulé:**
- ESP32
- Photorésistance (LDR)
- LED
- Résistances

**Concepts:**
- Lecture de capteur analogique
- Conditionner le signal d'entrée
- Prise de décision basée sur des seuils
- Contrôle LED en fonction de la luminosité

**Wokwi Link** https://wokwi.com/projects/378622524029754369

### Projet 3: Thermomètre

**Objectif:** Mesurer la température avec un capteur analogique

**Matériel simulé:**
- ESP32
- Thermistance ou capteur de température analogique
- Afficheur LCD

**Concepts:**
- Lecture de capteur
- Calcul de conversion tension-température
- Filtrage des mesures
- Affichage sur écran LCD

### Sorties analogiques sur ESP32

- L'ESP32 n'a pas de vrai DAC sur tous les pins
- 2 canaux DAC (8 bits) sur GPIO25 et GPIO26
- DAC: Digital-to-Analog Converter
- La plupart des "sorties analogiques" utilisent PWM
- Fonction pour DAC: `dacWrite(pin, value)`
- Valeurs de 0 (0V) à 255 (3.3V)

### Utilisation du DAC

```cpp
// Générer une tension analogique avec le DAC
const int dacPin = 25;  // GPIO25 ou GPIO26 uniquement

void setup() {
  Serial.begin(115200);
}

void loop() {
  // Sortie 1.65V (milieu de l'échelle)
  dacWrite(dacPin, 127);
  delay(1000);
  
  // Sortie 3.3V (maximum)
  dacWrite(dacPin, 255);
  delay(1000);
  
  // Sortie 0V
  dacWrite(dacPin, 0);
  delay(1000);
}
```

## Modulation de Largeur d'Impulsion (PWM)

### PWM - Principes fondamentaux

- PWM = Pulse Width Modulation (Modulation de Largeur d'Impulsion)
- Simulation d'une sortie analogique avec des signaux numériques
- Contrôle la quantité d'énergie délivrée à un composant
- Caractéristiques:
  - Fréquence: nombre de cycles par seconde (Hz)
  - Rapport cyclique (duty cycle): pourcentage de temps à l'état haut
  - Résolution: nombre de niveaux possibles du rapport cyclique

### PWM sur ESP32 - Caractéristiques

- 16 canaux PWM indépendants
- Fréquence configurable jusqu'à 40 MHz
- Résolution configurable (1-16 bits)
- Fonction principale: `ledcWrite()`
- Bibliothèque `ledc` intégrée à l'ESP32
- Peut être assigné à presque n'importe quel GPIO

### Configuration PWM sur ESP32

```cpp
// Configuration PWM
const int ledPin = 16;
const int pwmChannel = 0;
const int pwmFreq = 5000;    // 5 kHz
const int pwmResolution = 8; // 8 bits (0-255)

void setup() {
  // Configuration du canal PWM
  ledcSetup(pwmChannel, pwmFreq, pwmResolution);
  
  // Attache le canal PWM au GPIO
  ledcAttachPin(ledPin, pwmChannel);
}

void loop() {
  // Rapport cyclique 50%
  ledcWrite(pwmChannel, 127);
}
```

### Projet 4: Gradation de LED

**Objectif:** Contrôler l'intensité d'une LED avec PWM

**Matériel simulé:**
- ESP32
- LED
- Résistance
- Potentiomètre pour contrôle (optionnel)

**Concepts:**
- Configuration des canaux PWM
- Contrôle du rapport cyclique
- Effet de fondu (fading)
- Contrôle interactif (avec potentiomètre)

### Projet 5: Contrôle de servomoteur

**Objectif:** Contrôler la position d'un servomoteur

**Matériel simulé:**
- ESP32
- Servomoteur
- Potentiomètre (optionnel)

**Concepts:**
- Configuration PWM pour servos (fréquence 50Hz)
- Bibliothèque ESP32Servo
- Mappage des valeurs angulaires
- Contrôle de position précis

### Code pour servomoteur

```cpp
#include <ESP32Servo.h>

Servo myServo;
const int servoPin = 13;

void setup() {
  // Configure ESP32 PWM pour servomoteur
  ESP32PWM::allocateTimer(0);
  myServo.setPeriodHertz(50); // Standard 50Hz
  
  // Attache le servo au pin
  myServo.attach(servoPin, 500, 2400);
}

void loop() {
  // Position 0°
  myServo.write(0);
  delay(1000);
  
  // Position 90°
  myServo.write(90);
  delay(1000);
  
  // Position 180°
  myServo.write(180);
  delay(1000);
}
```

### Projet 6: Contrôle RGB LED

**Objectif:** Contrôler une LED RGB avec PWM

**Matériel simulé:**
- ESP32
- LED RGB (commune anode ou cathode)
- Résistances

**Concepts:**
- Utilisation de 3 canaux PWM indépendants
- Mélange de couleurs RGB
- Création d'effets lumineux
- Transitions douces entre couleurs

### Contrôle LED RGB

```cpp
// Configuration des canaux PWM pour LED RGB
const int redPin = 25;
const int greenPin = 26;
const int bluePin = 27;

// Canaux PWM
const int redChannel = 0;
const int greenChannel = 1;
const int blueChannel = 2;

void setup() {
  // Configure les 3 canaux PWM
  ledcSetup(redChannel, 5000, 8);
  ledcSetup(greenChannel, 5000, 8);
  ledcSetup(blueChannel, 5000, 8);
  
  // Attache les pins aux canaux
  ledcAttachPin(redPin, redChannel);
  ledcAttachPin(greenPin, greenChannel);
  ledcAttachPin(bluePin, blueChannel);
}

void setColor(int r, int g, int b) {
  ledcWrite(redChannel, r);
  ledcWrite(greenChannel, g);
  ledcWrite(blueChannel, b);
}
```

### Projet 7: Synthétiseur audio simple

**Objectif:** Générer des tonalités audio avec PWM

**Matériel simulé:**
- ESP32
- Haut-parleur ou buzzer
- Boutons ou capteurs pour interagir

**Concepts:**
- Génération de fréquences audio avec PWM
- Contrôle de la tonalité et du volume
- Création de mélodies simples
- Interaction utilisateur

## Applications Avancées

### Projet 8: Contrôle moteur DC avec PWM

**Objectif:** Contrôler la vitesse d'un moteur DC

**Matériel simulé:**
- ESP32
- Moteur DC
- Transistor ou pont H (L293D)
- Potentiomètre pour contrôle

**Concepts:**
- PWM pour contrôle de vitesse
- Interface de puissance
- Protection du microcontrôleur
- Contrôle bidirectionnel (avec pont H)

### Projet 9: Capteur analogique avec traitement avancé

**Objectif:** Analyse avancée de signaux analogiques

**Matériel simulé:**
- ESP32
- Capteurs analogiques multiples
- Afficheur OLED

**Concepts:**
- Échantillonnage multicanal
- Filtrage numérique
- Moyennes mobiles
- Détection de seuils adaptatifs
- Visualisation des données

### Projet 10: Système de contrôle automatique

**Objectif:** Créer un système de régulation PID simple

**Matériel simulé:**
- ESP32
- Capteur analogique (ex: température)
- Actuateur contrôlé par PWM
- Interface utilisateur

**Concepts:**
- Boucle de contrôle feedback
- Algorithme PID (Proportionnel-Intégral-Dérivé)
- Réglage et optimisation
- Interface utilisateur pour ajuster les paramètres

## Bonnes Pratiques

### Bonnes pratiques - ADC

- Éviter d'utiliser ADC2 avec WiFi
- Appliquer un filtre moyenneur pour réduire le bruit
- Calibrer l'ADC pour une meilleure précision
- Utiliser un diviseur de tension pour les signaux > 3.3V
- Ajouter un filtrage matériel pour les signaux bruités
- Éviter d'échantillonner à fréquence trop élevée

### Bonnes pratiques - PWM

- Choisir la bonne fréquence selon l'application:
  - LEDs: 5-10 kHz (évite scintillement)
  - Moteurs: 500-30 kHz (selon type)
  - Servos: 50Hz (standard)
  - Audio: 40-50 kHz minimum
- Adapter la résolution aux besoins (8 bits souvent suffisant)
- Ne pas surcharger les sorties de l'ESP32 (20mA max)
- Utiliser des buffers ou transistors pour charges importantes

### Débogage et optimisation

- Utiliser le moniteur série pour afficher les valeurs
- Sauvegarder les données pour analyse
- Examiner les formes d'onde avec un oscilloscope virtuel
- Économiser l'énergie en mode deep sleep entre les mesures
- Optimiser le code pour réduire la latence
- Utiliser des interruptions pour les événements critiques

## Ressources et Conclusion

### Ressources complémentaires

- Documentation ESP32: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/
- Arduino ESP32: https://github.com/espressif/arduino-esp32
- Forum Wokwi: https://wokwi.com/discord
- Tutoriels ESP32: https://randomnerdtutorials.com/esp32/
- Librairies utiles:
  - ESP32Servo
  - ESP32AnalogRead
  - RunningMedian

### Conclusion

- L'ESP32 offre des capacités analogiques et PWM puissantes
- Nombreuses applications possibles
- Wokwi facilite le prototypage et l'apprentissage
- Points clés:
  - ADC: 18 canaux, résolution configurable
  - DAC: 2 canaux (GPIO25, GPIO26)
  - PWM: 16 canaux, haute résolution, fréquence réglable
- Possibilités infinies avec ces fonctionnalités!

---
