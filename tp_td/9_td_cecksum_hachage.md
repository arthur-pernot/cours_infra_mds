# TD — Checksums et fonctions de hachage

---

## Organisation

- Travail **individuel ou en binôme**
- Durée conseillée : **1h30 à 2h**

---

## Partie 1 — Introduction aux checksums

### 1.1 Définition

Un **checksum** est une valeur numérique calculée à partir d’un ensemble de données.
Cette valeur est transmise ou stockée avec les données afin de **détecter des erreurs accidentelles**.

Le principe est simple :
- L’émetteur calcule un checksum à partir du message
- Le récepteur recalcule le checksum à partir des données reçues
- Si les deux valeurs diffèrent, une **erreur est détectée**

Un checksum permet donc de vérifier **l’intégrité**, mais pas la sécurité.

---

### 1.2 À quoi sert un checksum ?

Les checksums sont utilisés dans de nombreux contextes :
- Réseaux (IP, TCP, UDP)
- Stockage (disques, fichiers)
- Transfert de fichiers
- Mémoire (RAM, EEPROM)

Objectif principal :
- Détecter des erreurs dues au bruit, aux pertes ou à des défauts matériels

---

### 1.3 Ce qu’un checksum ne garantit pas

Un checksum :
- ❌ ne chiffre pas les données
- ❌ ne garantit pas l’identité de l’émetteur
- ❌ ne protège pas contre une modification volontaire

---

## Partie 2 — Checksums simples (calculs manuels)

### 2.1 Checksum par somme modulo N bits

Principe général :

1. Découper le message en blocs de taille fixe (ex. 8 bits)
2. Additionner tous les blocs
3. Conserver uniquement les **N bits de poids faible**
4. Le résultat constitue le checksum

Ce type de checksum est simple mais peu robuste.

---

### Exercice 1 — Checksum par somme (8 bits)

Soit le message suivant :

Bloc 1 : 01010101
Bloc 2 : 00110011
Bloc 3 : 11110000


**Travail demandé :**

1. Convertir chaque bloc en décimal
2. Calculer la somme des valeurs
3. Conserver uniquement les 8 bits de poids faible
4. Donner le checksum final en binaire

---

### 2.2 Checksum avec complément à 1 (Internet checksum)

Ce mécanisme est utilisé historiquement dans les protocoles IP, TCP et UDP.

Principe détaillé :

1. Additionner les blocs de données
2. Si une retenue dépasse la taille autorisée, la reporter sur les bits de poids faible
3. Inverser tous les bits du résultat (complément à 1)

---

### Exercice 2 — Internet checksum (8 bits)

Soit les blocs suivants :

Bloc 1 : 01100110
Bloc 2 : 01001010


**Travail demandé :**

1. Additionner les deux blocs en binaire
2. Gérer la retenue si nécessaire
3. Appliquer le complément à 1
4. Donner le checksum final

---

## Partie 4 — Limites des checksums

Répondez aux questions suivantes :

1. Deux messages différents peuvent-ils produire le même checksum ?
2. Est-il possible de modifier un message sans changer son checksum ?
3. Pourquoi un checksum n’est-il pas suffisant pour garantir la sécurité d’un message ?

---

## Partie 5 — Fonctions de hachage cryptographiques

### 5.1 Définition

Une **fonction de hachage cryptographique** transforme une donnée de taille quelconque en une empreinte de taille fixe.

Propriétés essentielles :

- Déterministe
- Très sensible aux modifications
- Non réversible
- Résistante aux collisions (dans la mesure du possible)

---

### 5.2 Exemples courants

| Algorithme | Statut |
|----------|--------|
| MD5 | Obsolète |
| SHA-1 | Cassé |
| SHA-256 | Recommandé |
| SHA-3 | Moderne |

---

### 5.3 Checksum vs Hash

| Critère | Checksum | Hash |
|------|----------|------|
| Objectif | Détection d’erreurs | Sécurité |
| Calcul | Simple | Complexe |
| Collisions | Fréquentes | Très rares |
| Usage | Réseau, matériel | Mots de passe, signatures |

---

## Partie 6 — Exercices sur les hash

### Exercice 4 — Effet avalanche (détaillé)

L’**effet avalanche** désigne le fait qu’une modification minime de l’entrée entraîne une modification majeure du hash.

**Travail demandé :**

1. Calculer le hash SHA-256 de :

bonjour

2. Calculer le hash SHA-256 de :

Bonjour

3. Calculer le hash SHA-256 de :

bonjour!


Vous pouvez utiliser :
- Un outil en ligne
- La ligne de commande
- Le script PHP de l’exercice suivant

**Questions :**

- Combien de caractères changent entre les hash ?
- Peut-on deviner la relation entre les messages à partir des hash ?
- Pourquoi cette propriété est-elle essentielle pour la sécurité ?

Exercice 6 — Hash et mots de passe

Répondez précisément :
```text
    Pourquoi ne stocke-t-on jamais un mot de passe en clair ?

    Un checksum sert principalement à __________________________

    Un hash cryptographique permet de __________________________

    L’effet avalanche signifie que __________________________
```
