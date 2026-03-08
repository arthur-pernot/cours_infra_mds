# TP Réseau — Analyse de trafic avec Wireshark

**Durée : 4 heures**  
**Outil : Wireshark**  
**Niveau : Bachelor 1 — Introduction aux réseaux**

---

# Objectifs pédagogiques

Dans ce TP vous allez apprendre à :

- Utiliser **Wireshark** pour capturer et analyser du trafic réseau
- Comprendre les **échanges réels entre un navigateur et Internet**
- Identifier plusieurs protocoles fondamentaux : **ARP, DNS, TCP, HTTP, TLS**
- Lire et interpréter la structure d'un paquet réseau
- Utiliser les **filtres Wireshark** pour isoler un protocole spécifique

À la fin du TP, vous serez capables d'expliquer **ce qui se passe réellement lorsqu'un utilisateur ouvre un site web**.

---

# Contexte

Lorsque vous tapez une URL dans un navigateur (par exemple `https://example.com`), plusieurs protocoles réseau interviennent successivement :

1. **ARP** : trouver l'adresse MAC du routeur
2. **DNS** : traduire le nom de domaine en adresse IP
3. **TCP** : établir une connexion fiable
4. **TLS** : sécuriser la communication
5. **HTTP** : échanger les données du site

Dans ce TP, vous allez **observer ces étapes directement dans le trafic réseau**.

---

# Partie 1 — Installation et prise en main

## Étape 1 — Installation

Installez Wireshark sur votre machine.

Site officiel :

https://www.wireshark.org/

Pendant l'installation :

- acceptez l'installation de **Npcap** (nécessaire pour capturer le trafic)

---

## Étape 2 — Lancer une capture

1. Ouvrez **Wireshark**
2. Repérez l'interface réseau active

Selon votre machine cela peut être :

- `Wi-Fi`
- `Ethernet`
- `enp0s3`
- `wlan0`

Les interfaces actives affichent généralement **un graphique de trafic**.

3. Double‑cliquez sur l'interface active pour démarrer la capture.

Vous devez voir **des paquets apparaître en temps réel**.

---

## Structure de l'interface Wireshark

L'écran se divise en 3 zones :

1. **Liste des paquets capturés**
2. **Détails du paquet sélectionné**
3. **Données brutes (hexadécimal)**

Prenez quelques minutes pour explorer l'interface.

---

## Travail demandé

Répondez aux questions suivantes :

1. Combien de paquets sont capturés en **10 secondes** ?
2. Quels **protocoles** apparaissent dans la capture ?
3. Quelle est **votre adresse IP locale** visible dans les paquets ?
4. Quel est **le port le plus fréquemment utilisé** ?

---

# Partie 2 — Analyse du protocole ARP

## Rappel théorique

Le protocole **ARP (Address Resolution Protocol)** permet de trouver l'adresse **MAC** correspondant à une **adresse IP** sur le réseau local.

Exemple :

"Qui possède l'adresse IP `192.168.1.1` ?"

La machine possédant cette IP répond avec son **adresse MAC**.

Cette étape est nécessaire avant toute communication Ethernet.

---

## Étape 1 — Vider le cache ARP

Pour forcer l'envoi de nouvelles requêtes ARP, videz le cache.

### Windows

```
arp -d *
```

### Linux

```
ip neigh flush all
```

---

## Étape 2 — Capturer le trafic ARP

1. Démarrer une nouvelle capture Wireshark
2. Ouvrir un site web dans votre navigateur
3. Arrêter la capture après quelques secondes

---

## Filtre Wireshark

Dans la barre de filtre :

```
arp
```

---

## Travail demandé

1. Quelle **adresse IP** est recherchée dans la requête ARP ?
2. Quelle **adresse MAC** est retournée ?
3. Combien de **requêtes ARP** apparaissent dans la capture ?
4. Quel équipement correspond probablement à cette adresse IP ?

Indice : il s'agit souvent de **la passerelle (routeur)**.

---

# Partie 3 — Analyse DNS

## Rappel théorique

Le protocole **DNS (Domain Name System)** traduit un **nom de domaine** en **adresse IP**.

Exemple :

```
example.com → 93.184.216.34
```

Sans DNS, il faudrait mémoriser les adresses IP de tous les sites web.

---

## Étape 1 — Lancer une capture

1. Démarrer une capture
2. Ouvrir un site web que vous **n'avez jamais visité** (ou en navigation privée)
3. Arrêter la capture

---

## Filtre Wireshark

```
dns
```

---

## Travail demandé

1. Quel **nom de domaine** est demandé dans la requête DNS ?
2. Quelle **adresse IP** est retournée ?
3. Combien de **serveurs DNS** répondent ?
4. Quel est **le serveur DNS utilisé par votre machine** ?

---

# Partie 4 — Handshake TCP

## Rappel théorique

Le protocole **TCP** établit une connexion fiable entre deux machines.

Avant toute communication, il réalise un **handshake en 3 étapes**.

### Étapes du handshake

1. **SYN** : le client demande une connexion
2. **SYN‑ACK** : le serveur accepte
3. **ACK** : le client confirme

Cette séquence est appelée :

**TCP Three Way Handshake**.

---

## Étape 1 — Identifier un handshake

Dans Wireshark, utilisez le filtre :

```
tcp
```

Cherchez une séquence de paquets contenant :

```
SYN
SYN ACK
ACK
```

---

## Travail demandé

1. Quelle est **l'adresse IP du serveur distant** ?
2. Quel **port serveur** est utilisé ?
3. Quel paquet contient le **flag SYN** ?
4. Combien de **millisecondes** dure le handshake ?

---

# Partie 5 — Analyse HTTP

## Rappel théorique

HTTP est le protocole utilisé pour transférer les pages web.

Une requête HTTP contient plusieurs informations :

- la méthode (`GET`, `POST`)
- l'URL demandée
- les en‑têtes HTTP

La réponse contient :

- un **code de statut** (200, 404, etc.)
- le contenu de la page

---

## Étape 1 — Accéder à un site HTTP

Ouvrez un site **non sécurisé** :

```
http://example.com
```

---

## Filtre Wireshark

```
http
```

---

## Travail demandé

1. Quelle **méthode HTTP** est utilisée ?
2. Quel est le **code de réponse HTTP** ?
3. Quel est le **User‑Agent** envoyé par votre navigateur ?
4. Quelle est la **ressource demandée** ?

---

# Partie 6 — Analyse TLS (HTTPS)

## Rappel théorique

HTTPS est HTTP **chiffré avec TLS**.

Le protocole TLS permet :

- l'authentification du serveur
- le chiffrement des données
- l'intégrité de la communication

Même si les données sont chiffrées, certaines informations restent visibles.

---

## Étape 1 — Capturer une connexion HTTPS

1. Démarrer une capture
2. Ouvrir un site HTTPS
3. Arrêter la capture

---

## Filtre Wireshark

```
tls
```

---

## Travail demandé

1. Quelle **version de TLS** est utilisée ?
2. Quel est le **nom du serveur dans le champ SNI** ?
3. Quelle **autorité de certification** a signé le certificat ?
4. Quelle est la **durée de validité du certificat** ?

---

# Partie 7 — Analyse complète d'une visite web

Choisissez un site web et observez **toutes les étapes de la connexion**.

Essayez d'identifier dans l'ordre :

1. ARP
2. DNS
3. TCP handshake
4. TLS handshake
5. Requête HTTP

---

## Travail demandé

Expliquez **chronologiquement** ce qu'il se passe lorsque vous accédez à un site web.

Votre réponse doit décrire :

- les protocoles utilisés
- leur rôle
- l'ordre dans lequel ils apparaissent

---

# Conclusion

Dans ce TP vous avez observé les protocoles fondamentaux d'Internet :

- ARP
- DNS
- TCP
- HTTP
- TLS

Ces protocoles sont utilisés **à chaque fois qu'un utilisateur accède à un site web**.

Wireshark est un outil essentiel pour :

- diagnostiquer des problèmes réseau
- analyser des attaques
- comprendre le fonctionnement réel d'Internet.
