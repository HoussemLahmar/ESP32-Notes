## Projet de Détection de Luminosité avec ESP32

Ce projet utilise un ESP32 pour mesurer la luminosité ambiante à l'aide d'une photorésistance (LDR) et contrôler une LED en fonction du niveau de lumière détecté. La simulation est réalisée sur la plateforme Wokwi.

---

## 1. Vue d'ensemble

- **Microcontrôleur** : ESP32 DevKit V1
- **Capteurs et sorties** :
  - Photorésistance (LDR) connectée à la broche analogique A0 (D26)
  - LED rouge connectée à la broche numérique D23
- **Fonctionnalité** :
  1. Lire la valeur analogique de la LDR.
  2. Convertir cette valeur en tension, puis en résistance.
  3. Calculer l’éclairement en lux à partir de la résistance.
  4. Allumer la LED si le niveau de lumière dépasse un seuil (50 lux), sinon l’éteindre.
  5. Afficher dans le moniteur série l’éclairement et l’état (Light!/Dark).

---

## 2. Matériel et connexions

| Composant        | Broche ESP32   | Couleur câble |
|------------------|----------------|---------------|
| GND (LDR)        | GND            | Noir          |
| VCC (LDR)        | 3V3            | Rouge         |
| Sortie analogique (LDR A0) | D26 (A0)    | Vert          |
| Anode LED        | D23            | Vert          |
| Cathode LED      | GND            | Noir          |
| UART Serial      | TX0 / RX0      | —             |

Simulation Wokwi : [Visualiser le projet](https://wokwi.com/projects/377699965857221633)

---

## 3. Description du code

```cpp
#define LED_PIN 23
#define LDR_PIN 26

const float GAMMA = 0.7;
const float RL_10LUX = 33;
const float TEN_GAMMA = pow(10, GAMMA);

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  int analogValue = analogRead(LDR_PIN);
  float voltage = analogValue * 3.3 / 4096.0;
  float resistance = 2000 * voltage / (3.3 - voltage);
  float lux = pow((RL_10LUX * 1000 * TEN_GAMMA) / resistance, 1/GAMMA);

  bool isLight = lux > 50;
  digitalWrite(LED_PIN, isLight ? HIGH : LOW);

  Serial.printf("Room: %s  Lux: %.1f\n", isLight ? "Light!" : "Dark  ", lux);
  delay(1000);
}
```

- **analogRead()** : lit la valeur brute (0–4095) de la LDR.
- **Conversion tension** : `voltage = analogValue * 3.3 / 4096.0` (référence 3.3V, ADC 12 bits).
- **Calcul résistance** : `R = (R_pullup * V) / (Vcc - V)` avec R_pullup = 2 kΩ.
- **Calcul lux** : voir section suivante.

---

## 4. Calcul du Lux, Gamma et Formule

### 4.1. Éclairement (lux)

Le lux (symbole lx) est l’unité d’éclairement lumineux, i.e., le flux lumineux par unité de surface perçue par l’œil humain. Plus la valeur en lux est élevée, plus la scène est lumineuse.

### 4.2. Exposant Gamma

En photométrie, la relation entre la résistance d’une LDR et l’éclairement lumineux suit une loi de puissance :

> \[ R_{LDR} = R_{10lux} \times \left( \frac{E}{10} \right)^{-\gamma} \]

- **R_{LDR}** : résistance de la photorésistance (ohms)
- **E** : éclairement (lux)
- **R_{10lux}** : résistance mesurée à 10 lux (ohms)
- **γ** (gamma) : coefficient spécifique du capteur, typiquement entre 0,6 et 0,8. Ici, \(\gamma = 0.7\).

### 4.3. Formule de calcul utilisée

À partir de la loi ci-dessus, on isole E :

\[
E = 10 \times \left( \frac{R_{10lux}}{R_{LDR}} \right)^{1/\gamma}
\]

Dans le code :

```cpp
const float GAMMA = 0.7;
const float RL_10LUX = 33; // en kilo-ohms (33 kΩ)
const float TEN_GAMMA = pow(10, GAMMA);

// Calcul :
// R_LDR (ohms) = 2000 * V / (3.3 - V)
// lux = pow((RL_10LUX * 1000 * TEN_GAMMA) / R_LDR, 1.0/GAMMA);
```

- On convertit R_{10lux} en ohms (33 kΩ → 33 000 Ω).
- On calcule \(10^{\gamma}\) pour optimiser la formule.
- Enfin, on élève le ratio à la puissance de 1/γ pour obtenir l’éclairement E en lux.

---

## 5. Seuil et Affichage

- **Seuil** : 50 lux. Si E > 50, on considère la pièce suffisamment éclairée et on allume la LED.
- **Moniteur Série** : affiche l’état (`Light!` ou `Dark`) et la valeur mesurée en lux.

---

## 6. Améliorations possibles

- Calibration du capteur pour votre environnement (mesurer R_{10lux} réel).
- Ajouter filtrage (moyenne sur plusieurs mesures) pour réduire le bruit.
- Enregistrer les données de luminosité dans un service cloud ou sur carte SD.
- Intégrer un écran OLED pour un affichage local en temps réel.

---

© 2025 Houssem Lahmer

