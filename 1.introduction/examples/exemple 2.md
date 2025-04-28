## Exemple 2 : Moniteur série  

### Procédure  

1. Cliquez sur le lien ci-dessous pour voir et tester la simulation sur Wokwi :  
   https://wokwi.com/projects/4294283997805496332  
2. Écrivez le code suivant (voir Figure 7) dans l’éditeur :  
   ```cpp
   void setup() {
     // configuration initiale (exécutée une fois)
     Serial.begin(9600);
   }

   void loop() {
     // boucle principale (exécutée en continu)
     Serial.println("Bienvenue à Z-Training");
     Serial.print("\n");
     Serial.println("Apprenons l’IoT");
     delay(1000);
   }
   ```  
3. Cliquez sur l’icône de simulation et observez la sortie affichée dans le moniteur série.  

### Explication du code  

- Lorsque ce code est téléchargé sur une carte ESP32 et que l’on ouvre le Moniteur Série, les messages  
  > “Bienvenue à Z-Training”  
  > “Apprenons l’IoT”  
  se répètent toutes les 1000 millisecondes.  
- `Serial.print("\n");` : envoie un caractère de saut de ligne sur le port série, créant une ligne vide pour améliorer la lisibilité de la sortie.  
- `delay(1000);` : met la carte en pause pendant 1000 ms (1 s) avant de recommencer la boucle.  

---

**Figure 7 : Code du moniteur série**  

```cpp
void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.println("Bienvenue à Z-Training");
  Serial.print("\n");
  Serial.println("Apprenons l’IoT");
  delay(1000);
}
```  

> **Note** : vous pouvez ajuster la vitesse de transmission (`9600` bauds ici) dans le Moniteur Série pour correspondre à celle du `Serial.begin()` (Arduino ESP32 Reference) .