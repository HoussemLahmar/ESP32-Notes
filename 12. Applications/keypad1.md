

# 🎹 Clavier MIDI avec ESP32 et Keypad 4x4

Ce projet simule un contrôleur MIDI basé sur une carte **ESP32** avec un **clavier matriciel 4x4** et des **LEDs** pour visualiser l'état des touches. Il utilise la communication série pour envoyer des messages MIDI vers un ordinateur ou un logiciel compatible.

## 🔧 Matériel utilisé

* ESP32 Devkit V1
* Clavier matriciel 4x4
* 6 LEDs (rouges et une jaune)
* 6 résistances de 220Ω
* Breadboard et fils de connexion

## 🔌 Schéma de câblage (Wokwi)

Lien Wokwi du projet :
👉 [Simulation en ligne](https://wokwi.com/projects/372328090236616705)

**Connexion du Keypad :**

| Ligne | Pin ESP32 |
| ----- | --------- |
| R1    | GPIO 13   |
| R2    | GPIO 12   |
| R3    | GPIO 14   |
| R4    | GPIO 27   |
| C1    | GPIO 26   |
| C2    | GPIO 25   |
| C3    | GPIO 33   |
| C4    | GPIO 32   |

**Connexion des LEDs :**

| LED                | Pin ESP32 |
| ------------------ | --------- |
| L1                 | GPIO 15   |
| L2                 | GPIO 2    |
| L3                 | GPIO 4    |
| L4                 | GPIO 5    |
| L5                 | GPIO 18   |
| L6 (activité MIDI) | GPIO 19   |

---

## 🧠 Fonctionnement

### 1. Lecture du clavier

Le code lit en permanence les touches appuyées sur le clavier 4x4 à l’aide de la bibliothèque `Keypad.h`.

### 2. Allumage des LEDs

Lorsque certaines touches sont pressées, les LEDs s'allument :

* Touches `1` à `5` : allument respectivement LED1 à LED5.
* Touche `0` : éteint toutes les LEDs.
* Touches `A`, `B`, `C`, `D`, `*`, `#` : allument toutes les LEDs.

### 3. Envoi de messages MIDI

Quand une touche numérique est pressée (`1` à `9`, `0`, `S`, `P`) :

* Envoie un **message MIDI "Note ON"** via le port série (valeur `127`).
* Quand la touche est relâchée, envoie un **message MIDI "Note OFF"** (valeur `0`).
* Ces messages sont visibles dans le moniteur série.

Les notes MIDI envoyées sont basées sur la note `midC = 60` et peuvent être transposées avec la variable `transpose`.

### 4. Indicateur d’activité MIDI

LED6 (la jaune) est allumée brièvement lors de l'envoi de messages MIDI, indiquant une activité MIDI.

---

## 🎵 Exemple de message MIDI

* Touche `1` pressée :

  ```
  144
  60
  127
  ```

* Touche `1` relâchée :

  ```
  128
  60
  0
  ```

---

## 📁 Structure du code

* `ledStatus(char key)` : contrôle l’allumage des LEDs selon la touche.
* `MIDImessage(status, data1, data2)` : envoie un message MIDI par série.
* `loop()` : détection des touches, gestion des états (pressé / relâché), envoi MIDI.

---


