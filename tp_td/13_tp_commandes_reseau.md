# TP — Diagnostic réseau (Windows & Linux)

# Étape 1 — Exploration complète de la configuration réseau

### Objectif

Comprendre comment la machine est connectée au réseau et identifier ses paramètres essentiels.

---

### 1.1 Exécuter les commandes de base

#### Windows

```bash
ipconfig /all
route print
arp -a
netstat -an
```

#### Linux

```bash
ip addr
ip route
ip neigh
ss -tuln
```

---

### 1.2 Analyse et interprétation

À partir des commandes précédentes, **compléter le tableau suivant** :

| Élément                     | Valeur relevée | Commande utilisée |
| --------------------------- | -------------- | ----------------- |
| Interface réseau principale |                |                   |
| Adresse IP                  |                |                   |
| Masque de sous-réseau       |                |                   |
| Passerelle par défaut       |                |                   |
| Serveur(s) DNS              |                |                   |

---

### 1.3 Questions d’analyse

1. Quelle est la différence entre une interface Ethernet et Wi-Fi ?
2. Que signifie une adresse IP commençant par `169.254` ?
3. Pourquoi peut-on avoir plusieurs interfaces réseau actives ?
4. À quelle couche OSI appartient :

   * l’adresse IP ?
   * l’adresse MAC ?
   * la passerelle ?

---

# Étape 2 — Diagnostic de connectivité avec `ping`

### Objectif

Tester la connectivité **du local vers Internet**, en isolant chaque couche du problème.

---

### 2.1 Tests progressifs

Exécuter les commandes suivantes **dans l’ordre** et noter le résultat (OK / KO / latence) :

```bash
ping 127.0.0.1
ping <votre_IP>
ping <IP_passerelle>
ping 8.8.8.8
ping google.com
```

---

### 2.2 Tableau de résultats

| Cible      | Résultat | Interprétation | Couche OSI |
| ---------- | -------- | -------------- | ---------- |
| 127.0.0.1  |          |                |            |
| IP locale  |          |                |            |
| Passerelle |          |                |            |
| 8.8.8.8    |          |                |            |
| google.com |          |                |            |

---

### 2.3 Analyse de la latence

```bash
# Linux
ping -c 10 google.com

# Windows
ping -n 10 google.com
```

Questions :

1. Quelle est la latence moyenne ?
2. À partir de quelle valeur considère-t-on une latence élevée ?
3. La latence est-elle stable ? Pourquoi ?

---

# Étape 3 — Analyse des routes réseau

### Objectif

Comprendre le chemin emprunté par les paquets vers Internet.

---

### 3.1 Tracer une route

#### Windows

```bash
tracert google.com
tracert 8.8.8.8
```

#### Linux

```bash
traceroute google.com
traceroute -I google.com
```

---

### 3.2 Analyse

Pour chaque saut (hop) :

* Noter l’IP
* Noter le nom (si présent)
* Noter le temps de réponse

Questions :

1. À quoi correspond le premier saut ?
2. Pourquoi certains sauts affichent `* * *` ?
3. Pourquoi le chemin peut-il changer entre deux tests ?

---

# Étape 4 — Analyse des connexions et ports

### Objectif

Observer les services réseau actifs sur la machine.

---

### 4.1 Commandes

#### Windows

```bash
netstat -an | find "ESTABLISHED"
netstat -b
```

#### Linux

```bash
ss -tuln
ss -tup
netstat -tulpn
```

---

### 4.2 Travail demandé

1. Identifier :

   * 2 ports en écoute
   * 2 connexions établies
2. Associer les ports à des services connus (HTTP, HTTPS, SSH…)
3. Expliquer la différence entre :

   * un port en écoute
   * une connexion établie

---

# Étape 5 — Étude de la table ARP

### Objectif

Comprendre la résolution IP → MAC dans un réseau local.

---

### 5.1 Observation

```bash
arp -a
```

Puis :

```bash
ping <IP_passerelle>
arp -a
```

---

### 5.2 Manipulation

#### Vider la table ARP

* Windows :

```bash
netsh interface ip delete arpcache
```

* Linux :

```bash
sudo ip neigh flush all
```

Questions :

1. Pourquoi la table ARP se vide-t-elle automatiquement ?
2. À quelle couche OSI appartient ARP ?
3. Pourquoi ARP est indispensable avant tout échange IP ?

---
# Étape 6 — Scénarios guidés de diagnostic réseau

Dans cette partie, vous devez **raisonner comme un administrateur réseau**.

Pour chaque situation :

1. Analysez les symptômes
2. Identifiez **à quelle couche du réseau le problème peut se situer**
3. Indiquez **les commandes de diagnostic que vous utiliseriez**
4. Proposez **au moins une cause probable**
5. Proposez **une solution**

---

# Scénario 1 — Accès Internet impossible

### Situation

Un utilisateur signale qu'il n'arrive pas à accéder à Internet.

Les tests suivants ont été effectués :

```
ping 127.0.0.1  → OK
ping 8.8.8.8    → KO
```

### Aide au raisonnement

- Le test `127.0.0.1` vérifie le fonctionnement de la pile réseau locale.
- Le test vers `8.8.8.8` vérifie l'accès à Internet **sans utiliser DNS**.

### Travail demandé

1. Quelle partie du réseau semble fonctionner ?
2. Quelle couche OSI peut être concernée ?
3. Quelles commandes utiliser pour poursuivre le diagnostic ?
4. Proposer **2 causes possibles**.
5. Proposer **une solution**.

---

# Scénario 2 — Accès par IP mais pas par nom

### Situation

Un utilisateur peut contacter certains serveurs Internet, mais certains sites ne fonctionnent pas.

Résultats observés :

```
ping 8.8.8.8     → OK
ping google.com  → KO
```

### Aide au raisonnement

- La communication avec une **adresse IP fonctionne**.
- La communication avec un **nom de domaine échoue**.

### Travail demandé

1. Quel service réseau pourrait être impliqué ?
2. Quelle commande permet de tester la résolution de nom ?
3. Comment vérifier la configuration DNS de la machine ?
4. Proposer une solution.

---

# Scénario 3 — Deux ordinateurs ne communiquent pas

### Situation

Deux ordinateurs sont connectés au **même switch** dans un réseau local.

Informations connues :

- Les deux machines ont une adresse IP dans le même réseau
- Impossible de se pinguer

### Aide au raisonnement

Dans un réseau local, plusieurs éléments peuvent empêcher la communication :

- configuration IP
- masque réseau
- pare-feu
- segmentation réseau

### Travail demandé

1. Quelles vérifications effectuer **du niveau physique jusqu'au niveau réseau** ?
2. Quelles commandes utiliser ?
3. Proposer **au moins 3 causes possibles**.

---

# Scénario 4 — Temps de réponse très élevé

### Situation

Un utilisateur se plaint que la connexion Internet est très lente.

Observations :

- les pings fonctionnent
- la latence est élevée
- traceroute montre des temps de réponse importants

### Aide au raisonnement

Un ralentissement peut venir :

- du réseau local
- d'un routeur intermédiaire
- de la distance du serveur

### Travail demandé

1. Quelle commande permet d'identifier où se situe le ralentissement ?
2. Que faut-il observer dans les résultats ?
3. Proposer **2 causes possibles**.

---

# Scénario 5 — Adresse IP inhabituelle

### Situation

Après connexion au réseau, un ordinateur obtient l'adresse suivante :

```
169.254.32.12
```

Les symptômes :

- impossible de joindre la passerelle
- pas d'accès Internet

### Aide au raisonnement

Certaines adresses IP sont attribuées automatiquement par le système lorsqu'il ne reçoit pas de configuration réseau.

### Travail demandé

1. D'où peut venir cette adresse IP ?
2. Quel service réseau est probablement impliqué ?
3. Quelles commandes permettent de renouveler la configuration réseau ?
4. Proposer une procédure de résolution.

---

# Scénario 6 — Communication locale possible mais pas Internet

### Situation

Les tests suivants ont été réalisés :

```
ping IP_locale      → OK
ping passerelle     → OK
ping 8.8.8.8        → KO
```

### Aide au raisonnement

- la communication locale fonctionne
- la machine atteint le routeur

### Travail demandé

1. Quel paramètre réseau peut empêcher l'accès à Internet ?
2. Quelle commande permet d'afficher ce paramètre ?
3. Pourquoi ce paramètre est-il indispensable pour accéder à Internet ?

---

# Scénario 7 — Connexion instable sur le réseau local

### Situation

Un ordinateur perd régulièrement la connexion au réseau.

Symptômes observés :

- pertes de paquets
- connexion intermittente
- entrées ARP qui changent fréquemment

### Aide au raisonnement

Sur un réseau local, certaines anomalies peuvent provoquer ces comportements.

### Travail demandé

1. Quel type de problème peut provoquer ces symptômes ?
2. Quelle commande permet d'observer les correspondances IP → MAC ?
3. Proposer une méthode pour résoudre le problème.

---

# Scénario 8 — Accès aux sites très lent

### Situation

Un utilisateur remarque que :

- Internet fonctionne
- mais les pages web mettent longtemps à commencer à charger

### Aide au raisonnement

Le problème peut se produire **avant même l'envoi des paquets vers le serveur web**.

### Travail demandé

1. Quel service réseau intervient avant l'accès à un site web ?
2. Comment tester ce service ?
3. Proposer une amélioration possible.

---

# Scénario 9 — Machines sur le même switch mais communication impossible

### Situation

Deux ordinateurs sont physiquement connectés au **même switch**.

Observations :

- les deux machines ont une adresse IP valide
- elles sont censées être dans le même réseau
- pourtant le ping échoue

### Aide au raisonnement

Dans un réseau local, deux machines reliées au même switch devraient normalement pouvoir communiquer directement.

Si ce n'est pas le cas, plusieurs éléments peuvent expliquer le problème :

- mauvaise configuration IP
- masque de sous-réseau incorrect
- segmentation logique du réseau
- problème ARP

Il faut donc vérifier **si les machines sont réellement dans le même réseau logique**, même si elles sont connectées au même équipement physique.

---

### Travail demandé

1. Quelles vérifications effectuer sur les deux machines pour confirmer qu’elles sont dans le même réseau ?
2. Quelles commandes permettent d’observer :
   - l’adresse IP
   - le masque réseau
   - la table ARP
3. Quelles informations dans ces commandes pourraient expliquer l’impossibilité de communiquer ?
4. Proposer **au moins deux causes possibles** à ce problème.
5. Proposer **une solution pour rétablir la communication**.

---
