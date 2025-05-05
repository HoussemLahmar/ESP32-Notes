# Projet d'affichage LCD avec ESP32


https://wokwi.com/projects/375092701719122945


## Description
Ce projet utilise un module ESP32 pour contrôler un afficheur LCD 20x4 via une interface I2C. Le programme affiche un message de bienvenue sur les quatre lignes de l'écran LCD.

## Matériel nécessaire
- 1 x ESP32 DevKit V1
- 1 x Écran LCD 20x4 avec module I2C (adresse 0x27)
- Câbles de connexion

## Connexions
| LCD I2C | ESP32 |
|---------|-------|
| GND     | GND   |
| VCC     | 3V3   |
| SDA     | D21   |
| SCL     | D22   |

## Bibliothèques requises
- `LiquidCrystal_I2C.h` - Pour contrôler l'écran LCD via I2C

## Code
Le code initialise l'écran LCD via I2C et affiche un message de bienvenue sur les 4 lignes:
```cpp
#include <LiquidCrystal_I2C.h>

#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  // Init
  lcd.init();
  lcd.backlight();

  // Print something
  lcd.setCursor(3, 0);
  lcd.print("Welcome to");
  lcd.setCursor(2, 1);
  lcd.print("lcd display");
  lcd.setCursor(5, 2);
  lcd.print("Simulator");
  lcd.setCursor(7, 3);
  lcd.print("Enjoy!");
}

void loop() {
}
```

## Explication du code
1. **Configuration de l'écran LCD**:
   - Définition de l'adresse I2C (`0x27`)
   - Configuration des dimensions de l'écran (20 colonnes x 4 lignes)
   - Création d'une instance `LiquidCrystal_I2C`

2. **Fonction `setup()`**:
   - Initialisation de l'écran LCD
   - Activation du rétroéclairage
   - Positionnement du curseur et affichage de texte sur chaque ligne

3. **Fonction `loop()`**:
   - Vide car aucune action répétée n'est nécessaire

## Configuration du simulateur Wokwi
Le projet est configuré pour fonctionner dans l'environnement de simulation Wokwi avec les paramètres suivants:
- Version: 1
- Auteur: 4JN20EC077 - Supriya M
- Éditeur: wokwi
- Composants: ESP32 DevKit V1 et LCD 1602 (en mode I2C)

## Utilisation
1. Téléversez le code sur votre ESP32
2. Connectez l'écran LCD selon le schéma de câblage
3. Mettez l'ESP32 sous tension
4. L'écran affichera le message de bienvenue

## Notes
- Assurez-vous que l'adresse I2C de votre écran LCD est bien `0x27`. Si ce n'est pas le cas, modifiez la valeur `I2C_ADDR` dans le code.
- Si vous utilisez un écran de dimensions différentes, ajustez les valeurs `LCD_COLUMNS` et `LCD_LINES`.