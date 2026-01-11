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
<img width="675" height="467" alt="image" src="https://github.com/user-attachments/assets/41839b72-95df-48f9-8d25-e39e2f9dd5ad" />


### SSL MITM Filtering

- Activer **HTTPS/SSL interception**
- Mode SSL/MITM : **Splice Whitelist, Bump Otherwise**
- Configurer le port proxy SSL sur **3129**
- Sauvegarder
<img width="700" height="463" alt="image" src="https://github.com/user-attachments/assets/c54ccfdb-5d07-41fe-bcb1-3ed3fd825821" />

---

## SSL MITM Configuration

Accéder à : System>Certificates>CA

Créer une nouvelle autorité de certification interne et Exporter le certificat apres creation:
| Champ | Valeur |
|------|------|
| Descriptive Name | Squid_CA |
| Method | Create an internal Certificate Authority |
| Common Name | Squid_CA |
| Lifetime | 3650 days |

Exporter le certificat apres Creation

## Client Certificate Deployment (Windows)

Sur chaque poste client :

1. Importer le certificat Squid_CA  
2. Choisir **Placer tous les certificats dans le magasin suivant**  
3. Sélectionner **Trusted Root Certification Authorities**
   <img width="525" height="84" alt="image" src="https://github.com/user-attachments/assets/f6bc5562-3618-4596-b455-37c1370c164a" />
  
5. Redémarrer la machine

---

## Validation

Accéder à un site HTTPS (ex. https://www.google.com).  
Ouvrir les informations du certificat dans le navigateur.

Si l’autorité **Squid_CA** est affichée, cela confirme que le trafic HTTPS est intercepté, déchiffré et journalisé par la passerelle de sécurité.
<img width="867" height="565" alt="image" src="https://github.com/user-attachments/assets/84cf5865-386f-4614-b1f8-2e4707fb4e22" />

---
## Screenshot 
### Squid proxy general configuration
<img width="675" height="467" alt="image" src="https://github.com/user-attachments/assets/5a1fc397-3be7-4d1d-8735-82aa10b5d4cf" />
### Squid SSL MITM configuration
<img width="700" height="463" alt="image" src="https://github.com/user-attachments/assets/535907cd-f01c-4fb3-a162-2a7acbe3409c" />
### Trusted Root Certificate installed on Windows
<img width="525" height="84" alt="image" src="https://github.com/user-attachments/assets/e00ad74d-7cf3-4cc1-a13e-48fc8320ebe2" />
SSL MITM Certificate Validation
<img width="867" height="565" alt="image" src="https://github.com/user-attachments/assets/f529f7e5-aeae-4d53-b221-7d63a5cfbdfa" />


# Resultat 

La solution permet:

- L’interception du trafic HTTPS
- L’analyse du trafic chiffré
- L’application de politiques de filtrage web
- La journalisation centralisée des connexions web

  Ce lab démontre la mise en œuvre d’une passerelle d’inspection SSL de niveau entreprise.
