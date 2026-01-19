# TP — Comprendre et expérimenter le DHCP

## Rappel théorique

### Pourquoi DHCP ?

Sans DHCP, chaque machine d’un réseau doit être configurée manuellement :

- Adresse IP
- Masque de sous-réseau
- Passerelle par défaut
- DNS

Cette approche devient rapidement ingérable et source d’erreurs.

Le **DHCP (Dynamic Host Configuration Protocol)** permet :
- Une attribution **automatique**
- **Centralisée**
- **Temporaire** (bail / lease)

---

### Le dialogue DHCP : DORA

Le protocole DHCP repose sur quatre messages :

| Étape | Nom        | Description |
|-----|-----------|-------------|
| 1   | Discover  | Le client cherche un serveur DHCP |
| 2   | Offer     | Le serveur propose une configuration |
| 3   | Request   | Le client accepte l’offre |
| 4   | Ack       | Le serveur confirme l’attribution |

---

## Topologie du TP

- 1 switch
- 1 serveur
- 3 PC
- Tous les équipements sont connectés au switch via des câbles droits

---

## Partie 1 — Mise en place du réseau

### Étape 1 — Création de la topologie

1. Ouvrir Cisco Packet Tracer
2. Ajouter :
   - 1 switch
   - 1 serveur
   - 3 PC
3. Relier tous les équipements au switch

---

### Étape 2 — Configuration IP du serveur

1. Cliquer sur le **Server**
2. Onglet **Desktop → IP Configuration**
3. Configurer une IP statique :

IP address: 192.168.1.2
Subnet mask: 255.255.255.0
Default gateway: 192.168.1.1


> Le serveur DHCP doit avoir une adresse IP fixe.

---

## Partie 2 — Configuration du serveur DHCP

### Étape 3 — Activation du service DHCP

1. Sur le serveur : **Services → DHCP**
2. Activer le service DHCP
3. Créer un pool avec les paramètres suivants :

| Champ | Valeur |
|-----|-------|
| Pool Name | LAN |
| Default Gateway | 192.168.1.1 |
| DNS Server | 8.8.8.8 |
| Start IP Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| Maximum Number of Users | 50 |

4. Cliquer sur **Add**

---

## Partie 3 — Configuration des clients

### Étape 4 — Configuration DHCP des PC

Pour chaque PC :

1. **Desktop → IP Configuration**
2. Sélectionner **DHCP**
3. Attendre l’attribution automatique de l’adresse IP

---

### Étape 5 — Vérification réseau

Depuis un PC :

ipconfig


Puis tester la connectivité :

ping 192.168.1.2


---

## Partie 4 — Observation du protocole DHCP

### Étape 6 — Mode simulation

1. Passer en **Simulation Mode**
2. Filtrer uniquement le protocole **DHCP**
3. Sur un PC :
   - Renouveler la configuration DHCP
4. Lancer la simulation pas à pas

---

### Étape 7 — Analyse

Pour chaque message observé :

- Qui est l’émetteur ?
- Qui est le destinataire ?
- Pourquoi le broadcast est-il utilisé ?
- À quelle couche OSI appartient DHCP ?

---

## Partie 5 — DHCP vs IP statique

### Étape 8 — Comparaison

1. Mettre un PC en IP statique
2. Laisser les autres en DHCP
3. Comparer :
   - Facilité de configuration
   - Risques d’erreurs
   - Scalabilité du réseau

---

## Questions de synthèse

1. Pourquoi un client DHCP peut-il communiquer sans adresse IP initiale ?
2. Que se passe-t-il si le serveur DHCP est indisponible ?
3. Pourquoi certaines adresses sont-elles exclues du pool DHCP ?
4. DHCP est-il indispensable dans tous les réseaux ?
