# Projet Arduino avec DAC et écran LCD

## Description du projet

Ce projet utilise un microcontrôleur ESP32 pour générer différentes tensions analogiques à l'aide d'un convertisseur numérique-analogique (DAC) intégré. Les valeurs de tension sont affichées sur un écran LCD à interface I2C. Le projet démontre comment convertir des valeurs numériques en tensions analogiques et les afficher en temps réel.

## Composants utilisés

- Microcontrôleur ESP32 (disposant d'un DAC intégré)
- Écran LCD 16×2 avec module I2C (adresse 0x27)
- Câbles de connexion

## Branchements

- LCD I2C:
  - SDA → PIN 21 de l'ESP32
  - SCL → PIN 22 de l'ESP32
  - VCC → 5V
  - GND → GND
- DAC (intégré à l'ESP32):
  - Canal 1 → PIN 25
  - Canal 2 → PIN 26 (non utilisé dans ce projet)

## Fonctionnement du code

Le programme réalise les opérations suivantes:

1. **Initialisation**: Configuration de la communication série, de l'écran LCD et du DAC
2. **Boucle principale**: Génère séquentiellement 9 valeurs de tension prédéfinies
3. **Affichage**: Pour chaque valeur, affiche à la fois la valeur numérique (0-255) et la tension correspondante (0-3.3V) sur l'écran LCD et via le port série

### Explication détaillée du code

```cpp
#include <Wire.h>                // Bibliothèque pour la communication I2C
#include <LiquidCrystal_I2C.h>   // Bibliothèque pour contrôler l'écran LCD via I2C
#define DAC_CH1 25               // Définition du pin pour le canal 1 du DAC
#define DAC_CH2 26               // Définition du pin pour le canal 2 du DAC (non utilisé)

LiquidCrystal_I2C lcd(0x27,16,2);  // Initialisation de l'écran LCD (adresse 0x27, 16 colonnes, 2 lignes)
float val = 0;                     // Variable pour stocker la valeur de tension calculée
int cislo[9] = {0,24,64,78,128,156,192,233,255};  // Tableau des valeurs numériques à envoyer au DAC
```

- Le tableau `cislo` contient 9 valeurs numériques entre 0 et 255 qui seront envoyées au DAC.
- Ces valeurs correspondent à différentes tensions entre 0V et 3.3V.

```cpp
void setup() {
  Serial.begin(115200);          // Initialisation de la communication série
  lcd.init();                    // Initialisation de l'écran LCD
  lcd.backlight();               // Activation du rétroéclairage de l'écran
}
```

La fonction `setup()` initialise:
- La communication série à 115200 bauds pour le débogage
- L'écran LCD et active son rétroéclairage

```cpp
void loop() {  
  for(int i=0; i<9; i++) {
    dacWrite(DAC_CH1, cislo[i]);     // Envoi de la valeur au DAC
    lcd.clear();                     // Effacement de l'écran LCD
    Serial.println("DAC Value " + String(cislo[i]));  // Affichage de la valeur numérique sur le port série
    lcd.setCursor(0,0);              // Positionnement du curseur sur la première ligne
    lcd.print("DAC Value " + String(cislo[i]));  // Affichage de la valeur numérique sur l'écran
    lcd.setCursor(0,1);              // Positionnement du curseur sur la deuxième ligne
    val = 3.3/256*cislo[i];          // Calcul de la tension correspondante (règle de trois)
    lcd.print("U = " + String(val) + " V ");  // Affichage de la tension sur l'écran
    delay(5000);                     // Attente de 5 secondes avant de passer à la valeur suivante
  }
}
```

La fonction `loop()`:
1. Parcourt le tableau des valeurs numériques
2. Pour chaque valeur:
   - L'envoie au DAC pour générer la tension correspondante
   - Affiche la valeur numérique sur le port série et sur la première ligne de l'écran
   - Calcule la tension correspondante (`val = 3.3/256*cislo[i]`)
   - Affiche la tension sur la deuxième ligne de l'écran
   - Attend 5 secondes avant de passer à la valeur suivante

## Calcul des tensions

Le DAC de l'ESP32 a une résolution de 8 bits, ce qui signifie qu'il peut générer 256 valeurs différentes (de 0 à 255).
La tension maximale est de 3.3V (référence interne de l'ESP32).

Pour calculer la tension correspondant à une valeur numérique, on utilise la formule:
```
Tension (V) = 3.3V * (valeur numérique / 256)
```

Exemples de valeurs utilisées dans le projet:
- 0 → 0V
- 24 → 0.31V
- 64 → 0.83V
- 128 → 1.65V
- 255 → 3.3V

## Simulation

Le projet peut être simulé en ligne sur Wokwi à l'adresse: https://wokwi.com/projects/389379674365238273

