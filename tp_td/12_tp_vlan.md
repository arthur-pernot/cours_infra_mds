# TP — Découverte des VLAN avec Cisco Packet Tracer

## Rappels théoriques sur les VLAN (25 min)

### 1 Qu’est-ce qu’un VLAN ?

Un **VLAN (Virtual Local Area Network)** est un **réseau local virtuel**, c’est-à-dire une **segmentation logique** d’un réseau, indépendante de la topologie physique.

* Sans VLAN :

  * Tous les ports d’un switch appartiennent au **même réseau**
  * Tous les postes reçoivent les **broadcasts** (ARP, DHCP…)

* Avec VLAN :

  * Le switch est découpé en **plusieurs réseaux logiques**
  * Chaque VLAN se comporte comme un **switch indépendant**

=> Deux machines dans des VLAN différents :

* ne communiquent pas directement
* même si elles sont branchées sur le même switch

---

### 2 VLAN et domaine de broadcast

Un VLAN correspond exactement à :

> **Un domaine de broadcast**

Cela signifie que :

* Une requête ARP envoyée dans un VLAN
* n’est jamais transmise aux autres VLAN

=> Avantages :

* Moins de trafic inutile
* Meilleures performances
* Réseau plus prévisible

---

### 3 VLAN et sécurité

Les VLAN sont un **mécanisme de sécurité de base** :

* Un poste d’un VLAN ne peut pas :

  * Scanner un autre VLAN
  * Écouter le trafic d’un autre VLAN

Exemples concrets :

* VLAN ADMIN ≠ VLAN ÉTUDIANTS
* VLAN INVITÉS ≠ réseau interne
* VLAN IoT ≠ postes utilisateurs

Attention :

> Les VLAN **ne remplacent pas un pare-feu**, mais ils réduisent fortement la surface d’attaque.

---

### 4 VLAN et adressage IP

En pratique :

* **1 VLAN = 1 réseau IP**

Exemple :

* VLAN 10 → 192.168.10.0 /24
* VLAN 20 → 192.168.20.0 /24

=> Deux VLAN différents :

* appartiennent à **deux réseaux IP différents**
* nécessitent un **équipement de niveau 3** pour communiquer

---

### 5 Types de ports sur un switch

#### Port Access

* Appartient à **un seul VLAN**
* Utilisé pour connecter :

  * PC
  * Imprimante
  * Serveur

=> Le poste connecté **ne voit pas les VLAN**

---

#### Port Trunk

* Transporte **plusieurs VLAN simultanément**
* Utilisé entre :

  * Switch ↔ Switch
  * Switch ↔ Routeur

Les trames sont marquées avec un **tag VLAN (802.1Q)**.

---

### 6 VLAN 802.1Q (notion importante)

* Norme IEEE : **802.1Q**
* Ajoute un champ VLAN dans la trame Ethernet
* Contient :

  * VLAN ID (1 à 4094)

=> Seules les liaisons **trunk** utilisent ce marquage.

---

### 7 Communication inter-VLAN

Par défaut :

* Aucun VLAN ne peut communiquer avec un autre

Pour autoriser la communication, il faut un **routeur**

=> Cette séparation est **volontaire** et **fondamentale**.

---

## Topologie du TP

* 2 switches Cisco 2960
* 4 PC
* Câbles cuivre droit

```
PC1 ──┐          ┌── PC3
      │          │
     SW1 ─────── SW2
      │          │
PC2 ──┘          └── PC4
```

---

## VLAN utilisés

| VLAN | Nom       | Usage                |
| ---- | --------- | -------------------- |
| 10   | ADMIN     | Réseau administratif |
| 20   | ETUDIANTS | Réseau étudiants     |

---

## Étape 1 — Création de la topologie (15 min)

1. Ouvrir **Cisco Packet Tracer**
2. Ajouter :

   * 2 × Switch **2960**
   * 4 × PC
3. Relier :

   * PC1 et PC2 au **Switch 1**
   * PC3 et PC4 au **Switch 2**
   * Switch 1 ↔ Switch 2 (FastEthernet)

---

## Étape 2 — Configuration IP des PC (15 min)

Configurer les adresses IP **manuellement** (Desktop → IP Configuration)

| PC  | VLAN | IP            | Masque        |
| --- | ---- | ------------- | ------------- |
| PC1 | 10   | 192.168.10.10 | 255.255.255.0 |
| PC3 | 10   | 192.168.10.20 | 255.255.255.0 |
| PC2 | 20   | 192.168.20.10 | 255.255.255.0 |
| PC4 | 20   | 192.168.20.20 | 255.255.255.0 |

**Ne pas configurer de passerelle pour l’instant**

---

## Étape 3 — Test sans VLAN (10 min)

1. Lancer un **ping** :

   * PC1 → PC2
   * PC1 → PC3

### Questions

* Quels pings fonctionnent ?
* Pourquoi ?

Tous les PC sont dans le VLAN 1 par défaut.

---

## Étape 4 — Création des VLAN (20 min)

Sur chaque switch :

1. Cliquer sur le switch
2. Onglet **Config** → **VLAN Database**
3. Créer :

   * VLAN 10 nommé `ADMIN`
   * VLAN 20 nommé `ETUDIANTS`

---

## Étape 5 — Affectation des ports aux VLAN (20 min)

Sur chaque switch :

1. Onglet **Config** → **Interface**
2. Sélectionner le port relié à un PC
3. Mode : **Access**
4. VLAN : 10 ou 20 selon le PC

Exemple :

* PC1 et PC3 → VLAN 10
* PC2 et PC4 → VLAN 20

---

## Étape 6 — Tests après VLAN (15 min)

Tester avec **ping** :

| Source | Destination | Résultat attendu |
| ------ | ----------- | ---------------- |
| PC1    | PC3         |   OK             |
| PC2    | PC4         |   OK             |
| PC1    | PC2         |   Échec          |

### Questions

* Pourquoi PC1 et PC2 ne communiquent plus ?
* Que représente un VLAN en termes de réseau IP ?

---

## Étape 7 — Configuration détaillée du lien trunk (25 min)

Jusqu’à présent, chaque switch connaît les VLAN, **mais ils ne sont pas encore transportés entre les switches**.

Par défaut :

* Un lien entre deux switches est en mode **access**
* Il ne transporte **qu’un seul VLAN** (souvent le VLAN 1)

Résultat :

* PC1 (VLAN 10 sur SW1) **ne peut pas joindre** PC3 (VLAN 10 sur SW2)

---

### Objectif de cette étape

Configurer le lien entre **SW1 et SW2** en **mode trunk** afin de :

* transporter **plusieurs VLAN simultanément**
* permettre aux VLAN 10 et 20 de traverser les switches

---

### Configuration du trunk (via interface graphique)

Sur **chaque switch** :

1. Cliquer sur le switch
2. Onglet **Config**
3. Aller dans **Interface**
4. Sélectionner le port connecté à l’autre switch
5. Régler les paramètres suivants :

   * **Port Mode** : `Trunk`
   * **Allowed VLANs** : `All` (par défaut)

Important :

> Le port trunk **ne doit pas être configuré en access**

---

### Tests après configuration du trunk

Relancer les tests **ping** :

| Source | Destination | Résultat attendu |
| ------ | ----------- | ---------------- |
| PC1    | PC3         |   OK             |
| PC2    | PC4         |   OK             |
| PC1    | PC2         |   Échec          |

Interprétation :

* Les VLAN **traversent désormais les switches**
* L’isolement entre VLAN est toujours respecté

---

### Comprendre ce qui se passe réellement

Sur le lien trunk :

* Les trames Ethernet sont **taguées avec un VLAN ID (802.1Q)**
* Le switch destinataire sait :

  * à quel VLAN appartient chaque trame
  * sur quels ports la retransmettre

Sans trunk :

* Le switch **ne sait pas à quel VLAN appartient la trame**
* Elle est donc rejetée ou mal interprétée

---

### Questions de compréhension

1. Pourquoi un port trunk est-il inutile pour connecter un PC ?
2. Pourquoi ne met-on pas tous les ports en trunk par défaut ?
3. Que se passerait-il si le trunk n’autorisait que le VLAN 10 ?

---

## Étape 8 — Observation et compréhension (10 min)

Dans **Simulation Mode** :

* Observer les trames ARP
* Vérifier qu’elles restent dans leur VLAN

---

## Étape 9 — Communication inter-VLAN (réflexion)

Pourquoi PC1 (VLAN 10) ne peut-il pas joindre PC2 (VLAN 20) ?

Indice :

> Il faut un équipement de **niveau 3 (routeur ou switch L3)**

*(Ce point sera traité dans un TP suivant)*

---

## Questions de fin de TP

1. À quoi sert un VLAN ?
2. Que se passe-t-il si deux PC sont dans le même VLAN mais sur deux switches différents ?
3. Quelle est la différence entre un port **access** et un port **trunk** ?
4. Un VLAN correspond-il à un réseau IP ou physique ?

---

## Bonus

* Ajouter un **VLAN 30 – INVITES**
* Ajouter un PC invité
* Vérifier qu’il ne communique avec aucun autre VLAN
