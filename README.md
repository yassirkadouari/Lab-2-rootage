#  Lab-2-rootage : Rooting Android - Pixel 6.2 AVD
**Rapport complet de pentest mobile - Comprendre le rooting et ses impacts**

---

## Fiche Périmètre

- **App + version** : Android OS (Pixel 6.2 writable-system)  
- **Support** : AVD – Émulateur Android Studio (Android Virtual Device)  
- **Objectif** : Comprendre rooting et impacts  
- **Données** : Fictives  
- **Réseau** : Test  

---

## Définition du Rooting (4 phrases)

Le rooting est l’obtention des droits administrateur (UID 0) sur un appareil Android.  
Il permet de contourner toutes les restrictions imposées par le fabricant et le système d’exploitation.  
Une fois rooté, l’utilisateur peut modifier ou supprimer n’importe quel fichier système, installer des modules système ou contourner les protections sandbox.  
C’est l’équivalent Android du « jailbreak » sur iOS ou du passage en administrateur sur Windows.

---

## Schéma simple – Chaîne de confiance Verified Boot / AVB

```mermaid
flowchart TD
    A[ROM - Bootloader verrouillé] -->|Vérification signature| B[Bootloader]
    B -->|AVB - Android Verified Boot| C[Kernel + System dm-verity]
    C -->|État orange = déverrouillé| D[Android - Système modifiable en root]

```


## Matrice de risques (8 risques)

Perte totale de la garantie constructeur
Exposition des données sensibles (clés API, mots de passe)
Installation plus facile de malwares/rootkits persistants
Blocage des mises à jour OTA officielles
Détection et blocage par les apps bancaires
Risque de brick (appareil inutilisable)
Contournement des protections SELinux et sandbox
Risque légal en environnement professionnel


## Mesures défensives (8 mesures)

Réseau isolé pour éviter toute communication non contrôlée
Données fictives uniquement
Device/AVD dédié exclusivement aux tests
Snapshots ou wipe complet en fin de séance
Journal de configuration détaillé
Aucun compte personnel utilisé
Contrôle strict des APK installées
Horodatage + captures systématiques


## OWASP MASVS – 2 exigences résumées

STORAGE-1 : Les données sensibles doivent être stockées avec chiffrement approprié.
NETWORK-1 : Toutes les communications doivent utiliser TLS avec validation stricte des certificats.


## OWASP MASTG – 2 idées de tests

Vérifier si l’application détecte le root (su, Magisk, /system rw) et bloque les fonctionnalités.
Intercepter le trafic HTTPS après installation d’un certificat CA rooté.


## Preuves techniques
Capture 1 – Verified Boot State = orange
<img width="962" height="329" alt="{BFA71AAD-4A2C-4C7A-8FBB-1797DD468621}" src="https://github.com/user-attachments/assets/8efce619-21f4-4d7b-b072-5d9c6a36049e" />

Capture 2 – Mobexler : adb root + remount succeeded
<img width="661" height="166" alt="{0D9E1194-8A8E-4BB2-AF54-DEF3A4816A19}" src="https://github.com/user-attachments/assets/0d8f7eff-63ca-4756-ae80-28bb73d5717f" />

Capture 3 – Fastboot en attente (limitation émulateur)
     A note apres plusieurs essaie sur plusieur emulateur, j'en ai conclu apres mes recherche que les emulateur android n'emule pas le bootloader et du coup c'est impossible d'utiliser fastboot dans ce cas, il faudra utiliser un vrai appareil android que je n'ai pas dans ma disposition
Capture 4 – uid=0(root) + disable-verity + reboot
<img width="773" height="257" alt="{E9FF2847-795D-4F25-8144-4DF3EE57C22B}" src="https://github.com/user-attachments/assets/ec508143-a177-4267-a46c-2588ce0e86d9" />
Capture 5 – Application de test en cours d’exécution sur l’AVD rooté
<img width="925" height="902" alt="{ABCDFB53-7A3E-4CBC-A074-64C5EA615BF5}" src="https://github.com/user-attachments/assets/de350965-a8a6-4b44-b2d8-9962e02ff1ea" />

## Fiche Environnement

Type : AVD Pixel 6.2 writable-system (Android Studio)
Version Android : 12
Root : Activé (uid=0(root))
Verity : Désactivé (verity is already disabled)
Verified Boot State : orange
Accès : ADB root + remount rw
Preuve log : logcat_root_check.txt généré


## Checklist Finale
Début

✓Périmètre écrit
✓ AVD neuf
✓ App de test installée
✓ Versions notées

## Fin

✓ Données de test supprimées
✓ Reset/wipe effectué
✓ Preuves du reset sauvegardées
✓ Rapport + traçabilité sauvegardés
✓ Aucun compte personnel utilisé

