# TP Réseaux — Conception d’une infrastructure pour une PME

**Outil : Cisco Packet Tracer**\
**Durée cible : 3 à 4 heures**\
**Niveau : Bachelor 1 — Infrastructure Réseau**

---

# Objectifs du TP

Dans ce TP vous allez concevoir l'infrastructure réseau complète d'une petite entreprise.

Vous devrez :

- construire une topologie réseau
- segmenter le réseau avec des VLAN
- mettre en place le routage entre les réseaux
- configurer DHCP
- permettre l'accès à Internet
- sécuriser une partie du réseau

Important : **à chaque étape vous devrez expliquer pourquoi la configuration est nécessaire.**

---

# Contexte

L'entreprise **TechNova** possède environ **25 employés** répartis dans plusieurs services.

L'objectif est de créer une infrastructure réseau **organisée, sécurisée et évolutive**.

Services présents dans l'entreprise :

| Service       | Nombre de postes |
| ------------- | ---------------- |
| Direction     | 3                |
| Administratif | 7                |
| Technique     | 10               |
| Invités       | variable         |

L'entreprise possède également :

- un serveur interne
- une connexion Internet

---

# Architecture à construire

Pour simplifier le TP, **un seul type de switch sera utilisé**.

Vous devez utiliser :

- 1 routeur (connexion Internet + routage entre VLAN)
- 2 switches (modèle 2960)
- 1 serveur
- 25 PC

Tout doit être réalisé dans **Cisco Packet Tracer**.

---

# Plan d'adressage

| VLAN | Service       | Réseau           |
| ---- | ------------- | ---------------- |
| 10   | Direction     | 192.168.10.0 /24 |
| 20   | Administratif | 192.168.20.0 /24 |
| 30   | Technique     | 192.168.30.0 /24 |
| 40   | Invités       | 192.168.40.0 /24 |

Règles :

- La passerelle de chaque réseau doit être **.1**
- Le serveur doit avoir l'adresse **192.168.20.10**

---

# Étape 1 — Construction de la topologie

## 1.1 Création du projet

1. Ouvrir **Cisco Packet Tracer**
2. Cliquer sur **File → New**
3. Sauvegarder le projet sous le nom :

```
TP_Reseau_TechNova.pkt
```

---

## 1.2 Ajout des équipements

Dans la barre d'équipements en bas de Packet Tracer :

### Routeur

1. Cliquer sur **Network Devices**
2. Sélectionner **Routers**
3. Choisir un modèle (ex : **2911**)
4. Glisser le routeur dans l'espace de travail

### Switches

1. Cliquer sur **Switches**
2. Choisir **2960**
3. Ajouter **2 switches identiques**

### Serveur

1. Cliquer sur **End Devices**
2. Ajouter **1 Server**

### PC

Ajouter environ **25 PC**.

---

## 1.3 Connexion des équipements

Utiliser l'outil **Connections (icône éclair)**.

Choisir **Copper Straight-Through**.

Réaliser les connexions suivantes :

- Routeur ↔ Switch 1
- Switch 1 ↔ Switch 2
- PC ↔ Switch
- Serveur ↔ Switch

Topologie minimale :

```
Internet
   |
Routeur
   |
Switch
   |
Switch
```

Puis connecter les PC aux switches.

---

## Questions

Expliquez :

1. Pourquoi utilise‑t‑on **plusieurs switches** dans une entreprise ?
2. Quel est le rôle du **routeur** dans ce réseau ?

---

# Étape 2 — Création des VLAN

## 2.1 Création des VLAN via l'interface graphique

1. Cliquer sur un **switch**
2. Aller dans l'onglet **Config**
3. Sélectionner **VLAN Database**

Créer les VLAN suivants :

| VLAN | Nom           |
| ---- | ------------- |
| 10   | Direction     |
| 20   | Administratif |
| 30   | Technique     |
| 40   | Invités       |

Répéter l'opération sur les deux switches.

---

## 2.2 Affectation des ports aux VLAN

Toujours dans l'onglet **Config** :

1. Sélectionner une **interface FastEthernet**
2. Dans **VLAN**, choisir le VLAN correspondant

Exemple :

| PC           | VLAN    |
| ------------ | ------- |
| PC Direction | VLAN 10 |
| PC Admin     | VLAN 20 |
| PC Technique | VLAN 30 |

---

## 2.3 Configuration du lien trunk

Le lien entre les deux switches doit transporter **plusieurs VLAN**.

Pour cela :

1. Cliquer sur le port reliant les switches
2. Dans **Port Mode** choisir **Trunk**

---

## Test

Configurer manuellement deux PC du **même VLAN** avec une adresse IP.

Exemple :

```
PC1 : 192.168.10.21
PC2 : 192.168.10.22
```

Tester :

```
ping 192.168.10.22
```

---

## Questions

1. Pourquoi créer plusieurs VLAN dans une entreprise ?
2. Quelle différence entre **VLAN** et **réseau physique séparé** ?
3. Pourquoi le lien entre switches doit-il être en **trunk** ?

---

# Étape 3 — Routage inter‑VLAN

Les VLAN sont **isolés par défaut**.

Pour permettre la communication entre VLAN, le **routeur sera utilisé**.

Cette technique s'appelle **Router-on-a-Stick**.

Le routeur va créer **une sous-interface pour chaque VLAN**.

---

## 3.1 Configuration des sous‑interfaces

1. Cliquer sur le **routeur**
2. Aller dans l'onglet **Config**
3. Sélectionner l'interface reliée au switch

Créer des sous‑interfaces pour chaque VLAN.

| Sous‑interface | Adresse      |
| -------------- | ------------ |
| VLAN 10        | 192.168.10.1 |
| VLAN 20        | 192.168.20.1 |
| VLAN 30        | 192.168.30.1 |
| VLAN 40        | 192.168.40.1 |

Masque :

```
255.255.255.0
```

Ces adresses seront les **passerelles** des réseaux.

---

## Test

Un PC du VLAN 10 doit pouvoir **ping** un PC du VLAN 20.

---

## Questions

1. Pourquoi les VLAN ne peuvent-ils pas communiquer entre eux par défaut ?
2. Quel est le rôle d'une **passerelle** ?
3. Pourquoi faut-il créer une **sous-interface par VLAN** sur le routeur ?

---

# Étape 4 — Mise en place du DHCP

Le serveur va distribuer automatiquement les adresses IP.

---

## 4.1 Configuration du serveur

1. Cliquer sur **Server**
2. Aller dans **Services**
3. Cliquer sur **DHCP**

Créer un pool pour chaque VLAN.

Exemple VLAN 10 :

| Paramètre       | Valeur        |
| --------------- | ------------- |
| Pool name       | VLAN10        |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 8.8.8.8       |
| Start IP        | 192.168.10.21 |
| Subnet Mask     | 255.255.255.0 |

Répéter pour chaque VLAN.

---

## 4.2 Configuration des PC

1. Cliquer sur un **PC**
2. Aller dans **Desktop**
3. Cliquer sur **IP Configuration**
4. Sélectionner **DHCP**

Le PC doit recevoir automatiquement :

- une adresse IP
- un masque
- une passerelle

---

## Questions

1. Pourquoi utilise-t-on DHCP dans une entreprise ?
2. Que se passerait-il si les IP étaient configurées manuellement ?

---

# Étape 5 — Accès Internet et NAT

Dans un réseau d'entreprise, les machines utilisent généralement des **adresses IP privées** (ex : 192.168.x.x).

Ces adresses **ne sont pas routables sur Internet**. Pour permettre aux machines internes d'accéder à Internet, le routeur utilise **NAT (Network Address Translation)**.

Le principe :

- les PC utilisent des **adresses privées**
- le routeur possède **une adresse côté Internet**
- lorsqu'un paquet sort vers Internet, le routeur **remplace l'adresse privée par sa propre adresse**

Dans ce TP nous utiliserons **PAT (NAT Overload)**, ce qui permet à **plusieurs machines internes de partager une seule adresse IP publique**.

---

## 5.1 Configuration du NAT

1. Cliquer sur le **routeur**
2. Aller dans l'onglet **Config**

### Interface Internet

Configurer l'interface connectée à Internet avec une adresse IP.

Exemple :

```
209.165.200.225
```

Masque :

```
255.255.255.248
```

Définir cette interface comme **NAT Outside**.

### Interface vers le réseau interne

Sélectionner l'interface connectée au switch.

Activer **NAT Inside**.

### Activation du PAT

Dans la section **NAT** :

1. Ajouter une règle NAT
2. Sélectionner le réseau interne :

```
192.168.0.0 /16
```

3. Utiliser l'interface Internet comme adresse de traduction
4. Activer **Overload (PAT)**

Cela permettra à **tous les VLAN internes d'accéder à Internet**.

---

## 5.2 Tests de connectivité

Depuis un PC :

1. Ouvrir **Desktop → Command Prompt**
2. Vérifier l'adresse IP :

```
ipconfig
```

3. Tester l'accès Internet :

```
ping 8.8.8.8
```

Si le ping fonctionne, cela signifie que :

- le routage fonctionne
- le NAT fonctionne
- le PC peut accéder à Internet

---

## Questions

1. Pourquoi est-il important de tester le réseau **progressivement** ?
2. Pourquoi commence-t-on souvent par tester la communication **dans le même VLAN** ?



---

# Étape 6 — Sécurité réseau

Objectif : empêcher les invités d'accéder au réseau interne.

Règle :

- VLAN 40 → Internet uniquement
- accès au serveur interdit

---

## 6.1 Création d'une ACL

1. Cliquer sur le **routeur**
2. Aller dans **Config**
3. Sélectionner **Access Lists**

Créer une règle qui :

- bloque l'accès au réseau interne
- autorise l'accès Internet

---

## Questions

1. Quel est l'objectif d'une **ACL** ?
2. Pourquoi isoler le réseau invité ?

---

# Étape 7 — Tests et diagnostic

Testez :

- communication dans un VLAN
- communication entre VLAN
- accès Internet

---

## Outils utiles

Dans chaque PC :

Desktop → Command Prompt

Commandes :

```
ping
ipconfig
```

---

## Questions

1. Quelle commande permet de tester la connectivité réseau ?
2. Pourquoi tester chaque couche du réseau séparément ?

#
