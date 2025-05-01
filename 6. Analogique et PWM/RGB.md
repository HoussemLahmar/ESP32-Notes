
# Contrôle d'une LED RGB avec ESP32

Ce projet Wokwi simule le contrôle d'une LED RGB à l'aide d'une carte **ESP32**. Le programme permet d'afficher trois couleurs différentes en changeant les intensités des canaux **Rouge**, **Vert** et **Bleu** toutes les secondes.  

## 🔧 Matériel utilisé

- ESP32 (ou Arduino Uno dans la simulation Wokwi)
- LED RGB cathode commune
- Résistances (optionnelles, pour protéger les broches)
- Câblage de connexion

## 🔌 Connexions

| Broche LED RGB | Broche ESP32 | Description         |
|----------------|--------------|---------------------|
| R              | D11          | Canal Rouge         |
| G              | D10          | Canal Vert          |
| B              | D9           | Canal Bleu          |
| COM (GND)      | 5V ou GND*   | Alimentation (commun) |

> ⚠️ Remarque : Dans la simulation Wokwi, la LED est reliée à **5V** car elle est en **cathode commune**, ce qui signifie que pour éteindre une couleur, on applique 0, et pour l’allumer, on applique une valeur PWM (0-255). Si tu passes sur ESP32, attention à la logique inverse si ta LED est **anode commune** (courant souvent inversé).

## 💡 Fonctionnement

Le programme utilise la fonction `analogWrite()` (simulée sur Arduino Uno) pour modifier l’intensité lumineuse de chaque couleur. Les combinaisons de couleurs sont :

1. **Cyan** : Rouge = 0, Vert = 255, Bleu = 255
2. **Magenta** : Rouge = 255, Vert = 0, Bleu = 255
3. **Jaune** : Rouge = 255, Vert = 255, Bleu = 0

Chaque couleur est affichée pendant **1 seconde**, en boucle.

## 📜 Code source

```cpp
int red_led = 11;
int green_led = 10;
int blue_led = 9;

void setup() {
  pinMode(red_led, OUTPUT);
  pinMode(green_led, OUTPUT);
  pinMode(blue_led, OUTPUT);
}

void loop() {
  led_color(0,255,255);   // Cyan
  delay(1000);
  led_color(255,0,255);   // Magenta
  delay(1000);
  led_color(255,255,0);   // Jaune
  delay(1000);
}

void led_color(int red_value, int green_value,int blue_value)
{
  analogWrite(red_led, red_value);
  analogWrite(green_led, green_value);
  analogWrite(blue_led, blue_value);
}
```

## 🚀 Portage vers ESP32

L'ESP32 utilise des broches PWM différentes. Tu peux modifier les numéros de broches comme suit par exemple :

```cpp
int red_led = 25;
int green_led = 26;
int blue_led = 27;
```

Et remplacer `analogWrite()` (qui n'existe pas directement sur ESP32 Arduino Core) par des fonctions `ledcWrite()` après avoir configuré les canaux `ledcSetup()` et `ledcAttachPin()`.
