# TD — Diagnostic réseau par couches OSI

## Consignes

Pour chaque situation :

1. Identifiez la **couche OSI principalement concernée**
2. Justifiez votre choix en vous appuyant sur :

   * le **rôle fonctionnel** de la couche
   * les **symptômes observés**
   * ce qui **fonctionne** et ce qui **ne fonctionne pas**



* Ne cherchez pas à « réparer » le problème
* Ne multipliez pas les couches : **une seule couche attendue par situation**

---

## Situations à analyser

---

### Situation 1 — Absence totale de connectivité

> L’ordinateur est allumé.
> Aucune communication réseau n’est possible.
> Le câble Ethernet n’est **physiquement pas branché** à l’ordinateur.

* Couche OSI concernée :
* Justification :

---

### Situation 2 — Accès réseau local fonctionnel, routage Internet impossible

> Le poste est connecté au Wi-Fi et l’interface réseau indique **“connecté”**.
> Le poste possède une **adresse IP valide**, un **masque correct**, et une **passerelle configurée**.
> La communication avec d’autres machines du **réseau local** fonctionne.
> En revanche, **aucun accès à Internet n’est possible** (aucun site ne répond).

* Couche OSI concernée :
* Justification :

*(Indice implicite : la connectivité locale fonctionne, mais la communication hors du réseau local échoue.)*

---

### Situation 3 — Connectivité IP valide, service web inaccessible

> Le poste peut joindre une adresse IP publique avec la commande `ping`.
> La résolution DNS fonctionne correctement.
> Lorsqu’un utilisateur tente d’accéder à un site web via un navigateur,
> celui-ci affiche une **erreur liée au chargement de la page web**.

* Couche OSI concernée :
* Justification :

*(Indice implicite : la communication réseau fonctionne, mais l’application ne répond pas correctement.)*

---

### Situation 4 — Problème de port applicatif

> Un serveur héberge un service web fonctionnel.
> Le service écoute sur un port précis.
> Le client tente d’accéder au serveur en utilisant **un port différent**.
> La machine est joignable sur le réseau.

* Couche OSI concernée :
* Justification :

---

### Situation 5 — Mauvaise configuration IP

> Le poste dispose d’une adresse IP.
> Le masque de sous-réseau est incorrect.
> Les machines qui devraient être dans le même réseau logique
> ne peuvent pas communiquer entre elles.

* Couche OSI concernée :
* Justification :

