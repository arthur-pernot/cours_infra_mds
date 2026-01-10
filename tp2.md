# TP — Comprendre les adresses MAC et la table ARP

## Objectifs pédagogiques

À la fin de ce TP, vous serez capables de :

* expliquer **pourquoi le protocole ARP existe**
* comprendre **le rôle des adresses MAC** dans un réseau local
* observer **le fonctionnement réel d’ARP** dans Cisco Packet Tracer
* faire le lien entre **adresse IP (logique)** et **adresse MAC (physique)**

---

## 1. Rappels théoriques essentiels

### 1.1. Pourquoi ARP existe-t-il ?

Dans un réseau informatique :

* les applications utilisent des **adresses IP**
* le réseau local (Ethernet) transmet les données grâce aux **adresses MAC**

Problème :

> Une trame Ethernet **ne peut pas être envoyée avec une adresse IP seule**.

Il faut donc un mécanisme pour répondre à la question :

> *« J’ai une adresse IP, mais à quelle adresse MAC dois-je envoyer la trame ? »*

C’est exactement le rôle du **protocole ARP (Address Resolution Protocol)**.

---

### 1.2. Pourquoi utilise-t-on des adresses MAC dans un réseau local ?

* Les **adresses MAC** identifient **une carte réseau**
* Elles sont :

  * uniques
  * utilisées en **couche 2 (liaison de données)**
* Les **switches** travaillent uniquement avec les **adresses MAC**

Dans un LAN :

* le switch ne connaît **pas les IP**
* il a besoin d’un identifiant **local, stable et immédiat**

---

### 1.3. Pourquoi les adresses MAC ne traversent pas les réseaux ?

Lorsqu’un paquet traverse un **routeur** :

* l’en-tête Ethernet est **supprimé**
* un **nouvel en-tête Ethernet** est recréé pour le réseau suivant

Conclusion importante :

> Les adresses MAC **ne sont valables que dans un réseau local**.
> Les adresses IP servent à **traverser les réseaux**, les MAC à **circuler à l’intérieur** d’un réseau.

---

## 2. Mise en place du réseau dans Cisco Packet Tracer

### 2.1. Topologie à créer

Créer le réseau suivant :

```
PC1 ----|
        |---- Switch ---- PC2
PC3 ----|
```

Matériel :

* 1 switch
* 3 PC
* câbles Ethernet droits

---

## 3. Configuration des adresses IP

Attribuer des **adresses IP statiques** :

| Machine | Adresse IP  | Masque        |
| ------- | ----------- | ------------- |
| PC1     | 192.168.1.1 | 255.255.255.0 |
| PC2     | 192.168.1.2 | 255.255.255.0 |
| PC3     | 192.168.1.3 | 255.255.255.0 |

Sur chaque PC :

* Desktop → IP Configuration
* Renseigner IP et masque
* **Pas de passerelle** (pas de routeur ici)

---

## 4. Observation initiale de la table ARP

### 4.1. Sur PC1

1. Desktop → Command Prompt
2. Taper :

```bash
arp -a
```

### Questions :

* La table ARP est-elle vide ?
* Pourquoi est-ce logique à ce stade ?

---

## 5. Passage en mode Simulation

1. Passer en **Simulation Mode**
2. Dans les filtres :

   * **désactiver tous les protocoles**
   * **activer uniquement** :

     * ARP
     * ICMP

---

## 6. Lancer la simulation ARP

### 6.1. Depuis PC1

Dans le terminal de PC1 :

```bash
ping 192.168.1.2
```

---

## 7. Observation de la simulation

Observer attentivement :

1. Le **premier paquet envoyé**
2. Sa destination (broadcast ou unicast)
3. La réponse de PC2
4. L’envoi des paquets ICMP ensuite

### Questions à se poser :

* Pourquoi le premier message n’est-il **pas un ping** ?
* Pourquoi est-il envoyé à **tout le réseau** ?
* Pourquoi PC2 est-il le seul à répondre ?
* Pourquoi les pings suivants sont-ils directs ?

---

## 8. Analyse et compréhension

### 8.1. Ce que vous avez observé

1. PC1 ne connaît que l’IP de PC2
2. Il envoie une **requête ARP en broadcast** :

   > « Qui a l’IP 192.168.1.2 ? »
3. PC2 répond avec son **adresse MAC**
4. PC1 enregistre cette information
5. Les paquets ICMP sont envoyés **directement** à la bonne MAC

---

### 8.2. Concept clé à retenir

> ARP fonctionne comme un **cache** :
> tant que l’association IP ↔ MAC est connue, il n’y a plus de broadcast.

---

## 9. Arrêter la simulation

1. Cliquer sur **Stop**
2. Repasser en **Real Time Mode**

---

## 10. Observer la table ARP après la communication

Sur PC1 :

```bash
arp -a
```

### Questions finales :

* Une entrée est-elle apparue ?
* À quoi correspond-elle ?
* Pourquoi PC1 n’a-t-il plus besoin d’ARP pour les prochains pings ?

---

## 11. Conclusion du TP

À l’issue de ce TP, vous devez être capables d’expliquer :

* pourquoi un réseau local **ne peut pas fonctionner sans ARP**
* la différence entre :

  * **adresse IP** (logique, routable)
  * **adresse MAC** (physique, locale)
* le rôle du **switch** dans l’apprentissage des adresses MAC
* pourquoi ARP génère du **broadcast**, mais seulement au début

---

## 12. Questions de réflexion

> Que se passerait-il si :
>
> * deux machines avaient la même adresse IP ?
> * la table ARP contenait une mauvaise adresse MAC ?
