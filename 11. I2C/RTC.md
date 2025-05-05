# Projet ESP32 avec Module RTC DS1307

## Introduction

Ce projet démontre comment intégrer un module RTC (Real-Time Clock) DS1307 avec un ESP32 pour maintenir l'heure et la date précises, même lorsque l'alimentation principale est coupée. Le DS1307 est un circuit intégré d'horloge en temps réel qui utilise une pile de secours pour continuer à fonctionner en cas de coupure d'alimentation.

https://wokwi.com/projects/404478711154455553

## Le Module DS1307

Le DS1307 est un RTC (Real-Time Clock) populaire qui offre les caractéristiques suivantes :

- Horloge/calendrier en temps réel avec secondes, minutes, heures, jour, date, mois et année
- Compensation automatique des mois de moins de 31 jours et des années bissextiles
- Format d'heure 12 ou 24 heures
- 56 octets de RAM non volatile pour stockage de données
- Interface I2C pour la communication avec le microcontrôleur
- Consommation d'énergie très faible (moins de 500nA en mode batterie)
- Plage de température de fonctionnement: -40°C à +85°C

## Matériel Nécessaire

- ESP32 DevKit C
- Module RTC DS1307
- Pile CR2032 (pour alimenter le DS1307 en cas de coupure d'alimentation)
- Câbles de connexion

## Connexions

Le DS1307 communique via le protocole I2C. Voici les connexions nécessaires :

| DS1307 | ESP32 |
|--------|-------|
| VCC    | 5V    |
| GND    | GND   |
| SDA    | GPIO 21 (SDA) |
| SCL    | GPIO 22 (SCL) |

## Bibliothèques Requises

Pour ce projet, vous aurez besoin des bibliothèques suivantes :
- `Wire.h` : Pour la communication I2C
- `RTClib.h` : Pour interagir avec le DS1307

Ces bibliothèques peuvent être installées via le Gestionnaire de bibliothèques Arduino.

## Code

```cpp
#include <Wire.h>
#include <RTClib.h>

RTC_DS1307 rtc;

void setup() {
  Serial.begin(115200);
  
  // Initialiser la communication I2C
  Wire.begin();
  
  // Vérifier si le RTC est correctement connecté
  if (!rtc.begin()) {
    Serial.println("Impossible de trouver le RTC");
    while (1);
  }
  
  // Vérifier si le RTC a perdu de l'alimentation et si c'est le cas, définir l'heure
  if (!rtc.isrunning()) {
    Serial.println("Le RTC ne fonctionne PAS !");
    // Cette ligne définit le RTC avec une date et une heure explicites
    // Par exemple, pour définir le 1er janvier 2024 à 00:00, vous appelleriez :
    rtc.adjust(DateTime(2024, 1, 1, 0, 0, 0));
  }
}

void loop() {
  DateTime now = rtc.now();
  
  // Afficher la date et l'heure actuelles sur le moniteur série
  Serial.print(now.year(), DEC);
  Serial.print('/');
  Serial.print(now.month(), DEC);
  Serial.print('/');
  Serial.print(now.day(), DEC);
  Serial.print(" ");
  Serial.print(now.hour(), DEC);
  Serial.print(':');
  Serial.print(now.minute(), DEC);
  Serial.print(':');
  Serial.print(now.second(), DEC);
  Serial.println();
  
  delay(1000);  // Attendre 1 seconde
}
```

## Fonctionnement du Code

1. **Initialisation** :
   - Configuration de la communication série à 115200 bauds
   - Initialisation de la communication I2C
   - Vérification que le RTC est correctement connecté
   - Si le RTC a perdu son alimentation (pile morte ou première utilisation), il règle l'heure à une valeur par défaut

2. **Boucle principale** :
   - Lecture de l'heure et de la date actuelles du RTC
   - Affichage des informations sur le moniteur série
   - Attente d'une seconde avant de répéter

## Avantages de l'Utilisation d'un RTC

- **Précision** : Le DS1307 offre une meilleure précision temporelle que les timers internes de l'ESP32
- **Persistance** : Grâce à la pile de secours, l'heure est maintenue même lorsque l'ESP32 n'est pas alimenté
- **Décharge du microcontrôleur** : Le RTC s'occupe du suivi du temps, libérant l'ESP32 pour d'autres tâches

## Applications Possibles

- Systèmes de journalisation de données avec horodatage précis
- Systèmes domotiques avec programmation temporelle
- Projets d'automatisation basés sur l'heure
- Réveils ou horloges intelligentes
- Systèmes de contrôle d'irrigation avec programmation horaire

## Dépannage

- **"Impossible de trouver le RTC"** : Vérifiez les connexions I2C et assurez-vous que l'adresse du RTC est correcte
- **"Le RTC ne fonctionne PAS !"** : La pile est peut-être morte ou n'est pas installée, ou c'est la première utilisation du RTC
- **Heures incorrectes** : Le RTC peut avoir besoin d'être réinitialisé avec l'heure correcte

## Extensions Possibles

- Ajout d'un écran LCD pour afficher l'heure
- Utilisation du RTC pour déclencher des événements à certaines heures
- Synchronisation de l'heure via NTP (Network Time Protocol) lors de la connexion à Internet
- Ajout d'un bouton pour ajuster manuellement l'heure