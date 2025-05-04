
# Communication UART

**Auteur : Houssem-eddine LAHMER**

---

## Plan du cours

* Introduction à l'UART
* UART sur ESP32
* Simulation avec Wokwi
* Atelier pratique
* Conclusion

---

## Introduction à l'UART

### Qu'est-ce que l'UART ?

* UART (Universal Asynchronous Receiver Transmitter)
* Protocole de communication série asynchrone
* Pas de signal d'horloge externe
* Utilisé pour échanger des données entre microcontrôleurs, capteurs, PC, etc.

### Illustrations

![Hardware UART](uart_hardware.png)
![Timing UART](uart_timing.png)

### Avantages et Inconvénients

| Avantages                                   | Inconvénients                         |
| ------------------------------------------- | ------------------------------------- |
| Simplicité d'implémentation                 | Vitesse limitée comparée à SPI        |
| Peu de broches nécessaires (RX, TX)         | Nécessite réglage précis du baud rate |
| Réassignation de broches possible sur ESP32 | Pas de détection d'erreurs avancée    |

---

## UART sur ESP32

### Présentation de l'ESP32

* Microcontrôleur 32 bits, dual-core, Wi-Fi & Bluetooth intégrés
* Plusieurs modules UART (UART0, UART1, UART2)
* Horloge jusqu'à 240 MHz

### Brochage par défaut des UARTs

| Port UART | TX | RX | RTS | CTS |
| --------- | -- | -- | --- | --- |
| UART0     | 1  | 3  | 22  | 19  |
| UART1     | 10 | 9  | 11  | 6   |
| UART2     | 17 | 16 | 7   | 8   |

> *Réassignation possible via le multiplexeur GPIO*

### Initialisation en Arduino

```cpp
#include <HardwareSerial.h>
HardwareSerial MySerial(1); // UART1
const int RX_PIN = 16;
const int TX_PIN = 17;

void setup() {
  MySerial.begin(115200, SERIAL_8N1, RX_PIN, TX_PIN);
}

void loop() {
  if (MySerial.available()) {
    int c = MySerial.read();
    MySerial.write(c);
  }
}
```

---

## Simulation avec Wokwi

### Présentation de Wokwi

* Simulateur en ligne pour Arduino & ESP32
* Permet de tester le code sans matériel physique
* Visualisation des signaux série

### Exemple de projet Wokwi

```cpp
// worsketch code pour ESP32 sur Wokwi
#include <HardwareSerial.h>
HardwareSerial MySerial(1);

void setup() {
  MySerial.begin(9600, SERIAL_8N1, 16, 17);
  Serial.begin(9600);
}

void loop() {
  if (MySerial.available()) {
    char c = MySerial.read();
    Serial.print("Reçu: ");
    Serial.println(c);
    MySerial.print("Echo: ");
    MySerial.println(c);
  }
}
```

* Ouvrir le moniteur série Wokwi
* Sélectionner ESP32 et broches 16/17

### Projets Wokwi associés

* [Projet 1](https://wokwi.com/projects/389315991086330881)
* [Projet 2](https://wokwi.com/projects/384715188313657345)

---

## Atelier pratique

### Exercice 1 : Communication PC-ESP32

* Connecter ESP32 à l'ordinateur via USB
* Envoyer des commandes via le moniteur série
* Faire écho des messages reçus

### Exercice 2 : Dialogue entre deux ESP32

* Simuler deux ESP32 sur Wokwi
* Utiliser UART1 pour échanger des données
* Afficher les données sur les moniteurs respectifs

---

## Conclusion

### Résumé

* Concepts de base de l'UART
* Configuration des UART sur ESP32
* Simulation et debugging avec Wokwi
* Applications pratiques

---

