# -Bypass-Detection-Root-Android-avec-Medusa

**Étapes du Lab**

Étape 1 : Constat de la détection

<img width="600" height="782" alt="image" src="https://github.com/user-attachments/assets/944f4a78-9b97-425b-9e82-6acbb358a334" />

Au lancement de l'application Uncrackable1, un message d'alerte "Root detected!" s'affiche. L'application refuse de fonctionner et se ferme dès que l'on appuie sur "OK". C'est notre point de départ.

Étape 2 : Vérification des prérequis Python

<img width="1702" height="427" alt="Screenshot 2026-05-01 130949" src="https://github.com/user-attachments/assets/e1400fdc-a5dc-4aac-a9da-d44b565c1982" />

Avant de lancer Medusa, nous vérifions que toutes les dépendances Python nécessaires sont installées (frida, frida-tools, androguard, etc.). On utilise la commande : pip install -r requirements.txt


Étape 3 : Initialisation du serveur Frida

<img width="1696" height="727" alt="Screenshot 2026-05-01 130754" src="https://github.com/user-attachments/assets/9e5ee137-6ef1-42be-a8b3-814fc414804b" />


L'instrumentation nécessite un serveur Frida actif sur l'émulateur :

Vérification de l'architecture (x86_64).

Transfert du binaire frida-server vers /data/local/tmp/.

Attribution des droits d'exécution (chmod 755).

Lancement du serveur en mode root via adb root.


Étape 4 : Vérification de la communication

<img width="1247" height="783" alt="Screenshot 2026-05-01 130848" src="https://github.com/user-attachments/assets/0d28f928-e124-449d-aa20-8d3077fd5a0b" />

On s'assure que l'hôte peut communiquer avec le serveur Frida :

Redirection des ports TCP (27042, 27043).

Test avec frida-ps -Uai pour lister les applications installées sur l'appareil. On y voit bien owasp.mstg.uncrackable1.

Étape 5 : Lancement de Medusa

<img width="1559" height="788" alt="image" src="https://github.com/user-attachments/assets/414c4723-550e-44fb-8050-1b84a747dec5" />

Lancement de l'outil avec python medusa.py. Medusa détecte automatiquement l'émulateur (index 3) et liste les applications tierces disponibles pour l'instrumentation.

Étape 6 : Bypass et Validation Finale

<img width="1712" height="849" alt="image" src="https://github.com/user-attachments/assets/8731cb7d-efea-49e9-b374-650e5c8a4958" />

C'est l'étape cruciale de l'instrumentation dynamique :

Sélection du module : use root_detection/universal_root_detection_bypass.

Injection : run -f owasp.mstg.uncrackable1.

Compilation : On accepte la recompilation du script (y).

Résultat : Les logs montrent que Medusa intercepte les appels système (fichiers su, Superuser.apk, etc.) et renvoie des valeurs "fausses" à l'application. Comme on le voit sur l'image du téléphone à droite, l'application est maintenant ouverte et fonctionnelle sans message d'erreur.
