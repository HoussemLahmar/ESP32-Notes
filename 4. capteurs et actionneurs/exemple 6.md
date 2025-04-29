## Exemple 6 : Détecteur d'obstacles à l'aide d'un capteur à ultrasons

Le capteur à ultrasons (HC-SR04) utilise le principe du sonar pour déterminer la distance d'un objet. 

Les capteurs à ultrasons sont largement utilisés pour :
- détecter des objets,
- mesurer la proximité,
- éviter les obstacles,
- mesurer des niveaux (dans les domaines de la robotique, de l'automatisation, de l'automobile et des processus industriels).

### Procédure

1. Cliquez sur le lien suivant pour voir et tester la simulation :  
   [Simulation Wokwi](https://wokwi.com/projects/378210688300606465)

2. Réalisez le montage du circuit 


---

### Explication du code

Ce code mesure en continu la distance à l’aide d’un capteur à ultrasons et affiche les résultats dans le moniteur série.

- `const int trigPin = 5;` et `const int echoPin = 18;` définissent les broches GPIO utilisées pour envoyer et recevoir les signaux du capteur.
- `#define SOUND_SPEED 0.034` et `#define CM_TO_INCH 0.393701` définissent respectivement la vitesse du son (en cm/μs) et le facteur de conversion des centimètres en pouces.

#### Fonction `loop()`

- Déclenche le capteur pour envoyer des impulsions.
- Calcule la durée nécessaire au retour de l’écho.
- En déduit la distance à l’aide de la vitesse du son, puis convertit cette distance en pouces.
- Affiche la distance en centimètres et en pouces dans le moniteur série.
- Attend 1 seconde avant de répéter la mesure.

