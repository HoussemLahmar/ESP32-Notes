# Sauvegarde de données sur carte microSD avec ESP32
*Par Houssem-eddine LAHMER*

## Table des matières
- [Introduction](#introduction)
- [Présentation matérielle](#présentation-matérielle)
- [Connexions et matériel](#connexions-et-matériel)
- [Bibliothèques et configuration](#bibliothèques-et-configuration)
- [Exemples de code](#exemples-de-code)
- [Conseils et bonnes pratiques](#conseils-et-bonnes-pratiques)
- [Conclusion](#conclusion)

## Introduction
### Pourquoi enregistrer sur microSD ?
- Stockage local de grandes quantités de données (capteurs, logging)
- Indépendance réseau / cloud
- Facilité de récupération et de transfert des données

## Présentation matérielle
### La carte ESP32
- Microcontrôleur dual-core 32 bits
- Interfaces SPI, SDMMC, I2C, UART...
- Alimentation 3.3 V
- Bibliothèque Arduino Core disponible

### La carte microSD
- Formats : microSD, microSDHC, microSDXC
- Système de fichiers : FAT16 / FAT32
- Tension 3.3 V (attention au niveau logique)
- Carte adaptateur break-out pour prototypage

## Connexions et matériel
### Schéma de câblage

![Connexion SPI ESP32 <--> microSD](1_HjtbjHda4oy-VH6plX81GA.jpg)

Broches SPI par défaut :
- MOSI : GPIO 23
- MISO : GPIO 19
- SCK  : GPIO 18
- CS   : GPIO 5 (ou autre broche libre)

## Bibliothèques et configuration
### Bibliothèque Arduino SD

```cpp
#include <SPI.h>
#include <SD.h>
```

- `SD.begin(chipSelect)` : initialisation de la carte
- `SD.exists(path)`, `SD.open(path, mode)`

### Paramétrage de l'ESP32

```cpp
const int chipSelect = 5;

void setup() {
  Serial.begin(115200);
  if (!SD.begin(chipSelect)) {
    Serial.println("Échec initialisation SD");
    return;
  }
  Serial.println("Carte SD prête.");
}
```

## Exemples de code
### Écriture de données

```cpp
void loop() {
  File dataFile = SD.open("datalog.txt", FILE_WRITE);
  if (dataFile) {
    dataFile.println("Nouvelle ligne de données");
    dataFile.close();
    Serial.println("Donnée écrite.");
  } else {
    Serial.println("Erreur ouverture fichier");
  }
  delay(1000);
}
```

### Lecture de données

```cpp
void lireFichier() {
  File dataFile = SD.open("datalog.txt");
  if (dataFile) {
    while (dataFile.available()) {
      Serial.write(dataFile.read());
    }
    dataFile.close();
  } else {
    Serial.println("Impossible d'ouvrir le fichier");
  }
}
```

## Conseils et bonnes pratiques
### Optimisations et fiabilité
- Éviter d'ouvrir/fermer trop fréquemment : tamponnez les écritures
- Vérifier l'espace disponible avant écriture
- Gérer les erreurs et contrôles CRC
- Utiliser un buffer circulaire en RAM pour logging intensif

## Conclusion
- La carte microSD est une solution simple pour du stockage local
- L'ESP32 offre plusieurs interfaces pour la piloter
- Les bibliothèques Arduino simplifient grandement le code
- Adapter le code selon l'application et la fréquence d'écriture

