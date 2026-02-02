# TP — Les ACL (Access Control Lists)

## Contexte du TP

Vous êtes administrateur réseau dans une petite entreprise.

Le réseau interne est composé de plusieurs postes clients et d’un serveur. L’accès à certains services doit être **contrôlé** à l’aide de **listes de contrôle d’accès (ACL)**.

Vous allez :

1. Mettre en place une topologie simple
2. Vérifier le fonctionnement sans ACL
3. Appliquer des ACL
4. Observer les effets sur le trafic
5. Diagnostiquer des erreurs volontaires

---

## Partie 1 — Mise en place de la topologie

### 1.1 Topologie à réaliser

* 1 routeur (R1)
* 1 switch (S1)
* 3 PC :

  * PC1 (Utilisateur)
  * PC2 (Utilisateur)
  * PC3 (Administrateur)
* 1 serveur (Server1)

### 1.2 Plan d’adressage

| Équipement | Adresse IP   | Masque        | Passerelle  |
| ---------- | ------------ | ------------- | ----------- |
| PC1        | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| PC2        | 192.168.1.20 | 255.255.255.0 | 192.168.1.1 |
| PC3        | 192.168.1.30 | 255.255.255.0 | 192.168.1.1 |
| Server1    | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |

Le routeur R1 possède deux interfaces :

* G0/0 : 192.168.1.1
* G0/1 : 192.168.2.1

---

## Partie 2 — Vérification du fonctionnement sans ACL

1. Depuis chaque PC, tester :

   * `ping` vers le serveur
   * Accès HTTP au serveur (si activé)

### Questions

1. Tous les PC peuvent-ils joindre le serveur ?
2. À quelle couche OSI se fait actuellement le contrôle du trafic ?

---

## Partie 3 — Introduction aux ACL standard

### 3.1 Principe

Une **ACL standard** filtre uniquement sur :

* L’**adresse IP source**

Elle agit à la **couche 3 (IP)**.

---

### 3.2 Objectif

* Interdire **PC2** d’accéder au serveur
* Autoriser tous les autres postes

---

### 3.3 Configuration sur le routeur

Sur R1 :

```text
access-list 10 deny 192.168.1.20
access-list 10 permit any
```

Appliquer l’ACL sur l’interface vers le serveur :

```text
interface g0/1
ip access-group 10 in
```

---

### 3.4 Tests

1. Tester le `ping` vers le serveur depuis :

   * PC1
   * PC2
   * PC3

### Questions

1. Quel PC est bloqué ?
2. Pourquoi l’ACL est-elle appliquée **au plus près de la destination** ?
3. Que se passe-t-il si on supprime la ligne `permit any` ?

---

## Partie 4 — ACL étendues

### 4.1 Principe

Une **ACL étendue** permet de filtrer sur :

* IP source
* IP destination
* Protocole (ICMP, TCP, UDP)
* Ports

Elle agit sur les **couches 3 et 4**.

---

### 4.2 Objectif

* Autoriser tous les PC à joindre le serveur
* Interdire **l’accès HTTP (port 80)** depuis PC1
* Autoriser le reste

---

### 4.3 Configuration

```text
access-list 100 deny tcp host 192.168.1.10 host 192.168.2.10 eq 80
access-list 100 permit ip any any
```

Application sur l’interface côté LAN :

```text
interface g0/0
ip access-group 100 in
```

---

### 4.4 Tests

1. Depuis PC1 :

   * `ping` vers le serveur
   * Accès HTTP
2. Depuis PC2 et PC3 :

   * Accès HTTP

### Questions

1. Pourquoi le ping fonctionne-t-il encore ?
2. À quelle couche OSI agit cette ACL ?

---

## Partie 5 — Erreurs courantes et diagnostic

### Situation 1 — Plus rien ne fonctionne

On remplace l’ACL par :

```text
access-list 100 deny ip any any
```

### Questions

1. Quel est le symptôme observé ?
2. Pourquoi ?
3. Quelle règle minimale faut-il ajouter pour rétablir le réseau ?

---

## Partie 6 — Synthèse

### À retenir

* Une ACL est **stateless**
* Les règles sont lues **de haut en bas**
* Il existe toujours un **deny implicite**
* ACL standard → simple
* ACL étendue → précise

### Question finale

> Dans quel cas préférer un **pare-feu stateful** à une ACL ?
