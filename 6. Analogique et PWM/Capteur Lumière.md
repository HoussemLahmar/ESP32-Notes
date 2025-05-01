# Module Capteur de Lumière LDR

Le **module capteur LDR** (Light Dependent Resistor ou **Photocapteur**) est un capteur numérique et analogique économique, capable de mesurer et de détecter l'intensité lumineuse. Ce module intègre une **résistance dépendante de la lumière (LDR)** pour détecter la lumière. Sa sortie passe à l'état **HAUT** en l'absence de lumière et à l'état **BAS** en présence de lumière. La sensibilité du capteur peut être réglée via un potentiomètre intégré.

## Fonctionnalités
- Mesure la luminosité ambiante.
- **Deux sorties** :
  - **Sortie numérique (D0)** : Signal **LOW** (lumière détectée) ou **HIGH** (obscurité).
  - **Sortie analogique (A0)** : Valeur variable en fonction de l'intensité lumineuse.
- Réglage de la sensibilité via un potentiomètre.

## Brochage du Module
- **VCC** : Alimentation (3,3V à 5V).
- **GND** : Masse (0V).
- **D0** : Sortie numérique (HAUT dans l'obscurité, BAS avec de la lumière). Le seuil est ajustable via le potentiomètre.
- **A0** : Sortie analogique. **La valeur diminue lorsque la lumière augmente** et augmente dans l'obscurité.

---

**Objectif** : Créer un système réactif à la luminosité.

## Matériel Simulé
- ESP32
- Photorésistance (LDR)
- LED
- Résistances

## Concepts Appliqués
- Lecture d'un capteur analogique.
- Conditionnement du signal d'entrée.
- Prise de décision basée sur des seuils.
- Contrôle d'une LED selon la luminosité.

## Procédure
Cliquez sur le lien pour tester la simulation :  
   [Simulation Wokwi](https://wokwi.com/projects/378622524029754369)
