# SSL-inspection-gateway-lab- pFSense and squid Proxy (FR/ENG)

# Overview
Ce laboratoire présente la mise en place d'une passerelle de securité résau capable d'intercepter, déchiffrer et analyser le trafic HTTPS en environnement LAN à l’aide de pfSense et du Squid Proxy.
La solution repose sur une inspection SSL de type Man-In-The-Middle  (on path) permettant d'appliquer des politiques de filtrages Web,de surveiller les connexions sécurisées et d’analyser le trafic réseau chiffré.
Ce lab nous permet de simuler une infrastrucre de sécurité web de niveau entreprise.

## Architechture
Windows Clients
|
(LAN Network)
|
pfSense Firewall
|
Squid Proxy (SSL MITM)
|
Internet


pfSense agit comme autorité de certification interne afin d’intercepter et réémettre les connexions HTTPS de manière contrôlée.

---

## Environment

- pfSense firewall
- Squid Proxy (transparent HTTP/HTTPS)
- Internal Certificate Authority (CA)
- Windows clients
- LAN network

---

## Squid Proxy Installation

Squid Proxy est installé à partir du package manager pfSense.

Accéder à : Services > Squid Proxy Server > General

### General Settings

- Activer **Enable Squid Proxy**
- Activer **Transparent HTTP Proxy**
- Configurer le port proxy sur **3128**

### SSL MITM Filtering

- Activer **HTTPS/SSL interception**
- Mode SSL/MITM : **Splice Whitelist, Bump Otherwise**
- Configurer le port proxy SSL sur **3129**
- Sauvegarder

---

## SSL MITM Configuration

Accéder à : System>Certificates>CA

Créer une nouvelle autorité de certification interne et Exporter le certificat apres creation

## Client Certificate Deployment (Windows)

Sur chaque poste client :

1. Importer le certificat Squid_CA  
2. Choisir **Placer tous les certificats dans le magasin suivant**  
3. Sélectionner **Trusted Root Certification Authorities**  
4. Redémarrer la machine

---

## Validation

Accéder à un site HTTPS (ex. https://www.google.com).  
Ouvrir les informations du certificat dans le navigateur.

Si l’autorité **Squid_CA** est affichée, cela confirme que le trafic HTTPS est intercepté, déchiffré et journalisé par la passerelle de sécurité.

---

# Resultat 

La solution permet:

- L’interception du trafic HTTPS
- L’analyse du trafic chiffré
- L’application de politiques de filtrage web
- La journalisation centralisée des connexions web

  Ce lab démontre la mise en œuvre d’une passerelle d’inspection SSL de niveau entreprise.
