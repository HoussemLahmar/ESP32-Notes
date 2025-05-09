# Résolution des convertisseurs DAC/ADC : Comprendre les bits, nombres binaires et valeurs clés

## Introduction

Les convertisseurs analogique-numérique (ADC) et numérique-analogique (DAC) sont au cœur de l'électronique moderne, formant l'interface entre notre monde physique (analogique) et le monde numérique des ordinateurs. Pour bien comprendre ces dispositifs, il est essentiel de maîtriser les concepts de **résolution en bits**, **représentation binaire**, et la signification de valeurs clés comme **0**, **255** et **4095**.

## 1. Fondamentaux du système binaire

### 1.1 Qu'est-ce qu'un bit ?

Un **bit** (binary digit) est l'unité fondamentale d'information en informatique, pouvant uniquement prendre deux valeurs: **0** ou **1**. C'est l'alphabet de base des ordinateurs et de tous les systèmes numériques.

### 1.2 Représentation en puissances de 2

Dans un nombre binaire, chaque position représente une **puissance de 2**:

| Position | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|----------|---|---|---|---|---|---|---|---|
| Valeur   | 2⁷| 2⁶| 2⁵| 2⁴| 2³| 2²| 2¹| 2⁰|
| Décimal  |128| 64| 32| 16| 8 | 4 | 2 | 1 |

Par exemple, le nombre binaire `10110` équivaut à:
1×2⁴ + 0×2³ + 1×2² + 1×2¹ + 0×2⁰ = 16 + 0 + 4 + 2 + 0 = 22 en décimal.

### 1.3 Regroupements courants de bits

| Terme       | Nombre de bits | Valeurs possibles            | Usage typique                    |
|-------------|----------------|------------------------------|----------------------------------|
| Bit         | 1              | 0 ou 1 (2 valeurs)           | Donnée binaire élémentaire       |
| Nibble      | 4              | 0 à 15 (16 valeurs)          | Chiffre hexadécimal              |
| Octet/Byte  | 8              | 0 à 255 (256 valeurs)        | Caractère ASCII, unité de base   |
| Mot/Word    | 12             | 0 à 4095 (4096 valeurs)      | ADC/DAC en microcontrôleurs      |
| Mot/Word    | 16             | 0 à 65535 (65536 valeurs)    | Unité de traitement CPU          |

## 2. Résolution des convertisseurs

### 2.1 Définition de la résolution

La **résolution** d'un convertisseur ADC ou DAC définit sa capacité à discriminer ou générer des niveaux de tension distincts. Elle est exprimée en **bits** et détermine:

- Le nombre de niveaux distincts que le convertisseur peut représenter
- La précision avec laquelle une tension analogique peut être numérisée ou générée

### 2.2 Calcul du nombre de niveaux

Pour un convertisseur à **N bits**, le nombre de niveaux distincts est:

$$\text{Nombre de niveaux} = 2^N$$

Les valeurs numériques possibles s'étendent de **0** à **2ᴺ - 1**.

### 2.3 Exemples courants

| Résolution | Nombre de niveaux | Plage de valeurs | Exemples d'utilisation                            |
|------------|-------------------|------------------|-------------------------------------------------|
| 8 bits     | 256 (2⁸)          | 0 à 255          | PWM Arduino standard, DAC ESP32 (broches 25/26)  |
| 10 bits    | 1024 (2¹⁰)        | 0 à 1023         | ADC Arduino UNO, Nano, etc.                     |
| 12 bits    | 4096 (2¹²)        | 0 à 4095         | ADC/PWM ESP32, ADC Arduino Due                  |
| 16 bits    | 65536 (2¹⁶)       | 0 à 65535        | ADC/DAC haute précision                         |

## 3. Signification des valeurs clés

### 3.1 Pourquoi 0, 255 et 4095 ?

Ces nombres ne sont pas arbitraires mais découlent directement de la résolution en bits:

- **0** : Représente toujours la valeur minimale (tous les bits à 0)
- **255** : Valeur maximale en 8 bits (2⁸-1) = `11111111` en binaire
- **4095** : Valeur maximale en 12 bits (2¹²-1) = `111111111111` en binaire

### 3.2 Correspondance tension-valeur numérique

Pour un convertisseur utilisant une tension de référence Vref (typiquement 3,3V ou 5V):

| Valeur numérique | Tension en sortie (DAC)                      | Signification                                |
|------------------|---------------------------------------------|---------------------------------------------|
| 0                | 0V                                          | Tension minimale (0% de Vref)                |
| 255 (8 bits)     | Vref × (255/255) = Vref                     | Tension maximale en 8 bits (100% de Vref)    |
| 127 (8 bits)     | Vref × (127/255) ≈ 0,498 × Vref             | Mi-échelle en 8 bits (≈50% de Vref)          |
| 4095 (12 bits)   | Vref × (4095/4095) = Vref                   | Tension maximale en 12 bits (100% de Vref)   |
| 2048 (12 bits)   | Vref × (2048/4095) ≈ 0,5 × Vref             | Mi-échelle en 12 bits (≈50% de Vref)         |

### 3.3 Calcul du pas de quantification (LSB)

Le **pas de quantification** (ou valeur d'un LSB - Least Significant Bit) est la plus petite variation de tension que le convertisseur peut détecter ou générer:

$$\text{Pas de quantification} = \frac{V_{ref}}{2^N - 1}$$

Par exemple, avec Vref = 3,3V:
- En 8 bits: Pas = 3,3V / 255 ≈ 12,94 mV
- En 12 bits: Pas = 3,3V / 4095 ≈ 0,81 mV

Plus la résolution est élevée, plus le pas est petit, et plus la précision est grande.

## 4. Processus de conversion

### 4.1 Étapes de conversion ADC (Analogique vers Numérique)

1. **Échantillonnage**: Prélèvement de la valeur de tension à un instant donné
2. **Maintien**: Stabilisation de cette valeur pendant la durée de conversion
3. **Quantification**: Comparaison avec des seuils de référence pour déterminer la valeur numérique correspondante
4. **Codage**: Génération du code binaire sur N bits

#### Exemple de quantification pour un ADC 3 bits

Pour un ADC 3 bits avec Vref = 5V:
- 8 niveaux possibles (0-7)
- Pas de quantification = 5V / 7 ≈ 0,714V

| Tension d'entrée | Valeur binaire | Valeur décimale |
|------------------|----------------|-----------------|
| 0 - 0,714V       | 000            | 0               |
| 0,714V - 1,428V  | 001            | 1               |
| 1,428V - 2,142V  | 010            | 2               |
| 2,142V - 2,856V  | 011            | 3               |
| 2,856V - 3,570V  | 100            | 4               |
| 3,570V - 4,284V  | 101            | 5               |
| 4,284V - 4,998V  | 110            | 6               |
| 4,998V - 5V      | 111            | 7               |

### 4.2 Étapes de conversion DAC (Numérique vers Analogique)

1. **Réception du code**: Le mot binaire est chargé dans un registre
2. **Pondération**: Chaque bit active ou non un générateur de courant/tension proportionnel à son poids binaire
3. **Sommation**: Addition des contributions de chaque bit
4. **Conditionnement**: Conversion courant-tension et mise à l'échelle selon Vref

#### Types de réseaux DAC

- **Réseau R-2R**: Utilise seulement deux valeurs de résistances (R et 2R) disposées en échelle
- **Réseau de résistances pondérées**: Chaque bit est associé à une résistance de valeur proportionnelle à son poids
- **DAC à capacités commutées**: Utilise des condensateurs chargés/déchargés selon la valeur binaire

#### Exemple de calcul pour un DAC 8 bits

Pour un DAC 8 bits avec Vref = 3,3V et valeur d'entrée = 127:

$$V_{sortie} = \frac{127}{255} \times 3,3V \approx 1,65V$$

## 5. Cas pratiques sur microcontrôleurs

### 5.1 Arduino standard (UNO, Nano, etc.)

- **ADC**: 10 bits (0-1023)
- **PWM (analogWrite)**: 8 bits (0-255)
- Fonction `analogWrite(pin, 255)` → Duty cycle 100% (tension moyenne maximum)

### 5.2 ESP32

- **ADC**: 12 bits natif (0-4095)
- **PWM via LEDC**: Configurable jusqu'à 20 bits, typiquement 8 ou 12 bits
  - 8 bits par défaut avec `analogWrite(pin, 255)` → 100% duty cycle
  - 12 bits avec `ledcWrite(channel, 4095)` → 100% duty cycle
- **DAC hardware** (broches 25/26 uniquement):
  - 8 bits (0-255)
  - `dacWrite(25, 255)` → tension continue de 3,3V

### 5.3 Configuration PWM ESP32 en 12 bits

```cpp
const int canal = 0;
const int freq = 5000;
const int resolution = 12;  // 12 bits → 0-4095
const int pinLED = 16;      // n'importe quelle broche GPIO

void setup() {
  ledcSetup(canal, freq, resolution);  // configure LEDC
  ledcAttachPin(pinLED, canal);        // attache la broche au canal
}

void loop() {
  // Balayage sur toute la plage 12 bits
  for (int valeur = 0; valeur <= 4095; valeur++) {
    ledcWrite(canal, valeur);          // PWM de 0 à 4095
    delay(1);
  }
}
```

## 6. Représentation binaire et hexadécimale

### 6.1 Table de conversion pour valeurs clés

| Valeur décimale | Binaire (8 bits) | Hexadécimal | Binaire (12 bits)     | Hexadécimal |
|-----------------|------------------|-------------|------------------------|-------------|
| 0               | 00000000         | 0x00        | 000000000000           | 0x000       |
| 127 (≈50%)      | 01111111         | 0x7F        | -                      | -           |
| 255 (max 8 bits)| 11111111         | 0xFF        | 000011111111           | 0x0FF       |
| 2048 (≈50%)     | -                | -           | 100000000000           | 0x800       |
| 4095 (max 12 bits)| -              | -           | 111111111111           | 0xFFF       |

### 6.2 Conversion en code Gray

Le code Gray est parfois utilisé dans les ADC pour minimiser les erreurs de conversion, car un seul bit change entre deux valeurs consécutives:

| Décimal | Binaire | Code Gray |
|---------|---------|-----------|
| 0       | 000     | 000       |
| 1       | 001     | 001       |
| 2       | 010     | 011       |
| 3       | 011     | 010       |
| 4       | 100     | 110       |
| 5       | 101     | 111       |
| 6       | 110     | 101       |
| 7       | 111     | 100       |

## 7. Considérations pratiques

### 7.1 Erreurs de conversion

- **Erreur de quantification**: Inhérente à tout convertisseur, égale à ±0,5 LSB
- **Non-linéarité**: Écart par rapport à la droite théorique
- **Erreur d'offset**: Décalage de toute l'échelle
- **Erreur de gain**: Pente incorrecte de la réponse

### 7.2 Techniques d'amélioration de la résolution

- **Suréchantillonnage**: Acquisition de multiples échantillons pour moyenner le bruit
- **Dithering**: Ajout contrôlé de bruit pour améliorer la résolution effective
- **Interpolation**: Estimation des valeurs intermédiaires

### 7.3 Choix de résolution selon l'application

| Application                   | Résolution typique | Justification                                     |
|-------------------------------|-------------------|--------------------------------------------------|
| Contrôle simple LED/moteur    | 8 bits            | Variation perceptible suffisante pour l'œil/charge|
| Acquisition signaux audio     | 16-24 bits        | Gamme dynamique requise pour son de qualité      |
| Instrumentation scientifique  | 16-24 bits        | Mesures précises nécessaires                     |
| Contrôle industriel standard  | 12 bits           | Bon compromis précision/coût                     |

## Conclusion

La compréhension des systèmes en bits est fondamentale en électronique numérique. Les valeurs caractéristiques comme 255 (2⁸-1) et 4095 (2¹²-1) représentent les maximums pour des convertisseurs 8 et 12 bits respectivement. Cette connaissance est essentielle pour:

- Choisir le bon convertisseur selon les besoins de précision
- Interpréter correctement les données obtenues
- Programmer efficacement les microcontrôleurs
- Comprendre les limites intrinsèques des systèmes numériques

Plus la résolution est élevée, plus le système peut représenter finement les variations analogiques, mais au prix d'une plus grande complexité matérielle et logicielle.