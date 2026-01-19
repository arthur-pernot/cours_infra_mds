# TP HTTP vs HTTPS dans Cisco Packet Tracer

## Objectifs pédagogiques

À l’issue de ce TP, l’étudiant devra être capable de :

- Mettre en place un **serveur Web** accessible en **HTTP (port 80)** et en **HTTPS (port 443)**
- Comprendre la différence entre **connexion TCP** et **sécurisation TLS**
- Observer, en mode Simulation, les éléments suivants :
  - Résolution **DNS**
  - Établissement TCP (SYN / SYN-ACK / ACK)
  - Échanges HTTP (lisibles) vs HTTPS (non lisibles)
  - Différence de ports 80 vs 443
- Expliquer le rôle des **certificats** dans l’authentification du serveur (selon possibilités Packet Tracer)

> Remarque : Packet Tracer simule certains aspects de TLS/HTTPS de manière simplifiée. L’objectif est l’**intuition réseau** : ports, flux, encapsulation, et différence “en clair vs chiffré”.

---

## Durée et organisation (2h)

- Travail en binômes
- Livrables :
  1. Capture d’écran de la topologie
  2. Tableau d’adressage complété
  3. 5 captures “Simulation” (DNS, TCP 80, HTTP, TCP 443, HTTPS)
  4. Réponses aux questions

### Découpage conseillé
- Partie A (20 min) : Topologie + plan d’adressage
- Partie B (20 min) : Configuration IP + tests de base
- Partie C (20 min) : DNS + accès HTTP
- Partie D (30 min) : Activation HTTPS + tests
- Partie E (20 min) : Analyse en Simulation (HTTP vs HTTPS)
- Partie F (10 min) : Questions de synthèse

---

## Matériel Packet Tracer

Équipements à utiliser :
- 1 routeur (ex : **2911**)
- 1 switch (ex : **2960**)
- 1 serveur (**Server-PT**)
- 2 PC (**PC-PT**)

Câbles :
- Copper straight-through (PC ↔ Switch, Serveur ↔ Switch, Switch ↔ Routeur)

---

# Partie A — Construction de la topologie (20 min)

## A1. Placer les équipements

1. Ouvrir Cisco Packet Tracer
2. Ajouter :
   - 1 **Router** (2911)
   - 1 **Switch** (2960)
   - 1 **Server**
   - 2 **PC**
3. Relier :
   - PC1 ↔ Switch
   - PC2 ↔ Switch
   - Server ↔ Switch
   - Switch ↔ Router (interface **G0/0** du routeur)

## A2. Renommer les équipements (recommandé)

- Router : `R1`
- Switch : `SW1`
- Server : `SRV-WEB`
- PC : `PC1`, `PC2`

---

# Partie B — Plan d’adressage + configuration IP (20 min)

## B1. Plan d’adressage

Utiliser le réseau suivant : `192.168.10.0/24`

| Équipement | Interface | IP | Masque | Passerelle |
|---|---|---:|---:|---:|
| R1 | G0/0 | 192.168.10.1 | 255.255.255.0 | — |
| SRV-WEB | Fa0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | Fa0 | 192.168.10.101 | 255.255.255.0 | 192.168.10.1 |
| PC2 | Fa0 | 192.168.10.102 | 255.255.255.0 | 192.168.10.1 |

## B2. Configurer le routeur R1

1. Cliquer sur `R1` → **CLI**
2. Taper :

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
end
write memory
```
✓ Vérifier que l’interface est up :

show ip interface brief

B3. Configurer SRV-WEB

    Cliquer sur SRV-WEB → Desktop → IP Configuration

    Saisir :

        IP : 192.168.10.10

        Mask : 255.255.255.0

        Gateway : 192.168.10.1

B4. Configurer PC1 et PC2

Pour chaque PC :

    Desktop → IP Configuration

    Renseigner IP / masque / passerelle comme dans le tableau.

B5. Test de connectivité (ICMP)

Depuis PC1 :

    Desktop → Command Prompt

    Tester :

ping 192.168.10.1
ping 192.168.10.10

Attendu :

    Réponses OK

Questions (à répondre)

    À quoi sert la passerelle dans ce scénario, même si tout est sur le même LAN ?

    Quel protocole est utilisé par ping ?

Partie C — DNS + HTTP (20 min)

Objectif : accéder au serveur via un nom de domaine, puis en HTTP.
C1. Activer le service DNS sur le serveur

    SRV-WEB → Services → DNS

    Mettre DNS: On

    Ajouter un enregistrement A :

        Name : site.local

        Address : 192.168.10.10

    Cliquer Add / Save selon l’interface

C2. Configurer les clients pour utiliser le DNS

Sur PC1 et PC2 :

    Desktop → IP Configuration

    DNS Server : 192.168.10.10

C3. Test DNS

Sur PC1 :

    Command Prompt :

ping site.local

Attendu :

    Le nom site.local se résout vers 192.168.10.10

C4. Activer HTTP sur le serveur

    SRV-WEB → Services → HTTP

    Mettre HTTP: On

    (Optionnel) Modifier la page d’accueil (index) pour afficher :

        Titre : “Bienvenue sur SRV-WEB”

        Une ligne “HTTP OK”

C5. Accès HTTP depuis le navigateur

Sur PC1 :

    Desktop → Web Browser

    Aller sur :

        http://site.local

Attendu :

    La page s’affiche

Questions

    Quel port est utilisé par HTTP par défaut ?

    Dans un réseau, pourquoi passer par DNS plutôt que taper l’IP ?

Partie D — HTTPS (TLS) sur le serveur (30 min)

Objectif : activer HTTPS et observer la différence.
D1. Activer HTTPS sur le serveur

    SRV-WEB → Services → HTTP

    Repérer l’option HTTPS (ou SSL/TLS selon version)

    Mettre HTTPS: On

    Selon la version de Packet Tracer :

        Vous pouvez avoir un bouton / champ pour générer un certificat (self-signed).

        Si aucune gestion de certificat n’est proposée, Packet Tracer simule quand même HTTPS au niveau service/port.

D2. Accéder au site en HTTPS

Sur PC1 :

    Web Browser

    Aller sur :

        https://site.local

Attendu :

    La page s’affiche également (parfois avec un avertissement simulé selon version)

D3. Vérification par port (méthode réseau)

Sur PC1 → Command Prompt, tester une connexion vers les ports (si la commande est disponible dans votre PT) :

    Si telnet est disponible :

telnet 192.168.10.10 80
telnet 192.168.10.10 443

Attendu :

    Connexion possible sur 80 et 443 si services actifs

    Si telnet n’est pas disponible, ne pas bloquer : l’analyse port se fera en Simulation.

Questions

    Quel port est utilisé par HTTPS ?

    Pourquoi HTTPS n’est pas “un autre protocole applicatif” mais plutôt HTTP + TLS ?

Partie E — Analyse en mode Simulation (20 min)

Objectif : observer concrètement les étapes réseau (DNS, TCP, HTTP vs HTTPS).
E1. Préparer la Simulation

    Passer en bas à droite sur Simulation

    Dans Event List Filters, laisser uniquement :

        DNS

        TCP

        HTTP

        HTTPS (si présent)

        (Optionnel) ARP pour voir la résolution MAC au départ

    Conseil : commencer avec ARP activé, puis le désactiver ensuite pour simplifier.

    Cliquer sur Clear (effacer les événements)

E2. Observer un accès HTTP

    Sur PC1, en mode navigateur :

        Aller sur http://site.local

    Revenir Simulation

    Cliquer plusieurs fois sur Capture/Forward

À capturer (capture d’écran ou notes)

    Un paquet DNS : requête + réponse

    Un échange TCP vers port 80

    Un événement HTTP (requête/réponse)

Questions guidées

    Quelle est la suite TCP observée avant HTTP ?

    Où apparaît le port 80 ?

    Peut-on considérer HTTP comme “au-dessus” de TCP ? Pourquoi ?

E3. Observer un accès HTTPS

    Cliquer Clear (effacer les événements)

    Sur PC1 :

        Aller sur https://site.local

    Cliquer Capture/Forward jusqu’à stabilisation

À capturer

    DNS (si relancé)

    TCP vers port 443

    Événements HTTPS/TLS (selon affichage)

Comparaison attendue

    HTTP : on voit “HTTP” et parfois des détails lisibles

    HTTPS : on voit la connexion, mais le contenu applicatif n’est pas lisible (ou est représenté de façon opaque)

Questions

    En quoi les flux HTTPS ressemblent-ils à HTTP au niveau transport ?

    Où se situe la différence principale (ports, chiffrement, contenu) ?

    Que garantit TLS que HTTP seul ne garantit pas ?

Partie F — Synthèse et questions finales (10 min)

Répondre en 5–8 lignes par question.

    Donner une définition claire de :

        Confidentialité

        Intégrité

        Authentification

    Expliquer pourquoi HTTPS utilise :

        du chiffrement asymétrique (au démarrage)

        du chiffrement symétrique (ensuite)

    Citer 2 attaques possibles sur HTTP en clair (en réseau local ou Wi-Fi public).

    Si un attaquant “redirige” le DNS de site.local vers une autre IP, que risque-t-il de se passer ?
    (Indice : quel mécanisme HTTPS aide à limiter ce risque ?)

Bonus
Bonus 1 — Bloquer HTTP pour forcer HTTPS (logique “site sécurisé”)

    Objectif : simuler une politique “HTTP interdit”.

    Sur le serveur, désactiver HTTP: Off et laisser HTTPS: On

    Tester :

        http://site.local (doit échouer)

        https://site.local (doit fonctionner)

Questions :

    Pourquoi certains sites redirigent HTTP → HTTPS plutôt que couper HTTP directement ?

    Quel intérêt pour l’expérience utilisateur ?

Bonus 2 — Ajout d’un second nom DNS

    Ajouter dans DNS :

        admin.site.local → 192.168.10.10

    Tester accès navigateur.

