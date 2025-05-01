```markdown
# Projet Wokwi – Contrôle de servo-moteur avec ESP32

## Description  
Ce projet montre comment piloter un servo-moteur à l’aide d’une carte ESP32 sous MicroPython (ou Arduino C++) dans l’environnement de simulation Wokwi. Le servo effectue un balayage (sweep) de 0° à 180°, puis de 180° à 0°, avec un temps de pause pour chaque position et un retour d’information via le port série.  

## Matériel requis  
- **Carte :** ESP32 DevKit V1 (Wokwi)  
- **Actionneur :** Servo-moteur (GPIO D2)  
- **Composants annexes :**  
  - Photo-résistance (LDR) connectée à D33  
  - Interrupteur à glissière (slide switch) vers GND  
  - LED rouge sur D13 via résistance 1 kΩ  
- **Simulation :** Projet Wokwi (https://wokwi.com/projects/429778316977213441)  

## Schéma de connexion  
```
ESP32 3V3 ───── V+ de la servo  
ESP32 GND ───── GND de la servo  
ESP32 D2  ───── PWM de la servo  

ESP32 3V3 ───── Photo-résistance VCC  
Photo-résistance AO ── D33 sur ESP32  
Photo-résistance GND ── ESP32 GND  

ESP32 D13 ── Résistance 1 kΩ ── LED (anode)  
LED (cathode) ── ESP32 GND  

Slide switch bornes 1 & 2 ── ESP32 GND  
```

![Schéma de câblage](https://i.imgur.com/xyz1234.png)  
*(Illustration du montage dans Wokwi)*  

## Aperçu du code  
Le sketch – disponible en MicroPython et en équivalent Arduino C++ – se comporte de la façon suivante :

1. **Initialisation**  
   - Attache le servo sur la broche dédiée.  
   - Démarre la communication série à 115 200 bps pour le débogage.  

2. **Fonction `moveServo(angle)`**  
   - Contraint l’angle entre 0° et 180°.  
   - Envoie la commande au servo et attend 1 s pour atteindre la position.  
   - Affiche l’angle courant dans le moniteur série.  

3. **Boucle principale**  
   - Balayage progressif de 0° à 180° par pas de 30°.  
   - Balayage inverse de 180° à 0° par pas de 30°.  
   - Pause de 2 s avant de recommencer.  

```cpp
#include <Servo.h>

const int SERVO_PIN = 2;
Servo myServo;

void setup() {
  myServo.attach(SERVO_PIN);
  Serial.begin(115200);
  while (!Serial);
  Serial.println("Servo controller initialized.");
}

void moveServo(int angle) {
  angle = constrain(angle, 0, 180);
  myServo.write(angle);
  delay(1000);
  Serial.print("Servo Angle: ");
  Serial.println(angle);
}

void loop() {
  for (int pos = 0; pos <= 180; pos += 30) {
    moveServo(pos);
  }
  for (int pos = 180; pos >= 0; pos -= 30) {
    moveServo(pos);
  }
  delay(2000);
}
```

## Utilisation  
1. Lancez la simulation dans Wokwi.  
2. Ouvrez le moniteur série (115 200 bps).  
3. L’interrupteur à glissière peut être utilisé pour couper la masse du circuit (simulateur uniquement).  
4. Observez le balayage du servo et les relevés d’angle dans le moniteur série.  

## Extensions possibles  
- Contrôler la position du servo en fonction de la luminosité mesurée par la LDR.  
- Faire clignoter la LED lorsque l’angle atteint un seuil donné.  
- Ajouter un bouton-poussoir pour passer du sweep automatique à un pilotage manuel.  
- Migrer le code vers MicroPython sur ESP32 pour expérimenter un environnement Python embarqué.  

## Auteurs  
- **Houssem Lahmer** – Conception du circuit, code et simulation Wokwi  
- **Référence projet** – Wokwi Project ID: 429778316977213441  
```