# Projet ESP32 avec carte microSD

https://wokwi.com/projects/323656763409695316

## Présentation du projet

Ce projet consiste à interfacer une carte ESP32 avec un lecteur de carte microSD. L'objectif est de pouvoir lire et écrire des données sur la carte SD, ce qui est utile pour diverses applications comme l'enregistrement de données (datalogger), le stockage de configurations, ou la lecture de fichiers.

## Composants utilisés

- Une carte ESP32 DevKit V1
- Un module lecteur de carte microSD
- Une carte microSD (préchargée avec un fichier de test)
- Fils de connexion

## Schéma de connexion

Le module microSD est connecté à l'ESP32 via l'interface SPI (Serial Peripheral Interface) :

| Pin microSD | Pin ESP32 | Couleur du fil | Description     |
|-------------|-----------|----------------|-----------------|
| SCK         | D18       | Vert           | Signal d'horloge |
| DO (MISO)   | D19       | Vert           | Sortie données  |
| DI (MOSI)   | D23       | Vert           | Entrée données  |
| CS          | D5        | Vert           | Chip Select     |
| VCC         | 3V3       | Rouge          | Alimentation    |
| GND         | GND       | Noir           | Masse           |

## Code du projet

Le code effectue les opérations suivantes :

1. Initialisation de la communication série à 115200 bauds
2. Initialisation de la carte SD en utilisant le pin 5 pour CS (Chip Select)
3. Affichage de la liste des fichiers présents sur la carte SD
4. Lecture et affichage du contenu du fichier "wokwi.txt"

```cpp
#include <SD.h>
#define CS_PIN 5

File root;

void setup() {
  Serial.begin(115200);
  
  Serial.print("Initializing SD card... ");
  
  if (!SD.begin(CS_PIN)) {
    Serial.println("Card initialization failed!");
    while (true);
  }
  
  Serial.println("initialization done.");
  
  Serial.println("Files in the card:");
  root = SD.open("/");
  printDirectory(root, 0);
  Serial.println("");
  
  // Example of reading file from the card:
  File textFile = SD.open("/wokwi.txt");
  if (textFile) {
    Serial.print("wokwi.txt: ");
    while (textFile.available()) {
      Serial.write(textFile.read());
    }
    textFile.close();
  } else {
    Serial.println("error opening wokwi.txt!");
  }
}

void loop() {
  delay(100);
  // nothing happens after setup finishes.
}

void printDirectory(File dir, int numTabs) {
  while (true) {
    File entry = dir.openNextFile();
    if (! entry) {
      // no more files
      break;
    }
    for (uint8_t i = 0; i < numTabs; i++) {
      Serial.print('\t');
    }
    Serial.print(entry.name());
    if (entry.isDirectory()) {
      Serial.println("/");
      printDirectory(entry, numTabs + 1);
    } else {
      // files have sizes, directories do not
      Serial.print("\t\t");
      Serial.println(entry.size(), DEC);
    }
    entry.close();
  }
}
```

## Configuration Wokwi

Le projet utilise la plateforme de simulation Wokwi. La configuration du projet est définie dans un fichier JSON qui spécifie :

- Les composants utilisés (ESP32 DevKit V1 et lecteur de carte microSD)
- Leur position et orientation sur l'interface
- Les connexions entre les composants

## Contenu de la carte SD

La carte SD contient un fichier nommé "wokwi.txt" avec le message suivant :
```
Hello, SD Card!
```

## Fonctionnalités du code

### 1. Initialisation de la carte SD
Le code commence par initialiser la bibliothèque SD en spécifiant le pin CS (Chip Select) utilisé pour la communication.

### 2. Parcours des fichiers
La fonction `printDirectory()` parcourt récursivement tous les dossiers et fichiers présents sur la carte SD et affiche leur nom et taille.

### 3. Lecture de fichier
Le programme lit le contenu du fichier "wokwi.txt" et l'affiche sur le moniteur série.

## Applications possibles

Ce projet peut servir de base pour de nombreuses applications :

- Enregistrement de données de capteurs (datalogger)
- Stockage de configurations pour des projets IoT
- Manipulation de fichiers (lire/écrire/créer/supprimer)
- Systèmes embarqués nécessitant du stockage persistant

## Extensions possibles

Le projet pourrait être étendu pour :

1. Écrire des données sur la carte SD
2. Créer une structure de fichiers
3. Implémenter un système de journalisation
4. Ajouter d'autres capteurs et enregistrer leurs données
5. Créer un serveur web qui affiche les fichiers stockés

## Conclusion

Ce projet démontre comment interfacer une carte ESP32 avec un lecteur de carte microSD via SPI, permettant ainsi de stocker et lire des données sur un support externe. Cette fonctionnalité est essentielle pour de nombreux projets IoT et d'électronique embarquée nécessitant du stockage persistant ou la manipulation de fichiers.