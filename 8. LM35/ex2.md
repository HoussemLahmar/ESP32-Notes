Réflexion durant quelques secondes


## Projet : Mesure de température avec Arduino, LM35 et simulation Wokwi

### Description  
Ce projet lit la température ambiante à l’aide d’un capteur analogique LM35 connecté à une carte Arduino Uno. Le code Arduino convertit la tension lue en degrés Celsius et en degrés Fahrenheit, puis affiche les deux valeurs sur le moniteur série chaque seconde. Parallèlement, une simulation Wokwi inclut un modèle de puce LM35 (`lm35chip.c`) qui génère dynamiquement la tension de sortie en fonction d’un contrôle de température virtuel.

---

### Matériel requis  
- 1 × Arduino Uno  
- 1 × capteur de température LM35 (chip-LM35 dans Wokwi)  
- Fils de connexion  
- Câble USB pour alimentation et programmation  

---

### Schéma de connexions  
```

LM35     →  Arduino Uno

---

VCC     →  5 V
GND     →  GND
OUT     →  A0

````

---

### Fichier JSON Wokwi  
```json
{
  "version": 1,
  "author": "Maverick",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-arduino-uno", "id": "uno", "top": 0, "left": 0, "attrs": {} },
    { "type": "chip-LM35",         "id": "chip1", "top": 234.36, "left": 229.47, "attrs": {} }
  ],
  "connections": [
    [ "chip1:VCC", "uno:5V",       "red",   [ "h0" ] ],
    [ "chip1:GND", "uno:GND.3",    "black", [ "h0" ] ],
    [ "chip1:OUT", "uno:A0",       "blue",  [ "h0" ] ]
  ],
  "dependencies": {}
}
````

> **Note :** ce JSON définit la carte Arduino et le composant LM35 simulé.

---

### Code Arduino (`.ino`)

```cpp
/*
 * Exemple ArduinoGetStarted.com
 * Tutoriel : https://arduinogetstarted.com/tutorials/arduino-lm35-temperature-sensor
 */

#define ADC_VREF_mV    5000.0  // Référence ADC en millivolts
#define ADC_RESOLUTION 1024.0  // Résolution ADC 10 bits
#define PIN_LM35       A0      // Broche analogique A0

void setup() {
  Serial.begin(9600);         // Démarrage de la liaison série
}

void loop() {
  int adcVal = analogRead(PIN_LM35);                       // Lecture ADC
  float milliVolt = adcVal * (ADC_VREF_mV / ADC_RESOLUTION);  
  float tempC     = milliVolt / 10;                        // 10 mV/°C
  float tempF     = tempC * 9.0 / 5.0 + 32.0;               // Conversion en °F

  Serial.print("Temperature: ");
  Serial.print(tempC);
  Serial.print(" °C  ~  ");
  Serial.print(tempF);
  Serial.println(" °F");

  delay(1000);                                             // 1 s d’attente
}
```

---

### Code de simulation Wokwi (`lm35chip.c`)

```c
#include "wokwi-api.h"
#include <stdlib.h>

// Données de la puce LM35 simulée
typedef struct {
  pin_t VCC, OUT, GND;
  uint32_t temperature;
} chip_data_t;

// Vérifie que VCC est à l’état haut et GND à l’état bas
bool power_connected(void *data) {
  chip_data_t *chip = data;
  return pin_read(chip->VCC) && !pin_read(chip->GND);
}

// Callback périodique : génère la tension de sortie selon la température
void chip_timer_callback(void *user_data) {
  if (power_connected(user_data)) {
    chip_data_t *chip = user_data;
    float volts = 0.01 * attr_read(chip->temperature);  
    pin_dac_write(chip->OUT, volts);
  }
}

// Initialisation de la puce simulée
void chip_init(void) {
  chip_data_t *chip = malloc(sizeof(chip_data_t));
  chip->VCC         = pin_init("VCC",   INPUT);
  chip->GND         = pin_init("GND",   INPUT);
  chip->OUT         = pin_init("OUT",   ANALOG);
  chip->temperature = attr_init("temperature", 50);

  timer_config_t config = {
    .callback  = chip_timer_callback,
    .user_data = chip,
  };
  timer_t timer_id = timer_init(&config);
  timer_start(timer_id, 10000, true); // Callback toutes les 10 ms
}
```

Et le contrôle utilisateur dans Wokwi (`LM35`):

```json
{
  "name": "LM35",
  "author": "Maverick",
  "pins": ["VCC","OUT","GND","",""],
  "controls": [
    {
      "id": "temperature",
      "label": "Temperature",
      "type": "range",
      "min": 2,
      "max": 150,
      "step": 1
    }
  ]
}
```

> **Fonctionnement de la simulation :**
> Le slider “Temperature” permet de faire varier la température virtuelle (2–150 °C). À chaque cycle, le callback génère une tension de sortie de 10 mV/°C sur la broche analogique, que l’Arduino lit et convertit.

---

### Explications détaillées

1. **Conversion ADC → millivolt**

   $$
     V_{\text{mV}} = \frac{\text{adcVal}}{1023} \times 5000
   $$

2. **Tension → température (°C)**

   $$
     T_{°C} = \frac{V_{\text{mV}}}{10}
   $$

3. **Conversion en Fahrenheit**

   $$
     T_{°F} = T_{°C} \times \frac{9}{5} + 32
   $$

4. **Simulation Wokwi**

   * Le code C crée un composant virtuel dont la sortie analogique suit la température réglée par l’utilisateur.
   * La fonction `timer_callback` assure la mise à jour continue de la tension.

---


