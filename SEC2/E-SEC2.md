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
