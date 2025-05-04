# Servo Sweep ESP32

Ce projet illustre un exemple simple de balayage (sweep) d'un servomoteur à l'aide d'une carte ESP32 et de la librairie `ESP32Servo`. Le code fait osciller l'angle du servomoteur entre 0° et 180° de manière répétitive.

---

## 📄 Description

* **Objectif** : Faire pivoter un servomoteur de 0° à 180° puis revenir à 0° en boucle.
* **Environnement** : Simulation sur [Wokwi](https://wokwi.com/arduino/projects/323706614646309460) ou déploiement sur un ESP32 réel.

---

## 🧰 Matériel requis

* Carte **ESP32 DevKit C**
* Servomoteur (5 V) compatible PWM
* Câbles de connexion (Dupont)
* (Optionnel) Alimentation externe 5 V pour le servomoteur

---

## 🔌 Schéma de câblage

| Servomoteur        | ESP32 DevKit C |
| ------------------ | -------------- |
| V+ (rouge)         | 5 V            |
| GND (noir)         | GND            |
| Signal PWM (jaune) | GPIO 18        |

> **Remarque** : En simulation Wokwi, la connexion est préconfigurée. Sur matériel réel, veillez à partager la masse (GND) entre l'ESP32 et le servomoteur.

---

## ⚙️ Installation & Simulation Wokwi

1. Ouvrez le projet sur Wokwi :
   [https://wokwi.com/arduino/projects/323706614646309460](https://wokwi.com/arduino/projects/323706614646309460)
2. Cliquez sur **Start Simulation**.
3. Le servomoteur commencera son mouvement de balayage automatiquement.

---

## 📥 Déploiement sur ESP32 réel

1. Installez l'IDE Arduino ou PlatformIO.
2. Ajoutez la carte ESP32 via le gestionnaire de cartes Arduino :

   * URL supplémentaire : `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
3. Installez la librairie **ESP32Servo** :

   * Menu **Croquis > Inclure une bibliothèque > Gérer les bibliothèques…**, recherchez `ESP32Servo` et installez.
4. Connectez l'ESP32 à votre ordinateur via USB.
5. Copiez-collez le code dans un nouveau sketch et téléversez.
6. Observez le mouvement du servomoteur.

---

## 📝 Explication du code

```cpp
#include <ESP32Servo.h>

const int servoPin = 18;
Servo servo;

void setup() {
  // Attache le servomoteur au pin 18 avec impulsions de 500 µs à 2400 µs
  servo.attach(servoPin, 500, 2400);
}

void loop() {
  // Balayage de 0° à 180°
  for (int pos = 0; pos <= 180; pos++) {
    servo.write(pos);
    delay(15);  // Pause pour permettre au servomoteur d'atteindre la position
  }
  // Retour de 180° à 0°
  for (int pos = 180; pos >= 0; pos--) {
    servo.write(pos);
    delay(15);
  }
}
```

* **`servo.attach(pin, minPulse, maxPulse)`** : Configure le pin PWM et les impulsions min/max pour le servomoteur.
* **`servo.write(angle)`** : Définit l'angle du servomoteur (0°–180°).
* **`delay(ms)`** : Attend le temps nécessaire pour que le servomoteur atteigne la position.

---

## 🛡️ Licence

Ce projet est distribué sous la licence MIT. Consultez le fichier [LICENSE](LICENSE) pour plus de détails.


