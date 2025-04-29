## Exemple 4 : Contrôle de la luminosité d’une LED avec un potentiomètre

https://wokwi.com/projects/429594490057164801

### Matériel et principe de fonctionnement  
- Un potentiomètre est un rhéostat à trois bornes constitué d’une résistance et d’un curseur, ce qui permet d’ajuster la position du contact pour obtenir un diviseur de tension variable  ([ESP32 - Potentiometer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer?utm_source=chatgpt.com)).  
- Employé avec deux bornes (une extrémité et le curseur), il se comporte comme une résistance variable, offrant une résistance ajustable en continu  ([ESP32 - Potentiometer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer?utm_source=chatgpt.com)).  
- Applications courantes :  
  - Contrôle du volume dans les appareils audio  ([ESP32 - Potentiometer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer?utm_source=chatgpt.com))  
  - Réglage de la luminosité dans les systèmes d’éclairage  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))  
  - Régulation de la vitesse dans les moteurs  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))  
  - Contrôle de la température dans les systèmes de chauffage  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))  
  - Interfaces utilisateur analogiques sur divers dispositifs  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))  

### Montage du circuit  
1. **Potentiomètre**  
   - Reliez l’une des extrémités à GND de l’ESP32  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com))  
   - Branchez le curseur (wiper) sur GPIO35 (ADC0)  ([ESP32 - Potentiometer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer?utm_source=chatgpt.com))  
   - Connectez l’autre extrémité au 3.3 V de l’ESP32  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com))  
2. **LED**  
   - Montez la cathode de la LED sur GND via une résistance de 220 Ω  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))  
   - Branchez l’anode de la LED sur GPIO26  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))  
3. **Schéma de câblage**  
   - Voir Figure 11 pour l’agencement complet du potentiomètre et de la LED sur la breadboard.  

### Code Arduino (ESP32)  
```cpp
/* Contrôle de la luminosité de la LED avec un potentiomètre */

#define POTENTIOMETER_PIN 35  // ESP32 GPIO35 (ADC0) connecté au potentiomètre
#define LED_PIN           26  // ESP32 GPIO26 connecté à l'anode de la LED

void setup() {
  Serial.begin(9600);             // initialisation du port série à 9600 baud  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com))
  pinMode(LED_PIN, OUTPUT);       // configure la broche LED en sortie  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))
}

void loop() {
  int analogValue = analogRead(POTENTIOMETER_PIN);           // lecture de la valeur ADC (0–4095)  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com))
  int brightness  = map(analogValue, 0, 4095, 0, 255);      // mise à l'échelle sur 0–255  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com))
  analogWrite(LED_PIN, brightness);                         // écriture PWM pour ajuster la luminosité  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com))

  // Affichage pour débogage
  Serial.print("Analog value = ");
  Serial.print(analogValue);
  Serial.print(" => brightness = ");
  Serial.println(brightness);
  delay(100);                                               // pause de 100 ms  ([ESP32 - Potentiometer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer?utm_source=chatgpt.com))
}
```

### Explications du code  
- La fonction `analogRead(POTENTIOMETER_PIN)` lit la tension issue du potentiomètre et renvoie une valeur entière entre 0 et 4095 (12 bits)  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com)).  
- `map(analogValue, 0, 4095, 0, 255)` convertit cette valeur en une plage adaptée à la commande PWM (0–255)  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com)).  
- `analogWrite(LED_PIN, brightness)` génère un signal PWM dont le rapport cyclique varie selon `brightness`, modulant ainsi l’intensité lumineuse de la LED  ([ESP32 PWM with Arduino IDE (Analog Output) | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-pwm-arduino-ide/?utm_source=chatgpt.com)).  
- Les instructions `Serial.print()` et `Serial.println()` affichent les valeurs lues et calculées dans le Moniteur Série, facilitant le débogage  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com)).  

### Remarques et optimisations  
- Sur ESP32, la fonction native `analogWrite()` peut ne pas être disponible ; on utilise alors les fonctions LEDC (`ledcSetup()`, `ledcAttachPin()`, `ledcWrite()`) pour un contrôle PWM avancé  ([ESP32 PWM Tutorial: Controlling the Brightness of LED](https://circuitdigest.com/microcontroller-projects/esp32-pwm-tutorial-controlling-brightness-of-led?utm_source=chatgpt.com)).  
- Vous pouvez ajuster la résolution et la fréquence PWM via `ledcSetup(channel, frequency, resolution)` selon les besoins de rapidité et de lissage  ([ESP32 PWM Tutorial: Controlling the Brightness of LED](https://circuitdigest.com/microcontroller-projects/esp32-pwm-tutorial-controlling-brightness-of-led?utm_source=chatgpt.com)).  
- Pensez à configurer l’atténuation de l’ADC (`analogSetAttenuation(ADC_11db)`) pour correspondre à la plage de tension du potentiomètre (0–3.3 V)  ([ESP32 Analog Input with Arduino IDE | Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-adc-analog-read-arduino-ide/?utm_source=chatgpt.com)).