# ESP32 ADC to DAC Example

Ce projet illustre la lecture d'un signal analogique via l'ADC de l'ESP32 et la génération d'une sortie PWM simulant un DAC pour piloter une LED. Le code lit la valeur d'un potentiomètre connecté à la broche ADC, convertit cette lecture en une valeur PWM, puis écrit ce PWM sur une broche LED. Les valeurs brutes et la tension équivalente sont affichées sur le moniteur série.

## Matériel requis

- Module ESP32 DevKit V1
- Potentiomètre (10 kΩ conseillé)
- LED (et résistance de 220 Ω)
- Câbles de connexion

## Schéma de connexion

1. **Potentiomètre**  
   - GND → ESP32 GND  
   - VCC → ESP32 3V3  
   - SIG → ESP32 GPIO34 (ADC_PIN)

2. **LED**  
   - Anode → ESP32 GPIO25 (LED_PIN) via une résistance de 220 Ω  
   - Cathode → ESP32 GND

![Wokwi Diagram](https://wokwi.com/render?project=429777346423532545)

## Configuration du code

```cpp
#define ADC_PIN 34    // Broche ADC (ADC1_CH6)
#define LED_PIN 25    // Broche PWM pour simuler le DAC

void setup() {
  // Initialisation du port série à 115200 bauds
  Serial.begin(115200);

  // Configuration de l'ADC
  analogReadResolution(12);         // Résolution 12 bits (0-4095)
  analogSetAttenuation(ADC_11db);   // Atténuation 11 dB pour plage 0-3.3 V

  // Configuration de la LED en sortie PWM
  pinMode(LED_PIN, OUTPUT);
}
```

## Boucle principale

```cpp
void loop() {
  // Lecture ADC (0 à 4095)
  int adcVal = analogRead(ADC_PIN);

  // Conversion pour PWM (0 à 255)
  int dacVal = adcVal / 16;

  // Calcul de la tension (en volts)
  float voltage = adcVal * 3.3 / 4095.0;

  // Écriture PWM sur la LED pour simuler un DAC
  analogWrite(LED_PIN, dacVal);

  // Affichage des valeurs dans le moniteur série
  Serial.print("ADC Val: "); Serial.print(adcVal);
  Serial.print(" | DAC Val: "); Serial.print(dacVal);
  Serial.print(" | Voltage: "); Serial.print(voltage);
  Serial.println(" V");

  delay(100);
}
```

### Détails techniques

- **analogReadResolution(12)** configure l'ADC pour fournir une sortie 12 bits (0–4095).  
- **analogSetAttenuation(ADC_11db)** permet de mesurer des tensions jusqu'à 3,3 V.  
- **analogWrite(LED_PIN, dacVal)** utilise la PWM interne de l'ESP32 (généralement 8 bits) pour simuler une sortie analogique.  
- La conversion `dacVal = adcVal / 16` réduit la plage 12 bits à 8 bits (4096/256 = 16).

## Exécution dans Wokwi

1. Ouvrez le projet [Wokwi ESP32 ADC-DAC Example](https://wokwi.com/projects/429777346423532545).
2. Cliquez sur **Démarrer la simulation**.
3. Ajustez le potentiomètre et observez la luminosité de la LED et les valeurs sur le console série.

## Licence

Ce projet est distribué sous licence MIT.  

*Créé par Houssem Lahmar*
