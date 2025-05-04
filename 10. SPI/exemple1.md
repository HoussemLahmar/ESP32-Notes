Cet README présente en détail le projet Wokwi simulant un ESP32 DevKitC-V4 pilotant un écran TFT ILI9341, une LED, un buzzer et trois boutons poussoirs. Il explique la configuration matérielle, les bibliothèques logicielles nécessaires, l’organisation du code et son fonctionnement pas à pas.

Aperçu du projet
Ce projet met en œuvre un système interactif où :

Un ESP32 DevKitC-V4 est simulé dans Wokwi, exploité en mode SPI pour communiquer avec un écran TFT ILI9341 
Welcome to Wokwi! | Wokwi Docs
Wokwi
.

Trois boutons poussoirs (vert, jaune, rouge) génèrent des événements d’appui, gérés avec une logique de désamorçage (debounce) 
Arduino
.

Un buzzer joue une mélodie de démarrage grâce à la fonction tone() d’Arduino 
Arduino
.

Une LED (GPIO 2) s’allume lors de chaque appui, tandis que l’écran affiche un écran contextuel adapté 
Adafruit
.

Matériel et connexions
Toutes les connexions sont définies dans le projet Wokwi (diagram.json) :

Composant	Broche ESP32	Couleur
ILI9341 – MISO	GPIO 26	(vert)
ILI9341 – LED (back)	GPIO 5	(orange)
ILI9341 – SCK	GPIO 19	(cyan)
ILI9341 – MOSI	GPIO 23	(bleu)
ILI9341 – D/C	GPIO 21	(violet)
ILI9341 – RST	GPIO 18	(blanc)
ILI9341 – CS	GPIO 22	(pourpre)
ILI9341 – VCC	5 V	(rouge)
ILI9341 – GND	GND	(noir)
LED – anode + R1 (1 kΩ)	GPIO 2	(vert)
LED – cathode	GND	(vert)
Buzzer (tone) – +	GPIO 13	(rouge)
Buzzer – – GND	GND	(noir)
Bouton vert – entrée	GPIO 27 (INPUT_PULLUP)	(vert)
Bouton jaune – entrée	GPIO 16 (INPUT_PULLUP)	(jaune)
Bouton rouge – entrée	GPIO 32 (INPUT_PULLUP)	(rouge)

La communication SPI avec le TFT suit le schéma standard décrit par Adafruit pour les écrans ILI9341 (mode 0, CS géré manuellement) 
Adafruit
.

Dépendances logicielles
ESP32 Arduino Core (bibliothèque Wokwi simule l’ESP32 DevKitC-V4) 
Welcome to Wokwi! | Wokwi Docs
.

SPI.h : gestion du bus SPI (inclus par défaut dans l’IDE Arduino) 
Arduino Docs
.

Adafruit_GFX & Adafruit_ILI9341 : fonctions graphiques et pilote ILI9341 
Adafruit
.

tone() : génération de signaux sonores pour le buzzer 
Arduino
.

INPUT_PULLUP et désamorçage (debouncing) pour boutons poussoirs 
Circuit Basics
.

Structure du code
1. Initialisation (setup())
Serial à 115200 baud pour le debug.

GPIO 2 en sortie pour la LED.

GPIO 13 en sortie pour le buzzer.

GPIO 27, 16, 32 en INPUT_PULLUP pour les trois boutons, avec gestion de rebonds 
Arduino
.

TFT : appel à tft.begin() et configuration SPI à 40 MHz puis affichage du message d’accueil 
Adafruit
.

Mélodie de démarrage via tone(pin, fréquence, durée) 
Arduino
.

2. Boucle principale (loop())
Lecture des trois boutons, comparaison avec l’état précédent pour détecter changement et filtrage des rebonds avec un delay(50) ms 
Deep Blu Embedded
.

À chaque appui détecté (LOW), on :

Allume la LED (digitalWrite(2, HIGH)).

Appelle showScreen(n) pour afficher l’écran correspondant (1 = rouge, 2 = jaune, 3 = vert).

Au relâchement, on éteint la LED et restaure l’écran blanc.

3. Affichage dynamique (showScreen(int))
Efface l’écran en blanc (fillScreen(WHITE)).

Positionne le curseur et définit la couleur/taille du texte.

Affiche un titre ("Screen red", "Screen yellow", "Screen green") et des éléments graphiques (ex. rectangle couleur) pour l’écran vert.

Utilisation
Charger le code dans Wokwi ou sur un ESP32 DevKitC-V4 réel via l’IDE Arduino.

Lancer la simulation/run.

Observer la séquence startup : logo sur TFT et mélodie sur buzzer.

Interagir : appuyer sur les boutons pour changer d’écran et allumer la LED.

Consulter la console série pour les messages d'état.

Possibilités d’évolution
Écran tactile : remplacer les boutons par une interface tactile capacitive.

Menus : ajouter des sous-menus pour sélectionner des fonctions (capteurs, horloge, graphes).

Communication : envoyer les événements vers un serveur MQTT via Wi‑Fi.

Optimisation : utiliser un timer hardware pour le debounce plutôt que delay().

Graphiques avancés : dessiner des images BMP ou polices personnalisées avec Adafruit_GFX.

Simulateur : Wokwi (https://wokwi.com/projects/410756603224055809)