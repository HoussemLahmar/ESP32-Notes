## Exemple 3 : Clignotement de LED

### Matériel

1. **Breadboard**  
   Une breadboard est un outil de prototypage permettant de monter et tester rapidement un circuit sans soudure.  
   _Figure 9 : Connexion sur breadboard_

2. **Résistance**  
   Les résistances contrôlent le flux de courant dans un circuit en offrant une certaine résistance au passage des électrons. Pour lire la valeur d’une résistance, on utilise le code des bandes de couleur :  
   - Les deux premières bandes indiquent les chiffres significatifs.  
   - La troisième bande donne le multiplicateur (nombre de zéros).  
   - La quatrième (et cinquième, si présente) indique la tolérance.  ([Resistor Color Code | Resistor Standards and Codes - EEPower](https://eepower.com/resistor-guide/resistor-standards-and-codes/resistor-color-code/?utm_source=chatgpt.com))

   _Figure 10 : Code de couleur des résistances_

3. **LED (Light-Emitting Diode)**  
   Une LED est un composant semi-conducteur qui émet de la lumière lorsque le courant la traverse. La résistance en série limite le courant pour protéger la LED contre les surtensions.

---

### Procédure

1. Cliquez sur le lien ci-dessous pour voir et tester la simulation :  
   https://wokwi.com/projects/429496468995834881

2. Saisissez le code suivant dans l’éditeur :  
   ```cpp
   //**** LED Blinking *******
   const int ledPin = 25;

   void setup() {
     // configure la broche 25 en sortie digitale
     pinMode(ledPin, OUTPUT);
   }

   void loop() {
     digitalWrite(ledPin, HIGH);   // allume la LED
     delay(500);                   // attend 500 millisecondes
     digitalWrite(ledPin, LOW);    // éteint la LED
     delay(500);                   // attend 500 millisecondes
   }
   ```  
    ([blink.ino - Wokwi - Online ESP32, STM32, Arduino Simulator](https://wokwi.com/projects/344891652101374548?utm_source=chatgpt.com))

3. Démarrez la simulation et observez le clignotement de la LED dans la vue « Serial » ou « Simulation ».

---

### Explication du code

- La fonction `setup()` est exécutée une fois au démarrage : elle configure la broche `ledPin` (25) en sortie via `pinMode(ledPin, OUTPUT)`.  
- La boucle `loop()` s’exécute en continu :  
  1. `digitalWrite(ledPin, HIGH)` applique la tension et allume la LED.  
  2. `delay(500)` interrompt le programme pendant 500 ms (0,5 s).  
  3. `digitalWrite(ledPin, LOW)` coupe la tension et éteint la LED.  
  4. `delay(500)` attend encore 500 ms avant de recommencer.  

Ainsi, la LED fait un « blink » toutes les 500 ms, créant un effet de clignotement régulier.