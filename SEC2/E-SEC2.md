# Concept d'un firewall

Un firewall est un système de sécurité qui contrôle le trafic réseau entrant et sortant en fonction de règles de sécurité prédéfinies. Il agit comme une barrière entre un réseau interne de confiance et des réseaux externes non fiables, comme Internet. Le firewall peut être matériel ou logiciel et est essentiel pour protéger les systèmes contre les accès non autorisés, les attaques et les malwares.

## 1. Tables de NetFilter

NetFilter est un cadre qui permet de créer des règles de filtrage de paquets sous Linux. Les tables de NetFilter sont des structures qui contiennent des ensembles de règles et déterminent comment le trafic doit être traité. Il existe plusieurs tables principales :

- **filter** : C'est la table par défaut qui gère le filtrage des paquets. Elle contient des chaînes pour décider si un paquet doit être accepté, rejeté ou ignoré.

- **nat** : Cette table est utilisée pour la translation d'adresses réseau (NAT). Elle permet de modifier les adresses IP des paquets lors de leur passage à travers le firewall, notamment pour le partage de connexion.

- **mangle** : Utilisée pour modifier les paquets en cours de traitement. Elle permet de changer les options des paquets, comme les TTL (Time to Live) ou le type de service (ToS).

- **raw** : Permet de gérer le traitement des paquets avant qu'ils ne soient pris en charge par les autres tables. Elle est souvent utilisée pour désactiver certaines fonctionnalités de suivi de connexions.

## 2. Chaînes de NetFilter

Chaque table contient des chaînes, qui sont des listes de règles. Les chaînes définissent les points de contrôle où les paquets sont analysés. Les principales chaînes dans la table `filter` sont :

- **INPUT** : Gère le trafic entrant destiné à l’hôte local. Les règles définies ici déterminent si un paquet entrant est accepté ou rejeté.

- **OUTPUT** : Gère le trafic sortant de l’hôte local. Les règles définies ici contrôlent les paquets qui sortent de l'hôte.

- **FORWARD** : Gère le trafic qui traverse l'hôte sans y être destiné, c’est-à-dire le trafic entre deux interfaces réseau. Les règles ici s'appliquent aux paquets qui ne sont pas destinés à l’hôte lui-même.

## 3. Exemples de règles par défaut

Voici quelques exemples de règles courantes basées sur les paramètres par défaut des firewalls :

1. **Accepter tout le trafic sortant :**
   ```bash
   iptables -A OUTPUT -j ACCEPT

# Concept d’une DMZ

Une **DMZ (Zone Démilitarisée)** est une zone réseau située entre le réseau interne sécurisé d'une organisation et le réseau externe (généralement Internet). Son but est de sécuriser l'accès aux services externes tout en isolant le réseau interne des menaces extérieures.

## Principes généraux d’une DMZ
- **Séparation des réseaux** : La DMZ isole les ressources internes sensibles des services accessibles depuis Internet.
- **Sécurisation par filtrage** : Utilisation de pare-feu et de contrôles d’accès pour limiter les flux de données entrants et sortants.
- **Accès restreint** : Les services exposés sont accessibles de manière contrôlée depuis l'extérieur, tandis que l'accès au réseau interne est limité.
- **Surveillance** : Mise en place de systèmes IDS/IPS pour détecter toute activité suspecte.

## Identification des flux
- **Entrants** : Les services accessibles depuis Internet (HTTP, FTP, SMTP, DNS) vers les serveurs dans la DMZ.
- **Sortants** : Réponses aux services externes, comme les pages web ou les emails envoyés.
- **Internes** : Flux entre la DMZ et le réseau interne pour des services spécifiques (bases de données, gestion).
- **Contrôles stricts** : Accès limité et surveillé pour les communications entre le réseau interne et la DMZ.

## Infrastructures possibles pour une DMZ
- **Architecture à un seul pare-feu** : Simple mais moins sécurisée.
- **Architecture à deux pare-feux** : Plus sécurisée, avec une séparation claire des flux entre Internet, DMZ et réseau interne.
- **Architecture multi-niveaux** : DMZ segmentée en plusieurs zones pour un contrôle granulaire.
- **DMZ avec bastion** : Utilisation d'un serveur intermédiaire pour sécuriser l'accès au réseau interne.
- **DMZ avec virtualisation ou conteneurisation** : Isolation des services à l'aide de machines virtuelles ou de containers pour renforcer la sécurité.

## Conclusion
La DMZ est une stratégie essentielle pour sécuriser les services exposés à Internet tout en préservant l'intégrité du réseau interne. Elle nécessite une gestion soignée des flux, des contrôles d’accès et une infrastructure adaptée pour être efficace.


# Concept d’un Proxy

## 1. Fonctionnement d’un Proxy
Un **proxy** est un serveur qui sert d’intermédiaire entre un client (comme un utilisateur) et un serveur distant, en filtrant ou en modifiant les requêtes et les réponses.

- **Proxy classique** : Intercepte et redirige les requêtes des clients vers des serveurs, permettant des fonctions comme le filtrage web, la mise en cache ou l’anonymat.
- **Proxy transparent** : Fonctionne sans que l’utilisateur soit au courant de son existence. Il intercepte le trafic sans nécessiter de configuration sur le client. Il est souvent utilisé pour la mise en cache ou la surveillance du réseau.
- **Reverse Proxy** : Se place entre les clients et un ou plusieurs serveurs, en agissant comme un intermédiaire pour diriger le trafic vers les serveurs appropriés. Il permet de gérer la charge, l'équilibrage de charge, ou d'appliquer des mesures de sécurité.

## 2. Analyse de divers produits et choix d’un proxy pour la DMZ
- **Squid** : Proxy populaire et open-source, efficace pour la mise en cache et le filtrage du contenu. Idéal pour les proxys classiques ou transparents.
- **Nginx** : Très utilisé comme reverse proxy pour l’équilibrage de charge, la gestion du trafic HTTP/HTTPS et la sécurisation des applications web.
- **HAProxy** : Utilisé principalement comme reverse proxy et load balancer avec des fonctionnalités avancées pour la haute disponibilité.
- **Choix pour la DMZ** : Un **rerverse proxy** comme **Nginx** ou **HAProxy** est souvent préféré dans la DMZ pour sécuriser l’accès aux applications internes et gérer la répartition du trafic.

## 3. Déploiement automatique de la configuration d’un Proxy
- **Outils de gestion de configuration** : Des outils comme **Ansible**, **Puppet**, ou **Chef** peuvent être utilisés pour déployer automatiquement la configuration du proxy sur plusieurs serveurs, assurant une gestion cohérente et évolutive des paramètres.
- **Conteneurisation** : L'utilisation de Docker ou Kubernetes permet de déployer et gérer automatiquement des instances de proxy dans des environnements conteneurisés.

## 4. Inspection SSL Profonde (Deep SSL Inspection)
- **Définition** : L’inspection SSL profonde consiste à intercepter et à décrypter les connexions SSL/TLS pour analyser le contenu (données chiffrées) avant de les renvoyer au client ou serveur cible.
- **Comment ça marche** : Le proxy ou le dispositif de sécurité agit comme un "homme du milieu", déchiffrant le trafic SSL/TLS, inspecte les données (à la recherche de menaces), puis renvoie le trafic déchiffré au serveur ou client en ré-encryptant la connexion. Cette technique est utilisée pour détecter les menaces qui passent normalement inaperçues dans le trafic chiffré.
