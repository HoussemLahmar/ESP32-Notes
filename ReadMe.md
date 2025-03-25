# Formation Introductive à l'ESP32  
*Par Z-training*  
**Formateur : Houssem Eddine Lahmer**  

---

## 📌 Informations Générales  
- **Format** : En ligne  
- **Outil de simulation** : [Wokwi](https://wokwi.com)  
- **Partage de code** : GitHub/Gist  

---

## 📅 Programme Détaillé  

### **1. Introduction et Bases (4h)**  
#### *1.1 Présentation ESP32 (1h)*  
- Architecture et fonctionnalités (WiFi/BLE, GPIO, ADC)  
- Comparaison Arduino vs ESP32  
- Cas d’usage IoT (Objets Connectés, Domotique)  

**TP1 : Découverte de Wokwi**  
- Création d’un premier projet  
- Simulation d’un clignotement de LED  

---

#### *1.2 Gestion des GPIO (2h)*  
- Digital Read/Write  
- Interruptions matérielles  
- Anti-rebond logiciel  

**TP2 : Contrôle LED avec bouton**  
- Simulation d’un bouton poussoir + LED  
- Exemple : Allumer/éteindre une LED via un bouton  

---

### **2. Entrées/Sorties Avancées (4h)**  
#### *2.1 Analogique et PWM (2h)*  
- ADC 10 bits  
- Sortie PWM (gradation LED, contrôle de moteur)  

**TP3 : Variateur de LED PWM**  
- Utilisation d’un potentiomètre virtuel pour contrôler la luminosité d’une LED  

---

#### *2.2 Communication Série (2h)*  
- Protocole UART  
- Débogage avec `Serial.print()`  

**TP4 : Écho Série**  
- Envoi et réception de données via le moniteur série  

---

### **3. Protocoles de Communication (4h)**  
#### *3.1 I2C (2h)*  
- Principes du bus I2C  
- Lecture de capteurs (exemple : MPU-6050)  

**TP5 : Capteur I2C**  
- Simulation d’un accéléromètre MPU-6050  

---

#### *3.2 SPI (2h)*  
- Différences entre SPI et I2C  
- Communication avec un écran OLED  

**TP6 : Affichage sur écran OLED**  
- Affichage de texte et formes basiques  

---

### **4. WiFi/Bluetooth & Projet Final (4h)**  
#### *4.1 WiFi (2h)*  
- Modes Station (STA) et Point d’Accès (AP)  
- Requêtes HTTP (API REST)  

**TP7 : Serveur Web ESP32**  
- Contrôle des GPIO via un navigateur web  

---

#### *Projet Intégré (2h)*  
- **Objectif** : Système IoT complet avec :  
  - Capteur virtuel (température)  
  - Affichage sur écran OLED  
  - Communication WiFi/MQTT  
  - Contrôle à distance  

**TP8 : Station Météo Connectée**  
- Combinaison des acquis dans un projet Wokwi  

 
---

## 🔗 Ressources Utiles  
- [Documentation ESP32](https://docs.espressif.com/projects/esp-idf/)  
- [Simulateur Wokwi](https://wokwi.com)  

