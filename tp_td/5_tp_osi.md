# TP – Le modèle OSI à travers l’analyse réseau avec Wireshark

## Rappels théoriques
Le **modèle OSI** est un modèle conceptuel en **7 couches**, utilisé pour :
- Décrire les communications réseau
- Structurer les protocoles
- Diagnostiquer les problèmes réseau

Chaque couche a un rôle précis et communique uniquement avec les couches adjacentes.

---

## Partie 1 – Démarrer une capture réseau

### Étape 1 – Lancer Wireshark
1. Ouvrir Wireshark
2. Identifier l’interface réseau active :
   - Ethernet (filaire)
   - Wi-Fi (sans fil)
3. Démarrer la capture sur cette interface

> **Remarque** : pendant la capture, tout le trafic réseau du poste est enregistré.

---

### Étape 2 – Générer du trafic
1. Ouvrir un navigateur web
2. Accéder à l’adresse suivante :

https://example.com

3. Attendre le chargement complet de la page
4. Revenir dans Wireshark et **arrêter la capture**

---



## Partie 2 – Filtrer et isoler le trafic pertinent

### Étape 3 – Utilisation des filtres
Appliquer successivement les filtres suivants dans la barre de filtre :

tcp

http

tls


Observer à chaque fois :
- Le nombre de paquets affichés
- Le type d’informations visibles

---

### Questions
1. Pourquoi voit-on du trafic TCP même lorsque l’on utilise HTTP ou HTTPS ?
2. Quelle différence observez-vous entre HTTP et TLS ?

---

## Partie 3 – Analyse OSI d’un paquet

### Étape 4 – Sélection d’un paquet
1. Sélectionner un paquet TCP ou HTTP/HTTPS
2. Déplier les différentes couches dans le panneau inférieur de Wireshark

---

### Étape 5 – Identification des couches OSI

Compléter le tableau suivant :

| Élément observé dans Wireshark | Couche OSI correspondante | Rôle |
|-------------------------------|---------------------------|------|
| Ethernet II | | |
| Adresse MAC source / destination | | |
| IPv4 / IPv6 | | |
| Adresse IP source / destination | | |
| TCP | | |
| Port source / destination | | |
| HTTP ou TLS | | |
| Données applicatives | | |

---

## Partie 4 – Encapsulation et décapsulation

### Étape 6 – Compréhension du cheminement des données

1. À partir de l’observation précédente, expliquer :
   - Quelles couches ajoutent des informations (headers)
   - À quel moment ces informations sont retirées
2. Pourquoi :
   - Le navigateur ne connaît pas l’adresse MAC du serveur ?
   - L’adresse MAC change à chaque saut réseau ?
3. Expliquer le rôle de chaque couche dans l’acheminement des données.

---

## Partie 5 – HTTP vs HTTPS

### Étape 7 – Comparaison
1. Identifier une requête HTTP (si possible)
2. Identifier une requête HTTPS
3. Comparer :
   - Lisibilité des données
   - Protocole utilisé
   - Couche OSI impliquée dans le chiffrement

---

### Questions
1. Pourquoi le contenu HTTPS est-il illisible ?
2. À quelle couche OSI appartient le chiffrement ?
3. Le protocole TCP est-il chiffré ?

---

## Partie 6 – Raisonnement par couche OSI

### Étape 8 – Diagnostic

Pour chaque situation suivante, indiquer :
- La **couche OSI concernée**
- Une **justification**

1. Le câble réseau est débranché
2. Le DNS ne répond pas
3. Le serveur répond, mais l’application affiche une erreur
4. La connexion TCP s’établit, mais la page ne s’affiche pas

