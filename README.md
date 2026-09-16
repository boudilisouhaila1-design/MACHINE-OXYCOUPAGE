# MACHINE-OXYCOUPAGE

Projet de réalisation d'une **machine automatique d'oxycoupage linéaire** avec commande d'axes, sécurité et interface homme-machine tactile.

## Contenu du dépôt

### 1. Arduino Pro Mini – commande de la machine

Dossier : `Arduino-Pro-Mini/`

Le programme `programme_arduino_facemag_v3.ino` assure notamment :
- la commande de l'axe X avec un driver Leadshine DM2282 ;
- la gestion du déplacement et du homing ;
- la commande de la torche et du variateur de l'axe Y ;
- la lecture des fins de course et des entrées de sécurité ;
- la communication I2C avec le MCP23017 ;
- la gestion des alarmes et des états de la machine.

### 2. ESP32-S3 – HMI tactile

Dossier : `ESP32-HMI/test_afficheur/`

Le programme `test_afficheur.ino` réalise l'interface tactile avec :
- écran RGB 800 × 480 ;
- tactile GT911 ;
- LVGL pour l'interface graphique ;
- LovyanGFX pour la gestion de l'écran ;
- PCA9557 pour l'extension d'E/S I2C ;
- écran de paramètres de coupe ;
- écran de suivi du cycle ;
- affichage de l'état, de la pièce, des positions X/Y et de la progression ;
- commandes PAUSE et STOP.

Le fichier `gfx_config.h` contient la configuration matérielle de l'écran RGB et du tactile GT911.

Le fichier `stellantis.c` contient les données graphiques utilisées pour le logo affiché au démarrage.

### 3. Bibliothèque PCA9557

Dossier : `ESP32-HMI/libraries/PCA9557/`

Cette bibliothèque locale contient les fichiers `PCA9557.h` et `PCA9557.cpp` utilisés par l'interface ESP32-S3.

## Bibliothèques nécessaires

Pour compiler les programmes, installer les bibliothèques correspondantes dans l'IDE Arduino :

### Arduino Pro Mini
- `Wire`
- `Adafruit_MCP23017`

### ESP32-S3
- `lvgl`
- `LovyanGFX`
- `PCA9557` (fournie dans ce dépôt)

Le programme ESP32 utilise également les fichiers d'exemples LVGL référencés par le code. Selon la version de LVGL installée, les chemins d'inclusion peuvent nécessiter une adaptation.

## Matériel principal

- Arduino Pro Mini 5 V / 16 MHz
- MCP23017 – extension d'entrées/sorties I2C
- Driver Leadshine DM2282
- Moteur pas à pas NEMA 42 pour l'axe X
- Variateur Schneider ATV320 pour l'axe Y
- ESP32-S3 avec écran tactile 7 pouces
- Contrôleur tactile GT911
- PCA9557
- Capteurs inductifs et éléments de sécurité

## Architecture générale

```text
                  +----------------------+
                  |      ESP32-S3 HMI    |
                  |  Ecran tactile 7"    |
                  |     LVGL / GT911      |
                  +----------+-----------+
                             |
                            I2C
                             |
                  +----------v-----------+
                  |    Arduino Pro Mini  |
                  |  Commande principale  |
                  +----+-------------+----+
                       |             |
                    MCP23017       DM2282
                       |             |
                 E/S / relais     Axe X
                                     |
                                  NEMA 42

                  Arduino / relais -> Variateur ATV320 -> Axe Y
```

## Utilisation

1. Ouvrir le programme Arduino dans l'IDE Arduino.
2. Sélectionner la carte correspondant au contrôleur utilisé.
3. Installer les bibliothèques nécessaires.
4. Vérifier les paramètres électriques et mécaniques avant toute mise sous tension.
5. Pour l'ESP32-S3, ouvrir `ESP32-HMI/test_afficheur/test_afficheur.ino` et installer LVGL, LovyanGFX et la bibliothèque PCA9557 fournie.

## Sécurité

Ce dépôt contient du code destiné à la commande d'une machine industrielle. **Ne pas utiliser le programme sur une machine réelle sans validation électrique, mécanique et fonctionnelle par une personne qualifiée.** Vérifier notamment les arrêts d'urgence, fins de course, relais, niveaux logiques, sens des axes et paramètres du variateur avant les essais.

## Auteur / Projet

Projet : **Machine automatique d'oxycoupage linéaire**  
Dépôt GitHub : https://github.com/boudilisouhaila1-design/MACHINE-OXYCOUPAGE
