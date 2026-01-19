# Correction — TD Diagnostic réseau par couches OSI

## Situation 1 — Absence totale de connectivité

**Couche OSI concernée :**
Couche 1 — Physique

**Justification :**
La couche physique est responsable de la transmission des signaux sur le support matériel (câble, fibre, ondes).
Dans cette situation, le câble Ethernet n’est pas branché : aucun signal ne peut circuler.
Aucune couche supérieure ne peut donc fonctionner tant que la couche physique est défaillante.

---

## Situation 2 — Accès réseau local fonctionnel, routage Internet impossible

**Couche OSI concernée :**
Couche 3 — Réseau

**Justification :**
La couche réseau gère l’adressage IP et le routage entre réseaux.
Ici, la connexion Wi-Fi fonctionne, l’adresse IP est valide et la communication locale est possible.
Le problème apparaît uniquement lors de la communication vers Internet, ce qui indique un dysfonctionnement du routage IP, donc de la couche 3.

---

## Situation 3 — Connectivité IP valide, service web inaccessible

**Couche OSI concernée :**
Couche 7 — Application

**Justification :**
La couche application correspond aux services utilisés par l’utilisateur, comme le web (HTTP/HTTPS).
Le ping fonctionne et la résolution DNS est correcte, ce qui montre que les couches réseau et transport fonctionnent.
L’erreur apparaît uniquement lors de l’accès au service web, ce qui situe le problème au niveau applicatif.

---

## Situation 4 — Problème de port applicatif

**Couche OSI concernée :**
Couche 4 — Transport

**Justification :**
La couche transport gère les ports et l’identification des services.
Le serveur est joignable et le service fonctionne, mais le client utilise un port incorrect.
Le problème concerne donc la gestion des ports, qui relève de la couche 4.

---

## Situation 5 — Mauvaise configuration IP

**Couche OSI concernée :**
Couche 3 — Réseau

**Justification :**
La couche réseau gère l’adressage IP et le découpage en sous-réseaux.
Un masque de sous-réseau incorrect empêche la machine de déterminer correctement quelles adresses sont locales.
Le problème est donc lié à la logique d’adressage IP, et concerne la couche 3.

---

## Synthèse

| Situation | Couche OSI | Raison principale |
|----------|------------|------------------|
| 1 | Couche 1 | Support de transmission absent |
| 2 | Couche 3 | Routage hors réseau local |
| 3 | Couche 7 | Service applicatif web |
| 4 | Couche 4 | Mauvais port utilisé |
| 5 | Couche 3 | Mauvais masque IP |
