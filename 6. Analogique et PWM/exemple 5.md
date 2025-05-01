# potentiomètre + buzzer
Dans cet exemple, on utilise un potentiomètre branché sur une entrée analogique de l’ESP32 pour piloter un buzzer piezo. La valeur lue (0–4095) est affichée dans le moniteur série, puis comparée à un seuil (1500) ; si elle est dépassée, la sortie digitale active le buzzer. Ce montage illustre l’usage de `analogRead()`, de la communication série (`Serial.print()`), et de la commande numérique via `digitalWrite()` et `delay()`.  

## Matériel et principe  

### Potentiomètre  
Un potentiomètre est une résistance à trois bornes avec un curseur mobile, formant un diviseur de tension ajustable. Utilisé sur deux bornes (une extrémité + curseur), il se comporte comme une résistance variable (rhéostat)  ([Potentiometer - Wikipedia](https://en.wikipedia.org/wiki/Potentiometer)). On l’emploie classiquement pour le contrôle de volume, de luminosité ou de vitesse de moteur  ([Potentiometer - Wikipedia](https://en.wikipedia.org/wiki/Potentiometer)).  

### Buzzer (piezo)  
Un buzzer (ou beeper) est un dispositif de signalisation sonore, souvent piézoélectrique, générant un son lorsqu’on lui applique un signal électrique. Il sert par exemple d’alarme ou de retour utilisateur  ([Buzzer - Wikipedia](https://en.wikipedia.org/wiki/Buzzer)).  

### Seuil de déclenchement  
Le code compare la valeur analogique du potentiomètre à une constante `ANALOG_THRESHOLD` ; si la lecture dépasse ce seuil, le buzzer s’active.  

## Montage du circuit  

1. **Potentiomètre**  
   - Extrémité 1 → GND de l’ESP32  
   - Curseur (wiper) → GPIO35 (ADC0)  ([ESP32 - Potentiometer Triggers Piezo Buzzer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer-triggers-piezo-buzzer?utm_source=chatgpt.com))  
   - Extrémité 2 → 3.3 V de l’ESP32  
2. **Buzzer**  
   - Fil rouge → GPIO21 via une résistance de 100 Ω (protection)  ([Potentiometer Triggers Piezo Buzzer - Arduino Tutorial](https://www.circuits-diy.com/potentiometer-triggers-piezo-buzzer-arduino-tutorial/?utm_source=chatgpt.com))  
   - Fil noir → GND de l’ESP32  

> _Voir la simulation Wokwi :_ https://wokwi.com/projects/429497725675530241  

## Code Arduino (ESP32)  

```cpp
#define POTENTIOMETER_PIN 35    // ESP32 GPIO35 (ADC0) connecté au potentiomètre
#define BUZZER_PIN        21    // ESP32 GPIO21 connecté au buzzer
#define ANALOG_THRESHOLD  1500  // Seuil de déclenchement

void setup() {
  pinMode(BUZZER_PIN, OUTPUT);     // configure la broche buzzer en sortie  ([pinMode() - Arduino Reference](https://www.arduino.cc/en/Reference/pinMode?utm_source=chatgpt.com))
  Serial.begin(9600);              // initialise la communication série à 9600 baud  ([Analog Read Serial | Arduino Documentation](https://www.arduino.cc/en/Tutorial/AnalogReadSerial?utm_source=chatgpt.com))
}

void loop() {
  int analogValue = analogRead(POTENTIOMETER_PIN);  // lecture ADC (0–4095)  ([analogRead() - Arduino Reference](https://www.arduino.cc/en/Reference/analogRead?utm_source=chatgpt.com))
  Serial.print("Potentiometer Reading: ");
  Serial.println(analogValue);                     // affichage de la valeur lue  ([Analog Read Serial | Arduino Documentation](https://www.arduino.cc/en/Tutorial/AnalogReadSerial?utm_source=chatgpt.com))

  if (analogValue > ANALOG_THRESHOLD) {
    digitalWrite(BUZZER_PIN, HIGH);  // active le buzzer  ([22.1: digitalWrite() - Engineering LibreTexts](https://eng.libretexts.org/Bookshelves/Electrical_Engineering/Electronics/Embedded_Controllers_Using_C_and_Arduino_%28Fiore%29/22%3A_Bits_and_Pieces__digitalWrite%28%29/22.1%3A_digitalWrite%28%29?utm_source=chatgpt.com))
    Serial.println("Buzzer is ON");
  } 
  else {
    digitalWrite(BUZZER_PIN, LOW);   // désactive le buzzer  ([22.1: digitalWrite() - Engineering LibreTexts](https://eng.libretexts.org/Bookshelves/Electrical_Engineering/Electronics/Embedded_Controllers_Using_C_and_Arduino_%28Fiore%29/22%3A_Bits_and_Pieces__digitalWrite%28%29/22.1%3A_digitalWrite%28%29?utm_source=chatgpt.com))
    Serial.println("Buzzer is OFF");
  }

  delay(500);  // pause 500 ms pour stabilité  ([22.1: digitalWrite() - Engineering LibreTexts](https://eng.libretexts.org/Bookshelves/Electrical_Engineering/Electronics/Embedded_Controllers_Using_C_and_Arduino_%28Fiore%29/22%3A_Bits_and_Pieces__digitalWrite%28%29/22.1%3A_digitalWrite%28%29?utm_source=chatgpt.com))
}
```  

## Explications détaillées  

1. **Lecture analogique**  
   - `analogRead(POTENTIOMETER_PIN)` convertit la tension (0–3.3 V) en une valeur entière 0–4095 grâce à l’ADC 12 bits de l’ESP32  ([analogRead() - Arduino Reference](https://www.arduino.cc/en/Reference/analogRead?utm_source=chatgpt.com)).  
2. **Affichage série**  
   - `Serial.begin(9600)` et `Serial.print()`/`Serial.println()` permettent de visualiser en temps réel la valeur du potentiomètre et l’état du buzzer dans le Moniteur Série  ([Analog Read Serial | Arduino Documentation](https://www.arduino.cc/en/Tutorial/AnalogReadSerial?utm_source=chatgpt.com)).  
3. **Comparaison au seuil**  
   - Si `analogValue > ANALOG_THRESHOLD` (1500), on passe la broche du buzzer à `HIGH`, sinon à `LOW`.  
4. **Commande digitale**  
   - `pinMode(..., OUTPUT)` configure la broche en sortie, puis `digitalWrite()` applique 3.3 V (HIGH) ou 0 V (LOW)  ([digitalWrite() | Arduino Documentation](https://www.arduino.cc/en/Reference/digitalWrite?utm_source=chatgpt.com)).  
5. **Temporisation**  
   - `delay(500)` suspend l’exécution 500 ms pour éviter un rafraîchissement trop rapide.  

---

**Sources principales :**  
- Potentiometer – Wikipedia (diviseur de tension, 3 bornes)  ([Potentiometer - Wikipedia](https://en.wikipedia.org/wiki/Potentiometer))  
- Buzzer – Wikipedia (signalisation audio piézoélectrique)  ([Buzzer - Wikipedia](https://en.wikipedia.org/wiki/Buzzer))  
- ESP32 potentiometer triggers piezo buzzer – esp32io.com  ([ESP32 - Potentiometer Triggers Piezo Buzzer | ESP32 Tutorial](https://esp32io.com/tutorials/esp32-potentiometer-triggers-piezo-buzzer?utm_source=chatgpt.com))  
- ArduinoGetStarted.com – potentiometer triggers piezo buzzer (code et explication)  ([Arduino - Potentiometer Triggers Piezo Buzzer | Arduino Getting Started](https://arduinogetstarted.com/tutorials/arduino-potentiometer-triggers-piezo-buzzer?utm_source=chatgpt.com))  
- Arduino Reference: analogRead(), digitalWrite(), pinMode(), delay()  ([analogRead() - Arduino Reference](https://www.arduino.cc/en/Reference/analogRead?utm_source=chatgpt.com), [digitalWrite() | Arduino Documentation](https://www.arduino.cc/en/Reference/digitalWrite?utm_source=chatgpt.com))