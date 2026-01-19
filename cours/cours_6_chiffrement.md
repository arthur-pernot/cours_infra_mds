# Cours — Construction des clés en chiffrement asymétrique (RSA)

## Objectifs pédagogiques

À l’issue de ce cours, l’étudiant doit être capable de :

- Comprendre l’origine mathématique des clés RSA
- Expliquer le rôle de **n**, **e** et **d**
- Savoir **comment** on choisit ces valeurs
- Comprendre **pourquoi** ces choix sont contraints
- Faire le lien entre la théorie et l’usage réel (TLS, HTTPS)

> ATTENTION : Les exemples utilisent de **petits nombres** pour des raisons pédagogiques.
> Ils ne sont **pas sécurisés** dans un contexte réel.

---

## 1. Rappel : principe du chiffrement asymétrique

Le chiffrement asymétrique repose sur une **paire de clés** :

- **clé publique** : peut être diffusée
- **clé privée** : doit rester secrète

Propriété fondamentale :
> Ce qui est chiffré avec la clé publique ne peut être déchiffré qu’avec la clé privée correspondante.

Dans RSA, cette propriété repose sur des **propriétés arithmétiques des nombres premiers**.

---

## 2. Vue d’ensemble des clés RSA

Dans RSA, les clés sont définies par trois valeurs principales :

| Symbole | Nom | Rôle |
|------|----|-----|
| \( n \) | module | base de tous les calculs |
| \( e \) | exposant public | sert à chiffrer |
| \( d \) | exposant privé | sert à déchiffrer |

Formules utilisées :

- **Chiffrement** :
\[
C = M^e \mod n
\]

- **Déchiffrement** :
\[
M = C^d \mod n
\]

---

## 3. Étape 1 — Choisir deux nombres premiers \( p \) et \( q \)

### Règles

- \( p \) et \( q \) doivent être **premiers**
- Ils doivent être :
  - distincts
  - suffisamment grands (en pratique)

### Exemple pédagogique

p = 3
q = 11


> En pratique, ces nombres font plusieurs centaines de chiffres.

---

## 4. Étape 2 — Calculer le module \( n \)

Formule :
\[
n = p \times q
\]

Exemple :
\[
n = 3 \times 11 = 33
\]

### Rôle de \( n \)

- Utilisé dans toutes les opérations modulo
- Fait partie de la **clé publique**
- Sa sécurité repose sur la difficulté de retrouver \( p \) et \( q \)

---

## 5. Étape 3 — Calculer l’indicatrice d’Euler \( phi(n) \)

Formule (dans le cas RSA) :
\[
phi(n) = (p - 1)(q - 1)
\]

Exemple :
\[
phi(33) = (3 - 1)(11 - 1) = 2 \times 10 = 20
\]

### Rôle de \( phi(n) \)

- Sert à déterminer les exposants \( e \) et \( d \)
- **Ne doit jamais être public**
- Dépend directement de \( p \) et \( q \)

---

## 6. Étape 4 — Choisir l’exposant public \( e \)

### Contraintes sur \( e \)

\( e \) doit vérifier :

1. \( 1 < e < phi(n) \)
2. \( \gcd(e, phi(n)) = 1 \)
   (e et \( phi(n) \) doivent être premiers entre eux)

### Exemple

Avec :
\[
phi(n) = 20
\]

On teste :

| \( e \) | \( \gcd(e, 20) \) | Valide ? |
|------|------------------|---------|
| 2 | 2 | ❌ |
| 3 | 1 | ✅ |
| 4 | 4 | ❌ |
| 5 | 5 | ❌ |
| 7 | 1 | ✅ |

Choix pédagogique :

e = 3


### Remarque

En pratique, on utilise souvent :

e = 65537

pour des raisons de performance et de sécurité.

---

## 7. Étape 5 — Calculer l’exposant privé \( d \)

### Définition

\( d \) est l’**inverse modulaire** de \( e \) modulo \( phi(n) \).

Formellement :
\[
d \times e \equiv 1 \ (\text{mod } phi(n))
\]

### Exemple

On cherche \( d \) tel que :
\[
3d \equiv 1 \ (\text{mod } 20)
\]

Essais :

| \( d \) | \( 3 \times d \) | mod 20 |
|------|---------------|--------|
| 1 | 3 | 3 |
| 3 | 9 | 9 |
| 7 | 21 | **1** ✅ |

Donc :

d = 7


---

## 8. Résumé des clés obtenues

### Clé publique

(e = 3, n = 33)


### Clé privée

d = 7


---

## 9. Pourquoi cette construction fonctionne ?

Propriété mathématique fondamentale :
\[
(M^e)^d \equiv M \ (\text{mod } n)
\]

Cette propriété est garantie par :
- le choix de \( e \) et \( d \)
- les propriétés de l’indicatrice d’Euler
- la factorisation \( n = p \times q \)

---

## 10. Sécurité : ce qui est facile et ce qui est difficile

| Problème | Facile ? |
|-------|---------|
| Multiplier deux grands nombres premiers | ✅ |
| Calculer \( phi(n) \) avec \( p \) et \( q \) | ✅ |
| Factoriser \( n \) sans \( p \) et \( q \) | ❌ |

=> Toute la sécurité de RSA repose sur ce dernier point.

---

## 11. Lien avec TLS / HTTPS

Dans TLS :

- RSA (ou ECDH) est utilisé pour :
  - authentifier le serveur
  - échanger un secret
- Les données applicatives sont ensuite chiffrées en **symétrique**

> RSA sert à **démarrer une communication sécurisée**,
> pas à chiffrer les données en continu.

---

## À retenir

- \( n \) est le produit de deux nombres premiers
- \( e \) est choisi sous contrainte mathématique
- \( d \) est calculé, jamais choisi au hasard
- La clé privée dépend directement de \( p \) et \( q \)
- RSA est un **outil de négociation**, pas un tunnel de données
