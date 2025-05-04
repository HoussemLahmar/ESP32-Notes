
## Projet : Communication SPI et chiffrement ROT13 avec Arduino et Wokwi

### Description  
Ce montage illustre l’utilisation du bus SPI pour échanger des données entre une carte Arduino Uno et un composant personnalisé Wokwi qui implémente un chiffrement ROT13. Le micro‑contrôleur envoie une chaîne de caractères, le composant la transforme caractère par caractère et renvoie le résultat que l’Arduino affiche ensuite sur le port série.
https://wokwi.com/projects/330669951756010068
---

### Matériel et simulation Wokwi  
- **Carte** : Arduino Uno  
- **Composant SPI** : `chip-spi-example` (custom Wokwi)  
- **Connexions** :  
  - MOSI → broche 11  
  - MISO → broche 12  
  - SCK  → broche 13  
  - CS   → broche 10  
  - VCC  → 5 V  
  - GND  → GND  

Le schéma complet est disponible dans Wokwi via le JSON suivant :

```json
{
  "version": 1,
  "author": "Uri Shaked",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-arduino-uno", "id": "uno",  "top": 10.2,  "left": 37.8,  "attrs": {} },
    { "type": "chip-spi-example",  "id": "chip1","top": -75.78,"left":196.8,  "attrs": {} }
  ],
  "connections": [
    ["chip1:SCK",  "uno:13",       "gold",   []],
    ["chip1:MOSI", "uno:11",       "violet", []],
    ["uno:12",     "chip1:MISO",   "blue",   []],
    ["chip1:CS",   "uno:10",       "orange", []],
    ["chip1:VCC",  "uno:5V",       "red",    []],
    ["chip1:GND",  "uno:GND.2",    "black",  []]
  ]
}
````

---

### Code Arduino (`.ino`)

```cpp
#include <SPI.h>
#define CS 10

void setup() {
  // Chaîne chiffrée en ROT13 : "Bonjour, SPI!"  
  char buffer[] = "Uryyb, FCV! ";

  Serial.begin(115200);
  pinMode(CS, OUTPUT);

  // Démarrage de la transaction SPI
  digitalWrite(CS, LOW);
  SPI.begin();
  SPI.transfer(buffer, strlen(buffer));
  SPI.end();
  digitalWrite(CS, HIGH);

  // Affichage du résultat retourné par le composant SPI
  Serial.println("Données reçues du périphérique SPI :");
  Serial.println(buffer);
}

void loop() {
  // Boucle vide : tout se passe en setup()
}
```

---

### Composant custom Wokwi (`chip-spi-example`)

#### Fichier C d’implémentation (`.c`)

```c
#include "wokwi-api.h"
#include <stdlib.h>
#include <stdio.h>

typedef struct {
  pin_t    cs_pin;
  uint32_t spi;
  uint8_t  spi_buffer[1];
} chip_state_t;

static void on_pin_change(void *ud, pin_t pin, uint32_t value);
static void on_spi_done(void *ud, uint8_t *buf, uint32_t count);

void chip_init(void) {
  chip_state_t *chip = malloc(sizeof(*chip));
  chip->cs_pin = pin_init("CS", INPUT_PULLUP);
  pin_watch(chip->cs_pin, &(pin_watch_config_t){
    .edge = BOTH, .pin_change = on_pin_change, .user_data = chip
  });
  chip->spi = spi_init(&(spi_config_t){
    .sck  = pin_init("SCK",  INPUT),
    .miso = pin_init("MISO", INPUT),
    .mosi = pin_init("MOSI", INPUT),
    .done = on_spi_done, .user_data = chip
  });
  printf("SPI Chip initialized!\n");
}

uint8_t rot13(uint8_t c) {
  if (c >= 'A' && c <= 'Z')
    return (c - 'A' + 13) % 26 + 'A';
  if (c >= 'a' && c <= 'z')
    return (c - 'a' + 13) % 26 + 'a';
  return c;
}

void on_pin_change(void *ud, pin_t pin, uint32_t value) {
  chip_state_t *chip = ud;
  if (pin == chip->cs_pin) {
    if (value == LOW) {
      // Sélection du composant : démarrage de la réception SPI
      chip->spi_buffer[0] = ' ';
      spi_start(chip->spi, chip->spi_buffer, 1);
    } else {
      // Désélection : arrêt du SPI
      spi_stop(chip->spi);
    }
  }
}

void on_spi_done(void *ud, uint8_t *buf, uint32_t count) {
  if (count == 0) return;
  buf[0] = rot13(buf[0]);
  chip_state_t *chip = ud;
  // Si toujours sélectionné, poursuivre l’échange
  if (pin_read(chip->cs_pin) == LOW)
    spi_start(chip->spi, chip->spi_buffer, 1);
}
```

#### Métadonnées Wokwi (`.json`)

```json
{
  "name": "SPI Example",
  "author": "Uri Shaked",
  "pins": ["SCK","MISO","MOSI","CS","GND","VCC"]
}
```

---

### Fonctionnement pas à pas

1. **Initialisation SPI**

   * L’Arduino configure CS (broche 10) en sortie, lance la liaison SPI à 8 MHz par défaut.
2. **Envoi de la chaîne**

   * La chaîne chiffrée en ROT13 (`"Uryyb, FCV! "`) est transmise octet par octet.
3. **Traitement côté puce**

   * À chaque réception, la puce applique la fonction `rot13()` et renvoie le caractère transformé.
4. **Affichage des données**

   * Une fois la transaction terminée, l’Arduino lit le buffer et affiche la chaîne déchiffrée (“Bonjour, SPI!”) sur le port série.

---

### Extensions possibles

* **DMA SPI** pour décharger le CPU des transferts.
* **Modes d’horloge** différents (mode 0, 1, 2 ou 3) pour tester la robustesse.
* **Chiffrements plus complexes** (AES, XOR, etc.) en modifiant la fonction de transformation.
* **Interface utilisateur** sur écran OLED pour saisir/en afficher les messages en temps réel.

```
```
