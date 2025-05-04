**Convert LaTeX to Markdown**

L'utilisateur a partagé un code beamer en LaTeX et souhaite le convertir en format Markdown. Cela semble être une simple transformation technique. Aucun besoin de rechercher ou de consulter des informations en ligne, c'est un changement direct de format. Je dois simplement convertir le code LaTeX en markdown. Je vais donc envelopper les diapositives dans des titres markdown, en m'assurant que le texte est correctement représenté sans le besoin de navigation en ligne.


Réflexion durant 6 secondes


# Servomoteurs et bibliothèque Servo pour ESP32

**Auteur : Houssem‑eddine LAHMER**

---

## Plan du cours

1. [Introduction aux servomoteurs](#introduction-aux-servomoteurs)
2. [Types de servomoteurs](#types-de-servomoteurs)
3. [Principe de fonctionnement](#principe-de-fonctionnement)
4. [ESP32 et contrôle de servomoteurs](#esp32-et-contrôle-de-servomoteurs)
5. [Bibliothèque ESP32Servo](#bibliothèque-esp32servo)
6. [Simulation avec Wokwi](#simulation-avec-wokwi)
7. [Applications pratiques](#applications-pratiques)
8. [Conclusion](#conclusion)
9. [Ressources complémentaires](#ressources-complémentaires)

---

## Introduction aux servomoteurs

### Qu'est‑ce qu'un servomoteur ?

* Un **servomoteur** est un moteur capable de maintenir une position angulaire précise.
* **Composants principaux :**

  * Moteur DC
  * Système d'engrenages réducteurs
  * Circuit de contrôle avec retour de position
  * Potentiomètre de retour de position
* **Utilisations courantes :**

  * Robotique
  * Modélisme (avions, voitures RC)
  * Automatismes
  * Projets IoT

---

## Types de servomoteurs

### Servo standard (180°)

* Rotation limitée entre 0° et 180°
* Précision angulaire élevée
* Exemples : SG90, MG996R

### Servo à rotation continue

* Rotation complète à 360°
* Contrôle de vitesse (et non de position)
* Utile pour les roues, convoyeurs

**Caractéristiques techniques importantes :**

* Couple (torque) en kg·cm ou oz·in
* Vitesse de rotation
* Tension d'alimentation (typiquement 4.8 V–6 V)
* Poids et dimensions

---

## Principe de fonctionnement

* Contrôle par **signaux PWM** (Pulse Width Modulation)
* Largeur d'impulsion entre 1 ms et 2 ms sur une période de 20 ms

  * 1 ms → position 0°
  * 1,5 ms → position 90°
  * 2 ms → position 180°
* Circuit interne :

  1. Compare la position demandée (signal PWM)
  2. Avec la position réelle (potentiomètre)
  3. Ajuste le moteur jusqu'à concordance

---

## ESP32 et contrôle de servomoteurs

### L'ESP32 et ses capacités PWM

* Dispose de 16 canaux PWM indépendants
* Résolution configurable jusqu'à 16 bits
* Fréquence réglable jusqu'à 40 MHz
* Idéal pour contrôler plusieurs servomoteurs simultanément
* Avantages par rapport à d'autres microcontrôleurs :

  * Plus de canaux PWM
  * Meilleure résolution et précision
  * Processeur double cœur à 240 MHz
  * Wi‑Fi et Bluetooth intégrés

### Connexion d'un servomoteur à l'ESP32

![Connexion d'un servomoteur à l'ESP32](esp32-servo-motor.jpg)

#### Branchement

* **Fil rouge** → 5 V (ou 3,3 V selon modèle)
* **Fil marron/noir** → GND
* **Fil orange/jaune** → GPIO de l'ESP32

#### Considérations d'alimentation

* Alimentation séparée recommandée si plusieurs servos
* Condensateur 100–470 µF en parallèle pour stabiliser
* Ne pas alimenter des servos puissants directement depuis l’ESP32

---

## Bibliothèque ESP32Servo

### Présentation

* Extension de la bibliothèque Servo standard d’Arduino, adaptée pour l’ESP32
* **Fonctionnalités :**

  * Contrôle jusqu’à 16 servomoteurs simultanés
  * Compatible avec tous les GPIOs de l’ESP32
  * Gestion automatique des timers matériels
  * Support des servos standard et à rotation continue
* Installation via le Gestionnaire de bibliothèques Arduino :

  * Rechercher **ESP32Servo** par Kevin Harrington
  * Ou depuis GitHub : [https://github.com/madhephaestus/ESP32Servo](https://github.com/madhephaestus/ESP32Servo)

### Fonctions principales

```cpp
// Création d'un objet servo
Servo monServo;

// Attacher le servo à une broche GPIO
monServo.attach(pin);            // Version simple
monServo.attach(pin, min, max);  // Avec impulsions min/max

// Contrôle de position (0–180°)
monServo.write(angle);

// Contrôle direct par durée d'impulsion (1000–2000 μs)
monServo.writeMicroseconds(us);

// Lire la position actuelle (0–180)
int position = monServo.read();

// Détacher le servo (libère le canal PWM)
monServo.detach();
```

---

### Exemples d’utilisation

#### Création et attachement d’un servo

```cpp
Servo monServo;
monServo.attach(pin);         // Version simple
monServo.attach(pin, min, max); // Avec configuration min/max
```

#### Contrôle de position par angle

```cpp
monServo.write(angle);  // Angle en degrés (0–180)
```

#### Contrôle direct par durée d’impulsion

```cpp
monServo.writeMicroseconds(us);  // Largeur en µs (typiquement 1000–2000)
```

#### Lecture de la position actuelle

```cpp
int position = monServo.read();  // Position last‑write (0–180)
```

#### Détacher le servo

```cpp
monServo.detach();  // Stoppe les impulsions PWM
```

---

## Simulation avec Wokwi

### Introduction à Wokwi

* Simulateur en ligne pour Arduino, ESP32, etc.
* **Avantages :**

  * Pas de matériel physique nécessaire
  * Visualisation immédiate
  * Pas de risque d’endommager le matériel
  * Partage facile des projets
  * Intégration avec l’IDE Arduino
* Accès : [https://wokwi.com/](https://wokwi.com/)

### Configuration d’un projet servo sur Wokwi

1. Créer un nouveau projet ESP32
2. Ajouter le composant **Servo Motor**
3. Connexions :

   * Signal → GPIO (ex. GPIO13)
   * * → 5 V
   * – → GND
4. Écrire le code (ESP32Servo)
5. Démarrer la simulation

#### Exemple de `diagram.json`

```json
{
  "version": 1,
  "author": "Votre Nom",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-esp32-devkit-v1", "id": "esp" },
    { "type": "wokwi-servo", "id": "servo1" }
  ],
  "connections": [
    ["esp:GND.1",   "servo1:GND", "black"],
    ["esp:5V",      "servo1:V+",  "red"],
    ["esp:D13",     "servo1:SIG","orange"]
  ],
  "dependencies": {}
}
```

---

## Applications pratiques

* **Projets robotiques :** bras robotique, hexapode, pan‑tilt caméra
* **Automatisation domestique :** stores, serrures connectées, distributeurs d’animaux
* **Projets IoT :** station météo, contrôle à distance Wi‑Fi/Bluetooth, suivi solaire

---

## Conclusion

* Les servomoteurs offrent un contrôle précis de la position angulaire.
* L’ESP32, avec ses 16 canaux PWM et sa connectivité, est idéal pour piloter des servos.
* La bibliothèque ESP32Servo simplifie grandement le développement.
* Wokwi permet de prototyper sans matériel.

---

## Ressources complémentaires

* **ESP32Servo** (GitHub) : [https://github.com/madhephaestus/ESP32Servo](https://github.com/madhephaestus/ESP32Servo)
* **ESP‑IDF** (docs) : [https://docs.espressif.com/projects/esp-idf](https://docs.espressif.com/projects/esp-idf)
* **Wokwi Docs** : [https://docs.wokwi.com/](https://docs.wokwi.com/)
* Tutoriels :

  * Random Nerd Tutorials (ESP32 & servos)
  * Electropeak (Guide servomoteurs)
  * DroneBot Workshop (Servos avancés)
* Livres :

  * *ESP32 Programming for the Internet of Things* by Agus Kurniawan
  * *Robotics with the ESP32* by Manoj R. Thakur

---

