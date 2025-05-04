# Scanner I²C avec Arduino UNO et Périphériques I²C

Ce projet illustre l'utilisation d'un Arduino UNO pour détecter et scanner plusieurs périphériques connectés sur le bus I²C. Il s'appuie sur le code de Rui Santos (Random Nerd Tutorials) pour l'exploration des adresses I²C, et met en œuvre une simulation Wokwi incluant :

* **MPU6050** (acéléromètre/gyroscope)
* **SSD1306** (afficheur OLED 128×64)
* **DS1307** (module RTC)
* **Résistance 1 kΩ** pour le pull-down/AD0 du MPU6050

https://wokwi.com/projects/362457199533004801

---

## 📂 Contenu du projet

* `I2C_scanner.ino` : code Arduino pour scanner le bus I²C et lister les adresses des appareils détectés.
* Fichier de simulation Wokwi qui décrit le câblage et la configuration de la carte.

---

## 🧰 Matériel requis

1. **Arduino UNO**
2. **Capteur MPU6050** (VCC, GND, SDA→A4, SCL→A5, AD0→résistance 1 kΩ vers GND)
3. **Afficheur OLED SSD1306** (VCC, GND, SDA, SCL)
4. **Module RTC DS1307** (5 V, GND, SDA, SCL)
5. **Résistance 1 kΩ** (entre AD0 du MPU6050 et la masse)
6. Fils de connexion

---

## 🔌 Schéma de connexion

```
Arduino UNO        MPU6050         SSD1306 OLED      DS1307 RTC
┌───────┐          ┌───────┐        ┌───────────┐      ┌───────┐
│     A4├── SDA ──▶ SDA            SDA◀──│ SDA │      │ SDA  │
│     A5├── SCL ──▶ SCL            SCL◀──│ SCL │      │ SCL  │
│     5V├── VCC ──▶ VCC            VCC──▶│ VCC │──▶ 5V │ 5V   │
│   GND├── GND ──▶ GND            GND──▶│ GND │──▶ GND│ GND  │
└───────┘          │ AD0─┬───┐                    └───────┘
                    │    │   │
                    └─┐  └───┘ Résistance 1 kΩ vers GND
                      AD0 vers pull-down
```

> **Note** : Le bus I²C est partagé entre tous les périphériques (SDA/SCL en commun). AD0 du MPU6050 permet de changer son adresse I²C (0x68 ou 0x69) selon sa connexion.

---

## 💻 Installation et utilisation

1. **Importer le code**

   * Ouvrez `I2C_scanner.ino` dans l’IDE Arduino.
2. **Ajouter la bibliothèque Wire**

   * Dans l’IDE Arduino, allez dans **Croquis > Inclure une bibliothèque > Wire** (incluse par défaut).
3. **Téléverser**

   * Branchez votre Arduino UNO et téléversez le sketch.
4. **Ouvrir le moniteur série**

   * Réglez la vitesse à **115200 bauds**.
5. **Observer la sortie**

   * Le programme affiche toutes les 5 secondes la liste des adresses détectées sur le bus I²C.

```
I2C Scanner
Scanning...
I2C device found at address 0x3C  // SSD1306
I2C device found at address 0x68  // MPU6050 ou DS1307
Done

// et ainsi de suite toutes les 5 secondes
```

---

## 📖 Explications du code

```cpp
#include <Wire.h>

void setup() {
  Wire.begin();
  Serial.begin(115200);
  Serial.println("\nI2C Scanner");
}

void loop() {
  byte error, address;
  int nDevices = 0;

  Serial.println("Scanning...");
  for(address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();

    if (error == 0) {
      Serial.print("I2C device found at address 0x");
      if (address < 16) Serial.print('0');
      Serial.println(address, HEX);
      nDevices++;
    }
    else if (error == 4) {
      Serial.print("Unknown error at address 0x");
      if (address < 16) Serial.print('0');
      Serial.println(address, HEX);
    }
  }

  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("Done\n");

  delay(5000);
}
```

* **Wire.begin()** : initialise le bus I²C.
* **Wire.beginTransmission(addr)** & **Wire.endTransmission()** : tente de communiquer avec `addr` et renvoie un code d’erreur (0 = succès).
* **Serial.println()** : affiche les résultats dans le moniteur série.
* **delay(5000)** : attend 5 s avant un nouveau scan.

---

## 🔗 Références

* Rui Santos, *ESP32 I2C communication*, Random Nerd Tutorials : [https://randomnerdtutorials.com/esp32-i2c-communication-arduino-ide/#2](https://randomnerdtutorials.com/esp32-i2c-communication-arduino-ide/#2)
* Documentation Wokwi : [https://wokwi.com](https://wokwi.com)

---

