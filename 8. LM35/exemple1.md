
## Projet : Mesure de température avec Arduino et LM35

### Description
Ce projet permet de mesurer la température ambiante à l’aide d’un capteur de température analogique (LM35) connecté à une carte Arduino Uno. Les valeurs lues sur la broche analogique sont converties en degrés Celsius, puis affichées dans le moniteur série toutes les secondes.

Le schéma Wokwi correspondant est disponible ici :  
https://wokwi.com/projects/383058011163054081

---

### Matériel requis
- 1 × Arduino Uno  
- 1 × capteur de température LM35  
- Fils de connexion  
- Câble USB pour l’alimentation et la programmation  

---

### Schéma de connexions
```

LM35     →  Arduino Uno

---

VCC     →  5 V
GND     →  GND
Vout    →  A3

````

Sur Wokwi, le fichier JSON utilisé pour générer le montage est :
```json
{
  "version": 1,
  "author": "Amer Mohia",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-arduino-uno", "id": "uno", "top": -37.8, "left": 18.6, "attrs": {} },
    { "type": "board-ds18b20", "id": "temp1", "top": 162.07, "left": -43.92, "attrs": {} }
  ],
  "connections": [
    [ "temp1:VCC", "uno:5V", "red" ],
    [ "temp1:GND", "uno:GND.2", "black" ],
    [ "uno:A3", "temp1:DQ", "green" ]
  ],
  "dependencies": {}
}
````

> **Note :** le composant `board-ds18b20` dans Wokwi est en fait utilisé ici comme symbole d’un capteur analogique sur A3 ; le code est écrit pour un LM35.

---

### Code Arduino

```cpp
// LM35 Temperature Sensor
const int sensorPin = A3;  // Entrée analogique A3

void setup() {
  Serial.begin(9600);      // Initialisation du port série
}

void loop() {
  // Lecture de la valeur analogique du LM35
  int sensorValue = analogRead(sensorPin);

  // Conversion en température (°C)
  // LM35 : 10 mV/°C, tension de référence 5 V, résolution 10 bits (0–1023)
  float voltage     = sensorValue * (5.0 / 1023.0);
  float temperature = voltage * 100.0;

  // Affichage dans le moniteur série
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" °C");

  delay(1000);            // Pause de 1 seconde
}
```

---

### Explication du code

1. **Déclaration de la broche**
   La constante `sensorPin` est initialisée à `A3`, correspondant à l’entrée analogique où est branché le LM35.

2. **Initialisation**
   Dans `setup()`, on démarre la communication série à 9600 bauds pour pouvoir afficher les résultats sur l’ordinateur.

3. **Lecture et conversion**

   * `analogRead(sensorPin)` retourne une valeur entre 0 et 1023 proportionnelle à la tension lue (0–5 V).
   * On calcule la tension réelle :

     $$
       V = \frac{\text{valeur}}{1023} \times 5\,\text{V}
     $$
   * Le LM35 délivre 10 mV par degré Celsius, donc

     $$
       T(°C) = V(\text{V}) \times 100
     $$

4. **Affichage**
   La température est envoyée sur le port série sous la forme `Temperature: XX.XX °C`.

5. **Temporisation**
   Le `delay(1000)` assure une mesure toutes les secondes.

---

### Utilisation

1. Chargez le code sur votre Arduino via l’IDE Arduino ou via Wokwi.
2. Ouvrez le **Moniteur Série** (vitesse 9600 bauds).
3. Observez la température en temps réel s’afficher chaque seconde.

---

### Extensions possibles

* **Calibration** : Comparer avec un thermomètre de référence et ajuster le coefficient si nécessaire.
* **Afficheur LCD/OLED** : Afficher la température directement sur un petit écran.
* **Envoi IoT** : Transmettre les mesures vers une plateforme en ligne (Blynk, ThingSpeak, Azure IoT, etc.).
* **Seuil d’alerte** : Allumer une LED ou émettre une alarme si la température dépasse une limite définie.

