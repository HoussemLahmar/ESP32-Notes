# Projet ESP32 avec MPU6050 et SSD1306

Ce projet utilise un ESP32 pour lire les données d'un accéléromètre/gyroscope MPU6050 et les afficher sur un écran OLED SSD1306.

## Matériel requis

- ESP32 DevKit V1
- MPU6050 (accéléromètre et gyroscope 6 axes)
- Écran OLED SSD1306 128x64 pixels
- Câbles de connexion

## Bibliothèques requises

- Adafruit_MPU6050
- Adafruit_Sensor
- Wire
- Adafruit_SSD1306
- Adafruit_GFX

## Connexions

### MPU6050 vers ESP32
- VCC → 3.3V
- GND → GND
- SCL → D22 (GPIO22, SCL)
- SDA → D21 (GPIO21, SDA)

### Écran OLED SSD1306 vers ESP32
- VIN → 3.3V
- GND → GND
- CLK → D22 (GPIO22, SCL)
- DATA → D21 (GPIO21, SDA)

## Fonctionnalités

1. **Initialisation**
   - Configuration du MPU6050 et de l'écran OLED
   - Affichage d'un écran de démarrage avec le texte "ESP32" et "MPU6050"

2. **Lecture des capteurs**
   - Acquisition des données de l'accéléromètre (axe X, Y, Z)
   - Acquisition des données du gyroscope (axe X, Y, Z)

3. **Affichage**
   - Affichage des valeurs de l'accéléromètre sur la première moitié de l'écran
   - Affichage des valeurs du gyroscope sur la seconde moitié de l'écran
   - Rafraîchissement des données toutes les 10 ms

## Configuration des capteurs

Le MPU6050 est configuré avec les paramètres suivants :
- Plage de l'accéléromètre : ±8g
- Plage du gyroscope : ±250 degrés/seconde
- Bande passante du filtre : 21 Hz

## Comment utiliser

1. Assemblez le circuit selon les connexions indiquées ci-dessus
2. Installez les bibliothèques requises dans l'IDE Arduino
3. Téléversez le code sur votre ESP32
4. L'écran OLED affichera les valeurs d'accélération et de rotation en temps réel

## Simulation

Ce projet peut être simulé en ligne sur Wokwi en utilisant le lien suivant :
[Simulation Wokwi](https://wokwi.com/projects/379135709875186689)

## Applications possibles

- Détection d'orientation
- Détection de mouvement
- Base pour un système de stabilisation
- Acquisition de données pour l'analyse de mouvement
- Projets IoT nécessitant des capteurs de mouvement

## Dépannage

Si l'écran ou le capteur ne s'initialise pas :
1. Vérifiez les connexions entre les composants
2. Assurez-vous que les adresses I2C sont correctes (0x3C pour l'écran OLED)
3. Vérifiez que les bibliothèques sont correctement installées