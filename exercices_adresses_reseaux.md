# Exercices – Réseaux, masques et adresses IP

> Objectif : vérifier la compréhension des notions de **réseau**, **masque de sous-réseau**, **adresse réseau**, **adresse de broadcast** et **adresses assignables**.
> Niveau : débutant → intermédiaire (cours d’infrastructure réseau).

---

## Exercice 1 – Même réseau ou non ?

Masque : **255.255.255.0**

Adresses IP :
- 192.168.1.12
- 192.168.1.200
- 192.168.2.5

**Questions :**
1. Quelles adresses sont dans le même réseau ?
2. Quelle est l’adresse réseau ?
3. Quelle est l’adresse de broadcast ?

---

## Exercice 2 – Calcul d’un réseau (/9)

Masque : **255.128.0.0**

Adresse IP : **10.2.45.78**

**Questions :**
1. Quel est le réseau associé ?
2. Quelle est la plage d’adresses IP possibles dans ce réseau ?
3. Combien d’adresses hôtes théoriques ce réseau contient-il ?

---

## Exercice 3 – Adresse valide ou non ?

Masque : **255.255.255.0**

Pour chaque adresse, indiquer si elle est **assignable à un ordinateur** :

- 192.168.1.0
- 192.168.1.1
- 192.168.1.254
- 192.168.1.255

---

## Exercice 4 – Identifier le masque

On souhaite créer un réseau pouvant accueillir **50 ordinateurs**.

**Questions :**
1. Quel est le masque minimum possible ?
2. Combien d’adresses seront inutilisées dans ce réseau ?

---

## Exercice 5 – Comparaison de réseaux

Masque : **255.255.254.0**

Adresses IP :
- 172.16.10.12
- 172.16.11.200
- 172.16.12.1

**Questions :**
1. Quelles adresses sont dans le même réseau ?
2. Quelle est la taille d’un sous-réseau avec ce masque ?

---

## Exercice 6 – Broadcast et réseau

Masque : **255.255.255.192**

Adresse IP : **192.168.10.130**

**Questions :**
1. Quelle est l’adresse réseau ?
2. Quelle est l’adresse de broadcast ?
3. Combien d’hôtes peuvent être connectés dans ce réseau ?

---

## Exercice 7 – Calcul rapide (QCM)

Masque : **255.255.255.0**

Quelle affirmation est correcte ?

A. 192.168.1.255 est une adresse assignable
B. 192.168.1.0 est une adresse réseau
C. 192.168.1.1 est une adresse de broadcast
D. 192.168.1.254 est une adresse réseau

---

## Exercice 8 – Réseau privé ou public ?

Indiquer si les adresses suivantes sont **privées ou publiques** :

- 10.12.4.8
- 172.20.5.1
- 172.40.1.2
- 192.168.100.10
- 8.8.8.8

---

## Exercice 9 – Même machine, deux IP ?

Un ordinateur possède :
- IP 1 : 192.168.1.10 /24
- IP 2 : 192.168.2.10 /24

**Questions :**
1. Ces deux adresses sont-elles dans le même réseau ?
2. L’ordinateur peut-il communiquer directement avec les deux réseaux sans routeur ?

---

## Exercice 10 – Raisonnement complet

Masque : **255.255.255.128**

Adresses IP :
- 192.168.1.12
- 192.168.1.126
- 192.168.1.129
- 192.168.1.254

**Questions :**
1. Combien de réseaux différents y a-t-il ?
2. Quelles adresses sont dans le même réseau ?
3. Quelles adresses sont des adresses réseau ou broadcast ?

---

> Astuce méthodologique :
> Toujours commencer par identifier **le pas du masque**, puis déterminer
> **réseau → hôtes → broadcast** avant de comparer les IP.
