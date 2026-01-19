# TP 1 – Découverte des réseaux et interconnexion de deux réseaux

## Objectifs du TP

À l’issue de ce TP, vous serez capable de :

- Comprendre ce qu’est un réseau informatique
- Attribuer des adresses IP statiques
- Tester la connectivité avec la commande `ping`
- Comprendre pourquoi deux réseaux différents ne communiquent pas directement
- Configurer un routeur pour relier deux réseaux
- Comprendre le rôle de la passerelle par défaut

---
# Instalation de Cisco Packet Tracker
[tuto](https://openclassrooms.com/fr/courses/6944606-concevez-votre-reseau-tcp-ip/7235342-decouvrez-l-outil-de-simulation-packet-tracer)


---

# PARTIE 1 — Mise en place d’un premier réseau local (LAN)

## 1. Topologie à réaliser

Dans Cisco Packet Tracer, vous devez créer :

- 1 switch
- 3 PC

Tous les PC sont reliés au **même switch**.

---

## 2. Plan d’adressage – Réseau 1

Réseau :

192.168.1.0 /24
Masque : 255.255.255.0


| Machine | Adresse IP |
|------|------------|
| PC1 | 192.168.1.10 |
| PC2 | 192.168.1.20 |
| PC3 | 192.168.1.30 |

---

## 3. Travail demandé

1. Ajouter les équipements dans Packet Tracer
2. Relier chaque PC au switch avec un câble cuivre droit
3. Configurer les adresses IP statiques sur chaque PC
4. Tester la connectivité avec la commande `ping`

Exemples :

ping 192.168.1.20
ping 192.168.1.30


---

## 4. Résultat attendu

- Tous les PC doivent se pinguer avec succès
- Les voyants des ports réseau doivent être verts
- Vous devez être capable d’expliquer :
  - ce qu’est un réseau local (LAN)
  - à quoi sert un switch
  - à quoi sert une adresse IP

---

# PARTIE 2 — Ajout d’un second réseau et d’un routeur

## 5. Nouveau contexte

L’entreprise ouvre un **second bureau** avec son propre réseau local.

Les deux réseaux doivent pouvoir communiquer.

---

## 6. Nouvelle topologie à réaliser

Vous devez ajouter :

- 1 routeur
- 1 second switch
- 2 nouveaux PC

---

## 7. Plan d’adressage – Réseau 2

Réseau :

192.168.2.0 /24
Masque : 255.255.255.0


| Machine | Adresse IP |
|------|------------|
| PC4 | 192.168.2.10 |
| PC5 | 192.168.2.20 |

---

## 8. Test volontairement bloquant (important)

Avant de configurer le routeur :

- Essayez de pinguer un PC du réseau 2 depuis le réseau 1

Exemple :

ping 192.168.2.10


### Observation attendue
- Le ping échoue

### Question à se poser
> Pourquoi deux réseaux différents ne peuvent-ils pas communiquer directement ?

---

## 9. Ajout du routeur

Le routeur va permettre de **relier les deux réseaux**.

Reliez :
- une interface du routeur au switch du réseau 1
- une interface du routeur au switch du réseau 2

---

## 10. Configuration du routeur

Attribuez les adresses IP suivantes au routeur :

| Interface du routeur | Adresse IP |
|--------------------|------------|
| Vers réseau 1 | 192.168.1.1 |
| Vers réseau 2 | 192.168.2.1 |

---

## 11. Configuration des passerelles par défaut

Sur chaque PC, configurez la **passerelle par défaut** :

| Réseau | Passerelle |
|------|------------|
| Réseau 1 | 192.168.1.1 |
| Réseau 2 | 192.168.2.1 |

> La passerelle est l’adresse du routeur vers lequel on envoie les paquets destinés à un autre réseau.

---

## 12. Tests finaux

Effectuez les tests suivants :

- PC1 → PC4
- PC5 → PC2

Résultat attendu :
- les pings réussissent

