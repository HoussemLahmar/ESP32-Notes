**Conversion en markdown**

L'utilisateur souhaite convertir un fichier LaTeX Beamer (.tex) en markdown, comme pour des tâches précédentes. Ce fichier concerne le cours SPI ESP32 Wokwi. Il m'indique simplement qu’il veut le contenu "en markdown". Il n'y a pas de besoin de recherche externe ici, donc je vais effectuer la conversion du format LaTeX en markdown, sans chercher de contenu supplémentaire.


Réflexion durant 5 secondes


## Communication SPI sur ESP32

**Auteur : Houssem‑eddine LAHMER**

---

## Plan du cours

* [Introduction au SPI](#introduction-au-spi)
* [SPI sur ESP32](#spi-sur-esp32)
* [Transfert de données](#transfert-de-données)
* [Simulation avec Wokwi](#simulation-avec-wokwi)
* [Atelier pratique](#atelier-pratique)
* [Conclusion](#conclusion)

---

## Introduction au SPI

### Qu'est‑ce que le SPI ?

* **SPI** (Serial Peripheral Interface)
* Protocole série synchrone maître‑esclave
* Utilise quatre lignes : MOSI, MISO, SCK, CS
* Très haute vitesse, large débit de données

### Illustration

![SPI with three slaves](SPI_three_slaves.svg.png)

### Brochage SPI – Signaux et connexions

* **MOSI** – ligne de données du maître vers l’esclave
* **MISO** – ligne de données de l’esclave vers le maître
* **SCLK** – horloge générée par le maître
* **SS / CS** – sélection de l’esclave (actif bas)

> **Remarques**
>
> * Plusieurs esclaves : chaque esclave nécessite une ligne SS/CS dédiée.
> * Adaptez les niveaux logiques (3,3 V vs 5 V) si nécessaire.
> * Quatre modes SPI (CPOL, CPHA) définissent l’échantillonnage des données.

---

## Avantages et inconvénients du SPI

| **Avantages**                   | **Inconvénients**                     |
| ------------------------------- | ------------------------------------- |
| Débit élevé (MHz)               | 4 lignes minimum (+ 1 CS par esclave) |
| Communication full‑duplex       | Pas d’adressage natif                 |
| Mise en œuvre logicielle simple | Pas de détection d’erreur intégrée    |

---

## SPI sur ESP32

### Contrôleurs SPI sur ESP32

* **ESP32, ESP32‑S2, ESP32‑S3** : 4 contrôleurs (SPI0, SPI1, HSPI, VSPI)
* **ESP32‑C3** : 3 contrôleurs (SPI0, HSPI, VSPI)
* SPI0 et SPI1 réservés à la flash interne
* HSPI et VSPI disponibles pour l’utilisateur

### Brochages par défaut

| **Bus** | **MOSI** | **MISO** | **SCK** | **CS** |
| :-----: | :------: | :------: | :-----: | :----: |
|   HSPI  |    13    |    12    |    14   |   15   |
|   VSPI  |    23    |    19    |    18   |    5   |

> *Réassignation possible via* `SPIClass.begin`

### Initialisation basique en Arduino

```cpp
#include <SPI.h>

void setup() {
  SPI.begin();         // utilise HSPI par défaut
  // SPI.begin(SCK, MISO, MOSI, SS);
}

void loop() {
  // ...
}
```

### Définition manuelle d'un bus SPI

```cpp
#include <SPI.h>

#define HSPI_CLK  14
#define HSPI_MISO 12
#define HSPI_MOSI 13
#define HSPI_SS   15

SPIClass hspi = SPIClass(HSPI);

void setup() {
  hspi.begin(HSPI_CLK, HSPI_MISO, HSPI_MOSI, HSPI_SS);
}

void loop() {
  // ...
}
```

---

## Transfert de données

### Envoyer et recevoir un octet

```cpp
byte dataToSend = 0xAA;
byte received;

// Sélection du périphérique
digitalWrite(HSPI_SS, LOW);

// Envoi et réception simultanés
received = hspi.transfer(dataToSend);

// Fin de communication
digitalWrite(HSPI_SS, HIGH);
```

---

## Simulation avec Wokwi

### Présentation de Wokwi pour SPI

* Simulateur en ligne pour ESP32 & périphériques SPI
* Visualisation des signaux MOSI/MISO/SCK/CS
* Test sans matériel physique

### Exemple Wokwi : lecture d'une mémoire SPI

```cpp
#include <SPI.h>
#define CS_PIN 15

void setup() {
  Serial.begin(115200);
  SPI.begin();
  pinMode(CS_PIN, OUTPUT);
}

void loop() {
  digitalWrite(CS_PIN, LOW);
  byte id = SPI.transfer(0x9F); // commande Read ID
  digitalWrite(CS_PIN, HIGH);
  Serial.print("Device ID: 0x");
  Serial.println(id, HEX);
  delay(1000);
}
```

---

## Atelier pratique

### Exercice 1 : SD Card SPI

* Connecter un module SD (MOSI = 13, MISO = 12, SCK = 14, CS = 15)
* Lire la liste des fichiers via la bibliothèque SD
* Afficher sur le moniteur série

### Exercice 2 : Écran TFT SPI

* Ajouter un écran TFT SPI
* Initialiser l’affichage et dessiner du texte
* Gérer plusieurs périphériques CS distincts

---

## Conclusion

* Protocole SPI et ses caractéristiques
* Configuration des bus HSPI/VSPI sur ESP32
* Transferts d’octets et simulations Wokwi
* Applications pratiques (SD, TFT)

---

## Questions ?

Merci pour votre attention !
