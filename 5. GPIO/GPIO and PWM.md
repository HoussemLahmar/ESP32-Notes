# ESP32: GPIO et Entrées/Sorties
## Principes et programmation
*Houssem-eddine LAHMER*

## Plan
- Introduction aux GPIO
- Modes de fonctionnement des GPIO
- Programmation des GPIO avec Arduino IDE
- Particularités des GPIO de l'ESP32
- PWM et contrôle de moteurs
- Interruptions
- Bonnes pratiques et considérations pratiques
- Exercices pratiques
- Conclusion

## Introduction aux GPIO

### Introduction aux GPIO
- **GPIO** = General Purpose Input/Output
- Broches polyvalentes pour interagir avec le monde extérieur
- L'ESP32 dispose de jusqu'à 34 GPIO physiques
- Certaines broches ont des fonctions spéciales
- Tension de fonctionnement: 3,3V (non tolérant au 5V!)

### Caractéristiques des GPIO de l'ESP32
**Points forts:**
- 34 broches GPIO (ESP32)
- Support de pull-up/pull-down intégré
- Protection ESD intégrée
- Charge maximale: ~40mA par broche
- Vitesse de commutation élevée

**Limitations:**
- Non tolérant au 5V
- Certaines broches réservées (strapping)
- Sorties PWM limitées
- Certaines broches connectées à la flash
- GPIO 34-39: entrées uniquement

### Pinout de l'ESP32
*[Image du pinout de l'ESP32 à insérer ici]*

## Modes de fonctionnement des GPIO

### Modes de fonctionnement des GPIO
- **Mode entrée**: lecture de signaux externes
  - Avec ou sans pull-up/pull-down internes
  - Compatible avec les sorties en collecteur ouvert
- **Mode sortie**: génération de signaux
  - Push-pull (sortie standard)
  - Open-drain (drain ouvert)
- **Modes spéciaux**: fonctions alternatives
  - Communication série (UART, SPI, I2C)
  - PWM, ADC, DAC, capteurs tactiles

### Pull-up et Pull-down
- **Pull-up**: résistance entre la broche et Vcc (3,3V)
  - État par défaut: HIGH
  - Évite les états flottants
  - Courant typique: 10-50µA
- **Pull-down**: résistance entre la broche et GND
  - État par défaut: LOW
  - Moins courant que pull-up
- L'ESP32 permet de configurer pull-up/down par logiciel!


## Programmation des GPIO avec Arduino IDE

### Configuration de base des GPIO
```cpp
// Configuration d'une broche en entrée ou sortie
void setup() {
  // Configuration d'une broche en sortie
  pinMode(LED_PIN, OUTPUT);
  
  // Configuration d'une broche en entrée
  pinMode(BUTTON_PIN, INPUT);
  
  // Configuration d'une broche en entrée avec pull-up
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Configuration d'une broche en entrée avec pull-down
  pinMode(BUTTON_PIN, INPUT_PULLDOWN);
}
```

### Lecture et écriture sur les GPIO
```cpp
void loop() {
  // Lecture de l'état d'une broche
  int buttonState = digitalRead(BUTTON_PIN);
  
  // Écriture sur une broche (HIGH ou LOW)
  digitalWrite(LED_PIN, HIGH);  // 3.3V
  delay(1000);
  digitalWrite(LED_PIN, LOW);   // 0V
  delay(1000);
  
  // Lecture analogique (ADC)
  int sensorValue = analogRead(SENSOR_PIN);  // 0-4095
}
```

### Exemple: LED contrôlée par bouton
```cpp
const int buttonPin = 4;    // GPIO4 pour le bouton
const int ledPin = 5;       // GPIO5 pour la LED

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);  // Pull-up interne
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  Serial.println("ESP32 GPIO Test");
}

void loop() {
  // Lecture du bouton (LOW quand pressé avec pull-up)
  int buttonState = digitalRead(buttonPin);
  
  // Inverser la logique du bouton
  digitalWrite(ledPin, !buttonState);
  
  // Débounce
  delay(50);
}
```

## Particularités des GPIO de l'ESP32

### ADC (Analog-to-Digital Converter)
- L'ESP32 possède 2 ADC intégrés
- ADC1: 8 canaux (GPIO32-39)
- ADC2: 10 canaux (broches GPIO0, 2, 4, etc.)
  - **Attention**: ADC2 n'est pas utilisable si WiFi est actif!
- Résolution: 12 bits (0-4095)
- Tension maximale: 3,3V (sans atténuation)
- Atténuation configurable (0dB, 2.5dB, 6dB, 11dB)

### Utilisation de l'ADC
```cpp
// Exemple d'utilisation avancée de l'ADC
#include <driver/adc.h>

void setup() {
  Serial.begin(115200);
  
  // Configuration de l'ADC
  adc1_config_width(ADC_WIDTH_BIT_12);  // Résolution 12 bits
  adc1_config_channel_atten(ADC1_CHANNEL_6, ADC_ATTEN_DB_11);
  // ADC1_CHANNEL_6 correspond à GPIO34
}

void loop() {
  // Lecture avancée de l'ADC
  int val = adc1_get_raw(ADC1_CHANNEL_6);
  Serial.printf("ADC Valeur: %d\n", val);
  
  // Utilisation de la fonction standard de Arduino
  int analogValue = analogRead(34);
  
  delay(1000);
}
```

### DAC (Digital-to-Analog Converter)
- L'ESP32 dispose de 2 canaux DAC 8 bits
- DAC1: GPIO25
- DAC2: GPIO26
- Résolution: 8 bits (0-255)
- Tension de sortie: 0V à 3,3V
- Applications:
  - Génération de son
  - Contrôle de tension analogique
  - Niveaux de référence variables

### Utilisation du DAC
```cpp
// Exemple d'utilisation du DAC
void setup() {
  Serial.begin(115200);
}

void loop() {
  // Création d'une onde en dent de scie sur DAC1 (GPIO25)
  for (int i = 0; i < 256; i++) {
    dacWrite(25, i);  // Générer une tension 
                      // entre 0V et 3.3V
    delayMicroseconds(100);
  }
}
```

### Touch Sensors (Capteurs capacitifs)
- L'ESP32 intègre 10 capteurs tactiles capacitifs
- Broches: T0 (GPIO4) à T9 (GPIO32)
- Détection par variation de capacité
- Sensibilité ajustable par logiciel
- Applications:
  - Boutons tactiles
  - Détection de proximité
  - Interfaces utilisateur sans contact

### Utilisation des capteurs tactiles
```cpp
// Exemple d'utilisation d'un capteur tactile
const int touchPin = 4;  // GPIO4 = T0
const int threshold = 40;

void setup() {
  Serial.begin(115200);
}

void loop() {
  // Lecture de la valeur du capteur tactile
  int touchValue = touchRead(touchPin);
  Serial.print("Valeur du capteur: ");
  Serial.println(touchValue);
  
  // Valeur faible = touché, valeur élevée = non touché
  if (touchValue < threshold) {
    Serial.println("Capteur touché!");
  }
  
  delay(500);
}
```

## PWM et contrôle de moteurs

### PWM (Pulse Width Modulation)
- L'ESP32 possède un contrôleur LED PWM (LEDC)
- Jusqu'à 16 canaux PWM indépendants
- Résolution configurable: 1-16 bits
- Fréquence: ~1Hz à 40MHz (selon résolution)
- Applications:
  - Contrôle de luminosité
  - Contrôle de servomoteurs
  - Contrôle de moteurs DC
  - Génération de signaux


### Utilisation du PWM
```cpp
// Exemple de contrôle PWM avec LEDC
#define LED_PIN 5   // GPIO5
#define CHANNEL 0   // Canal LEDC 0
#define FREQ 5000   // Fréquence 5kHz
#define RESOLUTION 8 // Résolution 8 bits (0-255)

void setup() {
  // Configuration du canal PWM
  ledcSetup(CHANNEL, FREQ, RESOLUTION);
  // Attachement du canal à la broche GPIO
  ledcAttachPin(LED_PIN, CHANNEL);
}

void loop() {
  // Augmentation progressive de l'intensité
  for (int i = 0; i < 256; i++) {
    ledcWrite(CHANNEL, i);  // Duty cycle (0-255)
    delay(10);
  }
  
  // Diminution progressive de l'intensité
  for (int i = 255; i >= 0; i--) {
    ledcWrite(CHANNEL, i);
    delay(10);
  }
}
```

### Contrôle de servomoteurs
```cpp
// Contrôle de servomoteur avec ESP32
#include <ESP32Servo.h>

Servo myServo;
const int servoPin = 13;  // GPIO13

void setup() {
  myServo.attach(servoPin);  // Attache le servomoteur
}

void loop() {
  // Rotation de 0° à 180°
  for (int pos = 0; pos <= 180; pos++) {
    myServo.write(pos);
    delay(15);
  }
  
  // Rotation de 180° à 0°
  for (int pos = 180; pos >= 0; pos--) {
    myServo.write(pos);
    delay(15);
  }
}
```

## Interruptions

### Interruptions GPIO
- Permet de réagir immédiatement aux événements externes
- Tous les GPIO de l'ESP32 peuvent générer des interruptions
- Types d'interruptions:
  - RISING: front montant (0→1)
  - FALLING: front descendant (1→0)
  - CHANGE: changement d'état (0→1 ou 1→0)
  - LOW/HIGH: niveau bas/haut
- Permet d'économiser l'énergie (pas de polling)
- **Important**: code d'interruption doit être court et rapide!

### Exemple d'utilisation d'interruptions
```cpp
// Exemple d'interruption sur GPIO
const int buttonPin = 4;
const int ledPin = 5;
volatile bool ledState = false;

// Fonction d'interruption (ISR)
void IRAM_ATTR buttonISR() {
  ledState = !ledState;  // Inverser l'état
}

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ledPin, OUTPUT);
  
  // Attacher l'interruption (front descendant)
  attachInterrupt(digitalPinToInterrupt(buttonPin), 
                  buttonISR, FALLING);
}

void loop() {
  // Refléter l'état de la LED
  digitalWrite(ledPin, ledState);
  
  // D'autres opérations peuvent être effectuées ici
  // sans bloquer la réponse au bouton
  delay(100);
}
```

## Bonnes pratiques et considérations pratiques

### Protection des GPIO
- Les GPIO de l'ESP32 fonctionnent à 3,3V (pas tolérantes au 5V!)
- Protection contre les dommages:
  - Utiliser des diviseurs de tension pour les entrées > 3,3V
  - Utiliser des convertisseurs de niveau pour interfaces 5V
  - Ajouter des résistances en série (330Ω) pour les sorties
  - Utiliser des drivers pour contrôler des charges importantes
- Éviter les courts-circuits entre GPIO ou vers GND/VCC
- Attention aux broches de strapping (bootloader):
  - GPIO0, GPIO2, GPIO5, GPIO12, GPIO15

### Débouncing des interrupteurs
- Les contacts mécaniques peuvent "rebondir"
- Un seul appui peut générer plusieurs transitions
- Solutions:
  - Hardware: condensateur + résistance
  - Software: délai, vérification d'état multiple
  - Timer de l'ESP32
- Particulièrement important avec les interruptions

### Exemple de débounce logiciel
```cpp
// Exemple de débounce de bouton
const int buttonPin = 4;
int lastButtonState = HIGH;  // État précédent
int buttonState;             // État actuel
unsigned long lastDebounceTime = 0;
const unsigned long debounceDelay = 50;  // 50ms

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);
  Serial.begin(115200);
}

void loop() {
  // Lire l'état actuel
  int reading = digitalRead(buttonPin);
  
  // Vérifier si l'état a changé
  if (reading != lastButtonState) {
    // Réinitialiser le timer de débounce
    lastDebounceTime = millis();
  }
  
  // Si suffisamment de temps s'est écoulé
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // État stable -> mettre à jour
    if (reading != buttonState) {
      buttonState = reading;
      
      // Action sur changement d'état
      if (buttonState == LOW) {  // Bouton pressé
        Serial.println("Bouton pressé!");
      }
    }
  }
  
  lastButtonState = reading;  // Sauvegarder pour le prochain cycle
}
```

### Récapitulatif: Broches spéciales de l'ESP32
**Broches à considérer:**
- GPIO 34-39: entrées uniquement
- GPIO 6-11: connectés à la flash SPI
- ADC2: inutilisable avec WiFi actif
- Broches de strapping (0, 2, 5, 12, 15)

**Utilisations spéciales:**
- GPIO 25, 26: DAC
- GPIO 0: bouton BOOT
- GPIO 2: LED sur DevKit
- GPIO 1, 3: UART0 (console)
- GPIO 34-39: ADC (entrée seulement)

## Exercices pratiques

### Exercice 1: LED clignotante
- **Objectif**: Faire clignoter une LED à différentes fréquences
- **Matériel**:
  - ESP32 DevKit
  - LED + résistance 220Ω ou 330Ω
  - Fils de connexion
- **Tâches**:
  - Connecter la LED à GPIO5
  - Programmer un clignotement simple
  - Modifier le programme pour varier les temps d'allumage/extinction
  - Utiliser le PWM pour faire varier l'intensité

### Exercice 2: Bouton et interruption
- **Objectif**: Créer un compteur d'appuis sur un bouton
- **Matériel**:
  - ESP32 DevKit
  - Bouton poussoir
  - Résistance 10kΩ (optionnel, pull-up interne)
  - LED + résistance 220Ω
  - Fils de connexion
- **Tâches**:
  - Connecter le bouton à GPIO4
  - Connecter la LED à GPIO5
  - Programmer un compteur avec interruption
  - Afficher le compteur sur le moniteur série
  - Faire clignoter la LED N fois après N appuis

### Exercice 3: Capteur analogique
- **Objectif**: Lire une valeur analogique et contrôler une sortie
- **Matériel**:
  - ESP32 DevKit
  - Potentiomètre ou capteur analogique (LDR, etc.)
  - LED + résistance 220Ω
  - Fils de connexion
- **Tâches**:
  - Connecter le potentiomètre/capteur à GPIO34 (ADC)
  - Connecter la LED à GPIO5 (PWM)
  - Lire la valeur analogique et l'afficher
  - Utiliser la valeur pour contrôler l'intensité de la LED
  - Ajouter un seuil pour déclencher une action

## Conclusion

### Points clés à retenir
- L'ESP32 offre de nombreuses options d'E/S:
  - GPIO numériques (entrée/sortie)
  - ADC (conversion analogique-numérique)
  - DAC (conversion numérique-analogique)
  - PWM (modulation de largeur d'impulsion)
  - Capteurs tactiles capacitifs
- Considérations importantes:
  - Niveau de tension: 3,3V uniquement
  - Limitations de certaines broches
  - Protection des entrées/sorties
  - Débounce des entrées
- La bonne gestion des GPIO est la base de tout projet IoT

### Ressources supplémentaires
- Documentation officielle:
  - [https://docs.espressif.com/projects/esp-idf/en/latest/esp32/](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
- Librairies utiles:
  - ESP32Servo
  - OneButton (gestion avancée de boutons)
  - FastLED (gestion de LEDs RGB)
- Sites de référence:
  - Random Nerd Tutorials
  - Last Minute Engineers
  - Espressif Learning Resources

