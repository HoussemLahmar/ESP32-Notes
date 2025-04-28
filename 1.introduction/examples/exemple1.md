# Exemple 1 : Moniteur Série

[Lien du projet sur Wokwi](https://wokwi.com/projects/429428204405658625)

```cpp
void setup() {
  // Initialisation du code ici (exécuté une fois)
  Serial.begin(9600);
}

void loop() {
  // Code principal ici (exécuté en boucle)
  Serial.println("Formation ESP32");
  delay(1000); // Ajout d'un délai pour une répétition toutes les secondes
}




[Lien du projet sur Wokwi](https://wokwi.com/projects/429428204405658625)

```cpp
void setup() {
  // Initialisation du code ici (exécuté une fois)
  Serial.begin(9600);
}

void loop() {
  // Code principal ici (exécuté en boucle)
  Serial.println("Formation ESP32");
  delay(1000); // Ajout d'un délai pour une répétition toutes les secondes
}
```

---

## **Comportement lors de l'exécution**
Après téléversement du code sur une carte ESP32 et ouverture du Moniteur Série, le message `"Formation ESP32"` s'affichera de manière répétée **toutes les secondes**. Ce programme simple permet d'afficher un message d'accueil et encourage l'apprentissage de l'IoT.

---

## **Explication du code**

✓ **`void setup()`**  
Fonction spéciale en programmation Arduino, exécutée **une seule fois** au démarrage du microcontrôleur.  
- `Serial.begin(9600)` : Initialise la communication série à un débit en bauds de **9600 bits par seconde**.

✓ **`void loop()`**  
Fonction exécutée **en boucle** après `setup()`. Contient le programme principal.  
- `Serial.println("Formation ESP32")` : Envoie la chaîne de caractères vers le port série.  
- `delay(1000)` : Pause de 1000 ms (1 seconde) entre chaque affichage.

✓ **Moniteur Série**  
Ouvrez-le via l'icône 🔍 de l'IDE Arduino ou avec `Ctrl+Maj+M`. Assurez-vous que le débit en bauds correspond à `9600`.
```

