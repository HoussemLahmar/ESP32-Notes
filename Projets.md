# Mini-Projets ESP32 sur Wokwi

Ce document présente une série de mini-projets pour ESP32 que vous pouvez réaliser sur la plateforme de simulation Wokwi. Les projets sont organisés par technologie et classés selon deux niveaux de difficulté: Facile et Intermédiaire.

## Table des matières
1. [Serial Monitor](#1-serial-monitor)
2. [GPIO](#2-gpio)
3. [Analogique et PWM](#3-analogique-et-pwm)
4. [UART](#4-uart)
5. [Servomoteur](#5-servomoteur)
6. [Capteur température LM35](#6-capteur-température-lm35)
7. [DHT11 et DHT22](#7-dht11-et-dht22)
8. [LEDs](#8-leds)
9. [Écran OLED](#9-écran-oled)
10. [Keypad](#10-keypad)
11. [SPI](#11-spi)
12. [I2C](#12-i2c)

---

## 1. Serial Monitor

### Projet Facile: Compteur Simple

**Objectif**: Créer un compteur qui affiche les nombres de 1 à 100 dans le moniteur série.

**Étapes**:
1. Créer un nouveau projet ESP32 sur Wokwi
2. Configurer le Serial Monitor à 115200 bauds
3. Initialiser une variable de compteur à 0
4. Dans la boucle principale:
   - Incrémenter le compteur
   - Afficher sa valeur dans le Serial Monitor
   - Ajouter un délai de 1 seconde
5. Réinitialiser le compteur à 0 lorsqu'il atteint 100

**Matériel**: ESP32 uniquement

### Projet Intermédiaire: Interface de Commande Série

**Objectif**: Créer une interface qui accepte des commandes depuis le Serial Monitor pour contrôler certaines fonctionnalités.

**Étapes**:
1. Configurer le Serial Monitor à 115200 bauds
2. Créer un menu d'aide qui s'affiche au démarrage avec les commandes disponibles
3. Implémenter les commandes suivantes:
   - `help`: Affiche la liste des commandes disponibles
   - `led on`: Allume la LED intégrée
   - `led off`: Éteint la LED intégrée
   - `status`: Affiche l'état actuel de la LED et le temps d'exécution
   - `reset`: Redémarre l'ESP32
4. Lire les entrées du Serial Monitor et traiter les commandes
5. Fournir des retours sur chaque commande exécutée

**Matériel**: ESP32 uniquement

---

## 2. GPIO

### Projet Facile: Contrôle de LED avec Bouton-Poussoir

**Objectif**: Allumer/éteindre une LED en appuyant sur un bouton.

**Étapes**:
1. Connecter une LED au GPIO 23 avec une résistance de 220 ohms
2. Connecter un bouton-poussoir au GPIO 15 (avec résistance pull-up)
3. Configurer la LED comme sortie et le bouton comme entrée
4. Lire l'état du bouton en continu
5. Lorsque le bouton est pressé, allumer la LED
6. Lorsque le bouton est relâché, éteindre la LED
7. Afficher l'état de la LED sur le Serial Monitor

**Matériel**:
- ESP32
- 1 LED
- 1 résistance de 220 ohms
- 1 bouton-poussoir
- 1 résistance de 10k ohms (pull-up)

### Projet Intermédiaire: Chenillard avec Interruption

**Objectif**: Créer un chenillard de LEDs dont la vitesse peut être modifiée par un bouton.

**Étapes**:
1. Connecter 6 LEDs aux GPIOs 13, 12, 14, 27, 26, 25 (avec résistances)
2. Connecter un bouton-poussoir au GPIO 4 (avec résistance pull-up)
3. Configurer toutes les LEDs comme sorties
4. Configurer le bouton comme entrée avec interruption
5. Créer une fonction d'interruption qui change la vitesse de défilement du chenillard
6. Implémenter 3 vitesses différentes (lent, moyen, rapide)
7. Allumer les LEDs une par une en séquence, puis éteindre dans le même ordre
8. Afficher la vitesse actuelle sur le Serial Monitor à chaque changement

**Matériel**:
- ESP32
- 6 LEDs
- 6 résistances de 220 ohms
- 1 bouton-poussoir
- 1 résistance de 10k ohms (pull-up)

---

## 3. Analogique et PWM

### Projet Facile: Contrôle de Luminosité d'une LED

**Objectif**: Faire varier la luminosité d'une LED en utilisant le PWM.

**Étapes**:
1. Connecter une LED au GPIO 16 avec une résistance de 220 ohms
2. Configurer le canal PWM avec une résolution de 8 bits
3. Configurer la fréquence PWM à 5000 Hz
4. Créer une boucle qui augmente graduellement la luminosité de 0 à 255
5. Puis diminuer graduellement de 255 à 0
6. Ajouter un petit délai entre chaque changement
7. Afficher la valeur actuelle du PWM sur le Serial Monitor

**Matériel**:
- ESP32
- 1 LED
- 1 résistance de 220 ohms

### Projet Intermédiaire: Contrôle de LED avec Potentiomètre

**Objectif**: Contrôler la luminosité d'une LED avec un potentiomètre et afficher la valeur analogique.

**Étapes**:
1. Connecter une LED au GPIO 16 avec une résistance de 220 ohms
2. Connecter un potentiomètre au GPIO 34 (ADC)
3. Configurer le canal PWM pour la LED
4. Configurer l'entrée analogique (ADC) pour le potentiomètre
5. Lire la valeur du potentiomètre (0-4095)
6. Mapper cette valeur à l'échelle du PWM (0-255)
7. Appliquer cette valeur au PWM de la LED
8. Afficher les valeurs brutes et mappées sur le Serial Monitor
9. Ajouter un bargraph ASCII dans le Serial Monitor pour visualiser le niveau

**Matériel**:
- ESP32
- 1 LED
- 1 résistance de 220 ohms
- 1 potentiomètre de 10k ohms

---

## 4. UART

### Projet Facile: Communication entre Deux Ports Série

**Objectif**: Configurer deux ports série et faire communiquer les données entre eux.

**Étapes**:
1. Configurer le port série standard (Serial) à 115200 bauds
2. Configurer un second port série (Serial2) à 9600 bauds
3. Recevoir des caractères du Serial Monitor
4. Transmettre ces caractères au Serial2
5. Lire les données entrantes sur Serial2
6. Renvoyer ces données vers le Serial Monitor
7. Ajouter un préfixe pour distinguer les données envoyées des données reçues

**Matériel**:
- ESP32 uniquement (simulation de loopback dans Wokwi)

### Projet Intermédiaire: Terminal de Communication UART

**Objectif**: Créer un terminal interactif qui communique avec un périphérique UART externe.

**Étapes**:
1. Configurer Serial à 115200 bauds pour la communication avec le PC
2. Configurer Serial2 à 9600 bauds pour la communication avec un périphérique externe
3. Créer un menu d'options:
   - Envoyer une commande prédéfinie
   - Envoyer un texte personnalisé
   - Afficher l'historique des communications
   - Modifier la vitesse de communication
4. Lire les commandes du Serial Monitor
5. Transférer les commandes appropriées au Serial2
6. Lire les réponses du Serial2 et les afficher sur le Serial Monitor
7. Implémenter un compteur de messages envoyés et reçus

**Matériel**:
- ESP32
- Module UART externe (simulé dans Wokwi)

---

## 5. Servomoteur

### Projet Facile: Contrôle de Position de Servomoteur

**Objectif**: Contrôler la position d'un servomoteur avec des valeurs prédéfinies.

**Étapes**:
1. Connecter un servomoteur au GPIO 26
2. Configurer la bibliothèque ESP32Servo
3. Initialiser le servomoteur avec les valeurs minimales et maximales appropriées
4. Créer une séquence qui déplace le servo:
   - Position 0° (minimum)
   - Attendre 1 seconde
   - Position 90° (milieu)
   - Attendre 1 seconde
   - Position 180° (maximum)
   - Attendre 1 seconde
5. Répéter la séquence en continu
6. Afficher la position actuelle sur le Serial Monitor

**Matériel**:
- ESP32
- 1 servomoteur standard (SG90 ou similaire)

### Projet Intermédiaire: Contrôle de Servomoteur avec Potentiomètre et Boutons

**Objectif**: Contrôler un servomoteur avec un potentiomètre et enregistrer des positions avec des boutons.

**Étapes**:
1. Connecter un servomoteur au GPIO 26
2. Connecter un potentiomètre au GPIO 34
3. Connecter deux boutons-poussoirs aux GPIO 15 et 4
4. Configurer le servomoteur avec la bibliothèque appropriée
5. Lire la valeur du potentiomètre et la mapper à l'angle du servo (0-180°)
6. Utiliser le premier bouton pour enregistrer la position actuelle (jusqu'à 5 positions)
7. Utiliser le second bouton pour exécuter une séquence des positions enregistrées
8. Afficher le mode actuel (manuel/séquence) et l'angle sur le Serial Monitor
9. Implémenter un petit délai entre les mouvements pour éviter les saccades

**Matériel**:
- ESP32
- 1 servomoteur standard
- 1 potentiomètre de 10k ohms
- 2 boutons-poussoirs
- 2 résistances de 10k ohms (pull-up)

---

## 6. Capteur température LM35

### Projet Facile: Thermomètre Simple avec LM35

**Objectif**: Lire la température depuis un capteur LM35 et l'afficher sur le Serial Monitor.

**Étapes**:
1. Connecter le capteur LM35 au GPIO 35 (ADC)
2. Configurer l'entrée analogique avec une résolution appropriée
3. Lire la valeur analogique du capteur
4. Convertir cette valeur en tension (mV)
5. Convertir la tension en température (°C) - le LM35 produit 10mV par °C
6. Afficher la température sur le Serial Monitor toutes les 2 secondes
7. Implémenter un filtre de moyenne mobile pour stabiliser la lecture

**Matériel**:
- ESP32
- Capteur de température LM35

### Projet Intermédiaire: Système d'Alerte de Température avec LM35

**Objectif**: Créer un système qui surveille la température et déclenche des alertes basées sur des seuils.

**Étapes**:
1. Connecter le capteur LM35 au GPIO 35
2. Connecter une LED verte au GPIO 16
3. Connecter une LED jaune au GPIO 17
4. Connecter une LED rouge au GPIO 18
5. Connecter un buzzer passif au GPIO 19
6. Définir trois seuils de température:
   - Normal (<30°C): LED verte allumée
   - Attention (30-40°C): LED jaune allumée
   - Critique (>40°C): LED rouge allumée + buzzer actif
7. Lire et convertir la température du LM35
8. Comparer avec les seuils et activer les alertes appropriées
9. Enregistrer les dépassements de seuils avec horodatage
10. Afficher toutes les informations sur le Serial Monitor

**Matériel**:
- ESP32
- Capteur de température LM35
- 3 LEDs (verte, jaune, rouge)
- 3 résistances de 220 ohms
- 1 buzzer passif

---

## 7. DHT11 et DHT22

### Projet Facile: Moniteur de Température et Humidité

**Objectif**: Lire et afficher la température et l'humidité à partir d'un capteur DHT11.

**Étapes**:
1. Connecter le capteur DHT11 au GPIO 21
2. Installer et configurer la bibliothèque DHT
3. Initialiser le capteur avec le type et la broche appropriés
4. Lire les valeurs de température et d'humidité toutes les 2 secondes
5. Vérifier si la lecture a réussi en contrôlant les valeurs renvoyées
6. Afficher les valeurs sur le Serial Monitor
7. Ajouter une indication de confort basée sur les valeurs (trop sec, confortable, trop humide)

**Matériel**:
- ESP32
- Capteur DHT11
- Résistance pull-up de 10k ohms (si nécessaire)

### Projet Intermédiaire: Enregistreur de Climat avec DHT22

**Objectif**: Créer un enregistreur qui collecte les données de température et d'humidité et génère des statistiques.

**Étapes**:
1. Connecter le capteur DHT22 au GPIO 21
2. Configurer la bibliothèque DHT pour le DHT22
3. Créer un tableau pour stocker les 24 dernières lectures (simulation d'un enregistrement horaire)
4. Prendre une mesure toutes les 5 secondes (simulation accélérée d'une heure)
5. Calculer et afficher des statistiques:
   - Température moyenne/min/max
   - Humidité moyenne/min/max
   - Tendance (hausse, baisse, stable)
   - Indice de chaleur
6. Afficher un graphique ASCII simple dans le Serial Monitor
7. Permettre à l'utilisateur de demander différentes vues des données via des commandes

**Matériel**:
- ESP32
- Capteur DHT22
- Résistance pull-up de 10k ohms (si nécessaire)

---

## 8. LEDs

### Projet Facile: Séquenceur de LEDs

**Objectif**: Créer différentes séquences d'allumage pour un groupe de LEDs.

**Étapes**:
1. Connecter 4 LEDs aux GPIO 13, 12, 14, 27
2. Configurer toutes les LEDs comme sorties
3. Implémenter quatre séquences différentes:
   - Séquence 1: Allumage séquentiel (une par une)
   - Séquence 2: Clignotement alterné (pair/impair)
   - Séquence 3: Effet de respiration (avec PWM)
   - Séquence 4: Allumage aléatoire
4. Changer de séquence toutes les 10 secondes
5. Afficher la séquence active sur le Serial Monitor

**Matériel**:
- ESP32
- 4 LEDs
- 4 résistances de 220 ohms

### Projet Intermédiaire: Matrice de LEDs 3x3

**Objectif**: Contrôler une matrice de LEDs 3x3 pour afficher des motifs et animations.

**Étapes**:
1. Connecter 9 LEDs en matrice 3x3 aux GPIO 21-23 (lignes) et 18-20 (colonnes)
2. Configurer toutes les broches comme sorties
3. Créer une fonction pour allumer une LED spécifique (x,y)
4. Créer une fonction pour effacer toutes les LEDs
5. Implémenter différents motifs:
   - Lettres (A, B, C...)
   - Chiffres (1, 2, 3...)
   - Symboles (flèches, cœur...)
   - Animations (spirale, rebond, vagues...)
6. Ajouter un bouton pour changer de motif
7. Utiliser un scanner de multiplexage pour contrôler efficacement la matrice

**Matériel**:
- ESP32
- 9 LEDs
- 9 résistances de 220 ohms
- 1 bouton-poussoir
- 1 résistance de 10k ohms (pull-up)

---

## 9. Écran OLED

### Projet Facile: Affichage d'Informations Basiques sur OLED

**Objectif**: Afficher du texte et des formes simples sur un écran OLED I2C.

**Étapes**:
1. Connecter l'écran OLED I2C (SSD1306) aux broches SDA (21) et SCL (22)
2. Installer et configurer la bibliothèque Adafruit SSD1306
3. Initialiser l'écran avec la résolution correcte (128x64 ou 128x32)
4. Afficher le texte "Hello World!" en haut de l'écran
5. Afficher un compteur qui s'incrémente chaque seconde
6. Dessiner un rectangle et un cercle
7. Rafraîchir l'affichage toutes les secondes
8. Ajouter une animation simple (un point qui se déplace)

**Matériel**:
- ESP32
- Écran OLED I2C SSD1306 (128x64 ou 128x32)

### Projet Intermédiaire: Station Météo avec OLED

**Objectif**: Créer une mini station météo qui affiche les données d'un capteur DHT sur un écran OLED.

**Étapes**:
1. Connecter l'écran OLED aux broches I2C (SDA 21, SCL 22)
2. Connecter un capteur DHT22 au GPIO 15
3. Configurer les bibliothèques pour l'OLED et le DHT22
4. Créer une interface utilisateur sur l'OLED avec:
   - En-tête avec le titre "Station Météo"
   - Section pour la température (avec icône)
   - Section pour l'humidité (avec icône)
   - Graphique de tendance sur les 10 dernières minutes
   - Horloge/chronomètre dans le coin
5. Mettre à jour les données toutes les 5 secondes
6. Ajouter une animation lors des transitions
7. Implémenter un mode économiseur d'écran après 1 minute d'inactivité

**Matériel**:
- ESP32
- Écran OLED I2C SSD1306 (128x64)
- Capteur DHT22
- Résistance pull-up de 10k ohms (pour DHT22)

---

## 10. Keypad

### Projet Facile: Saisie de Code avec Keypad

**Objectif**: Utiliser un clavier matriciel pour saisir et vérifier un code PIN.

**Étapes**:
1. Connecter un keypad 4x4 ou 4x3 aux GPIO 13, 12, 14, 27 (lignes) et 26, 25, 33, 32 (colonnes)
2. Installer et configurer la bibliothèque Keypad
3. Définir un code PIN secret (par exemple "1234")
4. Lire les touches pressées et les afficher sur le Serial Monitor
5. Stocker les entrées successives pour former le code
6. Vérifier si le code saisi correspond au code secret
7. Allumer une LED verte si le code est correct, rouge si incorrect
8. Réinitialiser après 3 secondes ou après 4 chiffres saisis

**Matériel**:
- ESP32
- Keypad matriciel 4x4 ou 4x3
- 2 LEDs (verte et rouge)
- 2 résistances de 220 ohms

### Projet Intermédiaire: Calculatrice Simple avec Keypad et OLED

**Objectif**: Créer une calculatrice qui utilise un keypad pour la saisie et un écran OLED pour l'affichage.

**Étapes**:
1. Connecter un keypad 4x4 aux GPIO appropriés
2. Connecter un écran OLED I2C aux broches SDA et SCL
3. Configurer les bibliothèques nécessaires
4. Créer une interface de calculatrice sur l'OLED:
   - Zone pour l'opération en cours
   - Zone pour le résultat
   - Indicateur d'état
5. Implémenter les fonctions mathématiques:
   - Addition, soustraction, multiplication, division
   - Effacement (C), égal (=)
   - Nombres décimaux
6. Gérer les erreurs (division par zéro, dépassement)
7. Ajouter une mémoire pour stocker un résultat
8. Ajouter un historique des opérations récentes

**Matériel**:
- ESP32
- Keypad matriciel 4x4
- Écran OLED I2C SSD1306

---

## 11. SPI

### Projet Facile: Affichage sur Écran TFT via SPI

**Objectif**: Utiliser la communication SPI pour contrôler un petit écran TFT.

**Étapes**:
1. Connecter un écran TFT SPI (ILI9341) aux broches:
   - MOSI: GPIO 23
   - MISO: GPIO 19
   - CLK: GPIO 18
   - CS: GPIO 5
   - DC: GPIO 2
   - RST: GPIO 4
2. Configurer la bibliothèque TFT_eSPI ou Adafruit_ILI9341
3. Initialiser l'écran avec les paramètres appropriés
4. Dessiner un texte multicolore
5. Dessiner des formes géométriques (cercle, rectangle, ligne)
6. Créer une animation simple (rebond)
7. Afficher les coordonnées d'un point qui se déplace
8. Mettre à jour l'affichage de manière fluide

**Matériel**:
- ESP32
- Écran TFT SPI (ILI9341 ou similaire)

### Projet Intermédiaire: Lecteur de Carte SD via SPI

**Objectif**: Lire et écrire des données sur une carte SD en utilisant le protocole SPI.

**Étapes**:
1. Connecter le module de carte SD SPI aux broches:
   - MOSI: GPIO 23
   - MISO: GPIO 19
   - CLK: GPIO 18
   - CS: GPIO 5
2. Configurer la bibliothèque SD
3. Initialiser la communication avec la carte SD
4. Créer des fonctions pour:
   - Lister le contenu du répertoire racine
   - Lire un fichier texte et l'afficher
   - Créer un nouveau fichier et y écrire des données
   - Ajouter des données à un fichier existant
5. Créer un fichier journal qui enregistre:
   - L'heure de démarrage
   - Les opérations effectuées
   - Les erreurs rencontrées
6. Implémenter un menu de commandes via le Serial Monitor
7. Ajouter une gestion des erreurs robuste

**Matériel**:
- ESP32
- Module de carte SD SPI

---

## 12. I2C

### Projet Facile: Lecture de Capteur I2C BMP180/BMP280

**Objectif**: Lire la température et la pression atmosphérique d'un capteur BMP180/BMP280 via I2C.

**Étapes**:
1. Connecter le capteur BMP aux broches I2C (SDA: GPIO 21, SCL: GPIO 22)
2. Installer et configurer la bibliothèque Adafruit BMP280 ou BMP180
3. Scanner l'adresse I2C du capteur et vérifier la connexion
4. Initialiser le capteur avec les paramètres par défaut
5. Lire les valeurs de température et de pression
6. Calculer l'altitude approximative à partir de la pression
7. Afficher toutes les données sur le Serial Monitor
8. Prendre une mesure toutes les 2 secondes

**Matériel**:
- ESP32
- Capteur BMP180 ou BMP280

### Projet Intermédiaire: Contrôle de Plusieurs Périphériques I2C

**Objectif**: Contrôler et coordonner plusieurs périphériques sur le bus I2C.

**Étapes**:
1. Connecter les périphériques suivants au bus I2C:
   - Écran OLED SSD1306
   - Capteur BMP280
   - Module RTC DS3231
   - Périphérique d'expansion GPIO PCF8574
2. Scanner le bus I2C et identifier toutes les adresses
3. Configurer chaque périphérique avec sa bibliothèque appropriée
4. Créer un système qui:
   - Lit l'heure depuis le RTC
   - Lit la température et la pression du BMP280
   - Contrôle 4 LEDs via le PCF8574
   - Affiche toutes les informations sur l'OLED
5. Organiser l'interface OLED avec différentes pages
6. Permettre la navigation entre les pages avec un bouton
7. Implémenter un mode d'alerte qui fait clignoter une LED si la température dépasse un seuil
8. Enregistrer les données toutes les heures dans une structure en mémoire

**Matériel**:
- ESP32
- Écran OLED SSD1306
- Capteur BMP280
- Module RTC DS3231
- Expandeur GPIO PCF8574
- 4 LEDs
- 4 résistances de 220 ohms
- 1 bouton-poussoir
- 1 résistance de 10k ohms (pull-up)