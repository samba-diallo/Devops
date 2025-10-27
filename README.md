
# 🚀 Déploiement d'une Application Node.js sur AWS EC2

> **Guide Complet pour Débutants - De Zéro à Production**
>
> Par **samba-diallo** | Octobre 2025

[![AWS](https://img.shields.io/badge/AWS-EC2-orange?style=flat&logo=amazon-aws)](https://aws.amazon.com/ec2/)
[![Node.js](https://img.shields.io/badge/Node.js-21.x-green?style=flat&logo=node.js)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![DevOps](https://img.shields.io/badge/DevOps-Basics-blueviolet)](https://aws.amazon.com/)

---

## 📑 Table des Matières

- [Introduction](#-introduction)
- [Prérequis](#-prérequis)
- [Partie 1 : Configuration AWS](#-partie-1--configuration-aws)
- [Partie 2 : Déploiement EC2](#-partie-2--déploiement-ec2)
- [Partie 3 : Comprendre le Code](#-partie-3--comprendre-le-code)
- [Partie 4 : Test & Vérification](#-partie-4--test--vérification)
- [Exercices Pratiques](#-exercices-pratiques)
- [Dépannage](#-dépannage)
- [Nettoyage](#-nettoyage)
- [Conclusion](#-conclusion)
- [Ressources](#-ressources)

---

## 🎯 Introduction

### Qu'allez-vous Apprendre ?

Ce guide vous apprend à déployer une application web simple sur **Amazon Web Services (AWS)** en utilisant **EC2 (Elastic Compute Cloud)**. C'est votre première étape dans le monde du **DevOps** et du **Cloud Computing**.

### 🎓 Compétences Acquises

- ✅ Créer et sécuriser un compte AWS professionnel
- ✅ Maîtriser IAM (Identity and Access Management)
- ✅ Lancer et configurer un serveur virtuel (EC2)
- ✅ Configurer des pare-feu cloud (Security Groups)
- ✅ Automatiser le déploiement avec User Data
- ✅ Déployer une application Node.js
- ✅ Comprendre les bases du DevOps

### 💼 Pourquoi C'est Important ?

```
🌐 Le Cloud Computing est partout
   → 94% des entreprises utilisent le cloud (source: Flexera 2023)
   → AWS détient 32% du marché cloud mondial

💰 Opportunités de Carrière
   → DevOps Engineer : 70-120k€/an
   → Cloud Architect : 80-150k€/an
   → Compétence #1 recherchée en tech (LinkedIn)

🚀 Foundation pour Concepts Avancés
   → Microservices, Containers (Docker, Kubernetes)
   → CI/CD (Jenkins, GitLab, GitHub Actions)
   → Infrastructure as Code (Terraform, CloudFormation)
```

---

## 📋 Prérequis

### Connaissances

| Niveau | Requis |
|--------|--------|
| **Essentiel** | Utilisation d'un navigateur web |
| **Recommandé** | Notions de ligne de commande (terminal/bash) |
| **Utile** | Bases de JavaScript/Node.js |

### Outils Nécessaires

| Outil | Utilité | Téléchargement |
|-------|---------|----------------|
| **Navigateur** | Accès AWS Console | Chrome, Firefox, Safari |
| **App Authentification** | Sécurité MFA | [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2) |
| **Gestionnaire Mots de Passe** | Sécurité | [Bitwarden](https://bitwarden.com/) (gratuit) |

### 💰 Coûts AWS

```
✅ AWS Free Tier (12 mois)
   → 750 heures/mois de t2.micro GRATUIT
   → Suffisant pour ce tutoriel
   → Valable 12 mois après inscription

⚠️ Important :
   → Utilisez t2.micro ou t3.micro uniquement
   → TERMINEZ l'instance après test
   → Sinon : ~0.01€/heure ≈ 7€/mois
```

---

## 🔐 Partie 1 : Configuration AWS

### Étape 1.1 : Créer un Compte AWS

#### 🔗 Inscription

1. Aller sur **[aws.amazon.com](https://aws.amazon.com)**
2. Cliquer **"Créer un compte AWS"**
3. Remplir les informations :
   - **Email** (deviendra votre compte root)
   - **Nom du compte AWS**
   - **Mot de passe fort** (12+ caractères, majuscules, minuscules, chiffres, symboles)

4. Vérifier l'email
5. Ajouter une carte bancaire (vérification, pas de débit immédiat)
6. Vérifier l'identité (SMS ou appel)
7. Choisir le plan **Gratuit**

---

#### ⚠️ Comprendre le Compte Root

> **Le compte root est comme la clé principale d'un immeuble qui ouvre TOUTES les portes.**

```
🔑 Compte Root (Utilisateur Racine)
   ✅ Accès ILLIMITÉ à tout le compte AWS
   ✅ Peut créer, modifier, supprimer TOUT
   ✅ Peut fermer le compte

🚨 Risques Majeurs
   ❌ Identifiants volés = Contrôle TOTAL par l'attaquant
   ❌ Peut générer des milliers d'euros de factures
   ❌ Peut supprimer toutes vos données

🔒 Bonnes Pratiques CRITIQUES
   1. Ne JAMAIS utiliser le root au quotidien
   2. Stocker dans un gestionnaire de mots de passe
   3. Activer MFA immédiatement
   4. Créer un utilisateur IAM pour l'usage quotidien
```

---

### Étape 1.2 : Créer un Utilisateur IAM

#### 🎯 Qu'est-ce qu'IAM ?

**IAM** = **Identity and Access Management**

```
IAM permet de :
→ Créer des utilisateurs avec permissions spécifiques
→ Définir QUI peut faire QUOI
→ Appliquer le principe du "moindre privilège"
→ Auditer les actions (qui, quoi, quand)
```

---

#### 📝 Création de l'Utilisateur

**1. Accéder à IAM**

```
AWS Console → Barre de recherche → "IAM" → Cliquer
```

**2. Naviguer vers Users**

```
IAM Console → Users (Utilisateurs) → Create User
```

**3. Configuration de Base**

| Paramètre | Valeur |
|-----------|--------|
| User name | `votre-nom` (ex: `samba-diallo`) |
| Provide user access to AWS Management Console | ✅ Coché |
| Console password | Auto-generated (recommandé) |

**4. Attacher des Permissions**

```
Option : "Attach policies directly"
Policy : AdministratorAccess
```

> ⚠️ **AdministratorAccess** donne un accès complet. OK pour apprendre, JAMAIS en production !

**5. Créer et Sauvegarder**

```
Cliquer : Next → Review → Create User
```

**⚠️ CRUCIAL : Sauvegarder immédiatement**

| Information | Action |
|-------------|--------|
| Console sign-in URL | 📋 Copier dans gestionnaire de mots de passe |
| User name | 📋 Copier |
| Console password | 🚨 **Affiché qu'UNE FOIS** → Copier immédiatement |

---

### Étape 1.3 : Activer la MFA

#### 🛡️ Qu'est-ce que la MFA ?

**MFA** = **Multi-Factor Authentication** = Authentification à Plusieurs Facteurs

```
Principe :
→ Quelque chose que vous CONNAISSEZ (mot de passe)
+ Quelque chose que vous POSSÉDEZ (smartphone)
= 🔒 Double sécurité
```

#### 📊 Pourquoi C'est CRITIQUE

```
Statistiques :
→ 81% des violations = Mots de passe faibles/volés (Verizon)
→ MFA bloque 99.9% de ces attaques (Microsoft)

Sans MFA :
❌ Mot de passe volé = Compte compromis

Avec MFA :
✅ Attaquant bloqué sans le 2ème facteur
✅ Protection contre phishing
✅ Norme de sécurité entreprise
```

---

#### 🔧 Configuration MFA

**1. Installer une App d'Authentification**

| Application | Plateformes | Note |
|-------------|-------------|------|
| **Google Authenticator** | iOS, Android | ⭐⭐⭐⭐⭐ Simple |
| **Microsoft Authenticator** | iOS, Android | ⭐⭐⭐⭐⭐ Fonctionnalités+ |
| **Authy** | iOS, Android, Desktop | ⭐⭐⭐⭐ Sauvegarde cloud |

**2. Activer MFA**

```
IAM Console
→ Users
→ Cliquez sur votre utilisateur
→ Security credentials
→ Multi-factor authentication (MFA)
→ Assign MFA device
```

**3. Choisir le Type**

```
✅ Authenticator app (RECOMMANDÉ - Gratuit)
❌ Security key (Clé USB - 30-50€)
❌ Hardware TOTP token (100€+)
```

**4. Scanner le QR Code**

```
1. AWS affiche un QR Code
2. Ouvrir l'app sur votre smartphone
3. Appuyer sur "+" → Scanner
4. Pointer vers le QR Code
5. L'app génère un code à 6 chiffres
```

**5. Vérification**

```
1. Entrer le code actuel (ex: 123 456)
2. Attendre 30 secondes (le code change)
3. Entrer le nouveau code (ex: 789 012)
4. Cliquer "Add MFA"
```

**6. ✅ Confirmation**

```
Message : "MFA device successfully assigned"
```

---

#### 🔐 Nouvelle Procédure de Connexion

```
1. URL de connexion IAM
2. Nom d'utilisateur
3. Mot de passe
4. 🆕 Code MFA à 6 chiffres (de l'app)
5. ✅ Connexion réussie
```

---

### Étape 1.4 : Se Connecter avec IAM

```
1. 🚪 Se DÉCONNECTER du compte root
2. 🔗 Aller sur l'URL de connexion IAM
3. 👤 Entrer nom d'utilisateur
4. 🔑 Entrer mot de passe
5. 📱 Entrer code MFA
6. ✅ Connecté en tant qu'utilisateur IAM
```

---

## 🖥️ Partie 2 : Déploiement EC2

### Qu'est-ce qu'EC2 ?

**EC2** = **Elastic Compute Cloud**

```
Définition :
→ Service de serveurs virtuels d'AWS
→ "Ordinateurs dans le cloud"
→ Location de puissance de calcul à la demande

Avantages :
✅ Lancement en quelques minutes
✅ Payez seulement ce que vous utilisez
✅ Scalable instantanément
✅ Disponible dans 25+ régions mondiales

Cas d'usage :
→ Héberger sites web / applications
→ Serveurs de base de données
→ Calcul haute performance
→ Machine learning / IA
→ Serveurs de jeux
```

---

### Étape 2.1 : Lancer une Instance

**1. Accéder à EC2**

```
AWS Console → Rechercher "EC2" → Cliquer
```

**2. Lancer**

```
🔶 Bouton orange : "Launch instance"
```

---

### Étape 2.2 : Configuration de l'Instance

#### 1️⃣ Nom et Tags

```
Name: sample-app
```

> 💡 **Bonne pratique** : Nommage cohérent (ex: `projet-env-fonction`)

---

#### 2️⃣ AMI (Amazon Machine Image)

**Qu'est-ce qu'une AMI ?**

```
AMI = "Photo" d'un système d'exploitation
→ Avec logiciels pré-installés
→ Prête à démarrer

Types :
- Amazon Linux (optimisé AWS)
- Ubuntu, Red Hat, Windows Server
- AMI personnalisées
```

**Choix :**

```
✅ Amazon Linux 2023 (ou Amazon Linux 2)

Pourquoi ?
→ Optimisée pour AWS
→ ✅ GRATUITE (Free Tier)
→ Support officiel Amazon
→ Mises à jour sécurité régulières
```

---

#### 3️⃣ Type d'Instance

**Définit les ressources matérielles**

```
✅ t2.micro OU t3.micro

Specs :
- 1 vCPU
- 1 GB RAM
- Performance modérée
- ✅ FREE TIER (750h/mois gratuit)

Coût (hors Free Tier) :
- ~0.01€/heure ≈ 7€/mois
```

**Comparaison Types d'Instances**

| Type | vCPU | RAM | Usage | Coût/h ($) |
|------|------|-----|-------|------------|
| t2.micro | 1 | 1 GB | Test, dev | 0.0116 |
| t3.small | 2 | 2 GB | Petites apps | 0.0208 |
| t3.medium | 2 | 4 GB | Apps moyennes | 0.0416 |
| m5.large | 2 | 8 GB | Production | 0.096 |

---

#### 4️⃣ Paire de Clés (Key Pair)

```
☑️ "Proceed without a key pair"

Pourquoi ?
→ Notre app s'installe automatiquement (User Data)
→ Test via HTTP (navigateur)
→ Pas besoin de SSH pour ce tutoriel
```

> 📝 **Note** : En production, utilisez TOUJOURS une key pair !

---

#### 5️⃣ Paramètres Réseau 🔥 CRITIQUE

##### a) VPC et Subnet

```
☑️ Network : Default VPC (vpc-xxxxxxxx)
☑️ Subnet : No preference
☑️ Auto-assign public IP : Enable
```

**Pourquoi "Auto-assign public IP" ?**

```
Enable → Instance accessible depuis Internet
        → Nécessaire pour tester dans le navigateur

Disable → Instance seulement accessible depuis AWS
         → Pas d'IP publique
```

---

##### b) Security Group 🔥 CRUCIAL

**Qu'est-ce qu'un Security Group ?**

```
Security Group = Pare-feu virtuel

Fonction :
→ Contrôle trafic entrant (inbound)
→ Contrôle trafic sortant (outbound)

Principe par défaut :
❌ TOUT EST BLOQUÉ (deny all)
✅ Vous devez EXPLICITEMENT autoriser
```

**Analogie :**

> Security Group = Agent de sécurité à l'entrée.
> Sans règle, personne ne peut entrer (même visiteurs légitimes).

**Configuration CRITIQUE :**

```
🔥 Firewall (security groups)
   ☑️ Create security group

Règles :
   ❌ DÉCOCHER : [ ] Allow SSH traffic from
   ✅ COCHER :   [✓] Allow HTTP traffic from the internet
```

**Détails de la Règle HTTP**

| Paramètre | Valeur | Explication |
|-----------|--------|-------------|
| Type | HTTP | Trafic web |
| Protocol | TCP | Protocol transport |
| Port | 80 | Port HTTP standard |
| Source | 0.0.0.0/0 | Tout Internet |

**⚠️ ERREUR FRÉQUENTE #1**

```
🚨 Oublier de cocher "Allow HTTP traffic"
   → Erreur "Connection timeout"
   → Instance fonctionne MAIS firewall bloque

C'est l'erreur la plus commune dans ce tutoriel !
```

---

##### c) HTTP vs HTTPS

```
HTTP (Port 80) :
→ NON chiffré
→ Pas de certificat SSL nécessaire
→ ✅ Simple pour apprendre
→ ❌ JAMAIS en production avec données sensibles

HTTPS (Port 443) :
→ CHIFFRÉ (SSL/TLS)
→ Nécessite certificat SSL
→ ✅ Obligatoire en production
→ Configuration plus complexe

Notre "Hello World" :
→ HTTP suffit (pas de données sensibles)
```

---

#### 6️⃣ Stockage

```
✅ Laisser par défaut :
   - Type : gp3 (General Purpose SSD)
   - Taille : 8 GB
   - Chiffrement : Activé
```

---

#### 7️⃣ User Data 🚀 IMPORTANT

**Qu'est-ce que le User Data ?**

```
User Data = Script exécuté AU PREMIER BOOT

Caractéristiques :
✅ Automatique (exécuté par AWS)
✅ Une seule fois (premier démarrage uniquement)
✅ Privilèges root (administrateur)
✅ Automatise installation et configuration

Cas d'usage :
→ Installer logiciels (Node.js, Apache, Docker)
→ Configurer fichiers
→ Lancer applications
→ Télécharger code depuis GitHub
```

**Analogie :**

> User Data = Liste de courses pour un assistant.
> Vous donnez la recette, il fait tout pendant que vous êtes absent.
> Quand vous rentrez → Le plat est prêt ! ✅

---

**Accéder à User Data**

```
1. Faire défiler jusqu'à "Advanced details"
2. Cliquer pour développer
3. Faire défiler jusqu'à "User data"
```

---

### Étape 2.3 : Le Script User Data

**Copier-coller ce script :**

```bash
#!/usr/bin/env bash
set -e

# [1] Install Node.js
curl -fsSL https://rpm.nodesource.com/setup_21.x | bash -
yum install -y nodejs

# [2] Write the sample app code to a file called app.js
tee app.js > /dev/null << "EOF"
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!\n');
});

# [3] Listen on port 80
const port = process.env.PORT || 80;
server.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
EOF

# [4] Run the app in the background
nohup node app.js &
```

---

### Étape 2.4 : Lancement

```
1. ✅ Vérifier tous les paramètres
2. ✅ Script User Data collé
3. ✅ "Allow HTTP traffic" COCHÉ
4. ✅ Tous les autres paramètres par défaut
5. 🚀 Cliquer "Launch instance"
```

**Confirmation**

```
✅ "Successfully launched instance"
Instance ID : i-xxxxxxxxxxxxxxxx
```

**Actions**

```
Cliquer sur l'Instance ID → Page "Instances"
```

---

## 💻 Partie 3 : Comprendre le Code

### Architecture de l'Application

```
┌─────────────────────────────────┐
│    Internet (Clients)           │
└────────────┬────────────────────┘
             │ HTTP Request
             ↓
┌────────────────────────────────────┐
│  AWS Security Group (Firewall)     │
│  Règle : HTTP (port 80) autorisé   │
└────────────┬───────────────────────┘
             │
             ↓
┌────────────────────────────────────┐
│      Instance EC2                  │
│  ┌──────────────────────────────┐  │
│  │  Amazon Linux 2023           │  │
│  │  ┌────────────────────────┐  │  │
│  │  │  Node.js Runtime       │  │  │
│  │  │  ┌──────────────────┐  │  │  │
│  │  │  │  app.js          │  │  │  │
│  │  │  │  Serveur HTTP    │  │  │  │
│  │  │  │  Port 80         │  │  │  │
│  │  │  └──────────────────┘  │  │  │
│  │  └────────────────────────┘  │  │
│  └──────────────────────────────┘  │
└────────────┬───────────────────────┘
             │ HTTP Response
             │ "Hello, World!"
             ↓
┌────────────────────────────────────┐
│   Navigateur (Client)              │
│   Affiche : "Hello, World!"        │
└────────────────────────────────────┘
```

---

### Explication Détaillée du Script

#### Partie 1 : Shebang et Options

```bash
#!/usr/bin/env bash
set -e
```

| Ligne | Signification |
|-------|---------------|
| `#!/usr/bin/env bash` | Exécute avec Bash |
| `set -e` | Arrête si erreur |

> **Analogie** : `set -e` = "Si tu rates une étape, arrête tout"

---

#### Partie 2 : Installation Node.js

```bash
# [1] Install Node.js
curl -fsSL https://rpm.nodesource.com/setup_21.x | bash -
yum install -y nodejs
```

**Ligne par ligne :**

**`curl -fsSL https://rpm.nodesource.com/setup_21.x | bash -`**

| Partie | Signification |
|--------|---------------|
| `curl` | Télécharge depuis Internet |
| `-fsSL` | Options (fail, silent, follow, show errors) |
| `URL` | Script de config Node.js 21 |
| `\| bash -` | Exécute le script téléchargé |

**`yum install -y nodejs`**

| Partie | Signification |
|--------|---------------|
| `yum` | Gestionnaire de paquets (Amazon Linux) |
| `install` | Commande d'installation |
| `-y` | "yes" automatique |
| `nodejs` | Paquet à installer |

---

#### Partie 3 : Création app.js

```bash
# [2] Write the sample app code
tee app.js > /dev/null << "EOF"
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!\n');
});

const port = process.env.PORT || 80;
server.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
EOF
```

**Syntaxe Bash :**

| Partie | Signification |
|--------|---------------|
| `tee app.js` | Écrit dans fichier app.js |
| `> /dev/null` | Pas d'affichage écran |
| `<< "EOF"` | Heredoc (contenu multiligne) |
| `EOF` | Marqueur de fin |

---

### Code Node.js Expliqué

#### Import du Module HTTP

```javascript
const http = require('http');
```

- `require('http')` : Importe module HTTP de Node.js
- Module inclus par défaut (pas besoin d'installer)

---

#### Création du Serveur

```javascript
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!\n');
});
```

**Explication :**

| Partie | Signification |
|--------|---------------|
| `http.createServer()` | Crée un serveur HTTP |
| `(req, res) =>` | Fonction callback (à chaque requête) |
| `req` | Request (infos requête) |
| `res` | Response (pour répondre) |

**Écriture de la Réponse :**

```javascript
res.writeHead(200, { 'Content-Type': 'text/plain' });
```

| Partie | Signification |
|--------|---------------|
| `res.writeHead()` | Écrit l'en-tête HTTP |
| `200` | Code HTTP OK (succès) |
| `'Content-Type': 'text/plain'` | Type : texte brut |

**Codes HTTP Courants :**

| Code | Signification |
|------|---------------|
| 200 | OK |
| 404 | Not Found |
| 500 | Server Error |
| 301 | Redirect |

```javascript
res.end('Hello, World!\n');
```

- Termine la réponse
- Envoie le contenu au client

---

#### Configuration du Port

```javascript
const port = process.env.PORT || 80;
server.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
```

**Ligne 1 :**

| Partie | Signification |
|--------|---------------|
| `process.env.PORT` | Variable d'environnement PORT |
| `\|\|` | Opérateur "OU" |
| `80` | Port par défaut |

**Pourquoi port 80 ?**

```
Port 80 = Port HTTP standard
→ http://example.com (port 80 implicite)
→ Pas besoin de spécifier :80

Autres ports courants :
- 443 : HTTPS
- 22 : SSH
- 3306 : MySQL
- 5432 : PostgreSQL
```

**Ligne 2-4 :**

```javascript
server.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
```

- `server.listen()` : Démarre le serveur
- Callback appelé quand prêt
- Template literal : `` `Listening on port ${port}` ``

---

#### Partie 4 : Lancement de l'App

```bash
# [4] Run the app in the background
nohup node app.js &
```

| Commande | Signification |
|----------|---------------|
| `nohup` | "No Hang Up" (continue après fin script) |
| `node app.js` | Lance l'application |
| `&` | Arrière-plan |

**Pourquoi nohup & ?**

```
Sans :
node app.js
→ Script attend indéfiniment
→ Si script se termine → App s'arrête

Avec :
nohup node app.js &
→ App démarre en arrière-plan
→ Script se termine
→ App continue ✅
```

---

### Timeline d'Exécution

```
00:00 - Instance démarre (Pending)
00:30 - Boot système (Amazon Linux)
01:00 - AWS exécute User Data
  [1] Installation Node.js (~45s)
  [2] Création app.js (~1s)
  [3] Configuration port (~1s)
  [4] Lancement app (~1s)
02:00 - Instance Running
02:30 - Application prête ✅
```

**Temps total : ~2-3 minutes**

---

### Flux d'une Requête HTTP

```
1. Client envoie : GET / HTTP/1.1
   ↓
2. Serveur reçoit la requête
   ↓
3. Callback (req, res) est appelé
   ↓
4. res.writeHead(200, ...) → En-tête
   ↓
5. res.end('Hello, World!\n') → Contenu
   ↓
6. Client affiche : Hello, World!
```

---

## ✅ Partie 4 : Test & Vérification

### Étape 4.1 : Attendre le Démarrage

**Observer l'État**

```
EC2 → Instances → Cliquez sur votre instance
```

**États Possibles :**

| État | Signification | Action |
|------|---------------|--------|
| **Pending** | ⏳ Démarrage | Attendre |
| **Running** | ✅ Active | Prêt à tester |
| **Stopped** | ⏹️ Arrêtée | Redémarrer |
| **Terminated** | ❌ Supprimée | Irrécupérable |

---

### Étape 4.2 : Récupérer l'IP Publique

**Où ?**

```
1. Page EC2 Instances
2. Cliquez sur votre instance
3. Tiroir en bas
4. Cherchez : "Public IPv4 address"
```

**Exemple :**

```
Public IPv4 address : 13.51.169.94
```

> ⚠️ **Votre IP sera différente !**

---

### Étape 4.3 : Tester l'Application

**Dans votre navigateur :**

```
1. Copier l'IP publique
2. Ouvrir navigateur (Chrome, Firefox, etc.)
3. Taper : http://13.51.169.94 (VOTRE IP)
   
⚠️ IMPORTANT : Taper "http://" explicitement !

4. Appuyer sur Entrée
```

**⚠️ HTTP vs HTTPS**

```
✅ CORRECT : http://13.51.169.94
❌ ERREUR :  https://13.51.169.94 (avec 's')
❌ ERREUR :  13.51.169.94 (sans http://)
```

**Pourquoi ?**

```
Navigateurs modernes → Tentent HTTPS par défaut
Notre app → Pas de certificat SSL
HTTPS → Ne fonctionnera pas

Solution → Taper "http://" explicitement
```

---

### 🎉 Résultat Attendu

```
Hello, World!
```

**FÉLICITATIONS !** 🎊

```
✅ Application déployée sur AWS !
✅ Serveur accessible sur Internet
✅ Bases du cloud et DevOps maîtrisées
```

---

## 🏋️ Exercices Pratiques

### Exercice 1 : Test de Redémarrage

#### Objectif

Comprendre les limites du User Data et la nécessité d'une gestion de processus.

#### Actions

```
1. EC2 → Instances
2. Sélectionner instance (checkbox)
3. Instance state → Reboot instance
4. Confirmer
5. Attendre ~1 minute
6. Tester : http://VOTRE_IP
```

#### Question

**L'application fonctionne-t-elle après le reboot ?**

<details>
<summary>💡 Cliquez pour voir la réponse</summary>

### Réponse : ❌ NON

**Pourquoi ?**

```
User Data s'exécute UNE SEULE FOIS
→ Au premier boot uniquement
→ Pas aux redémarrages suivants

Après reboot :
✅ Serveur redémarre
✅ Node.js installé
✅ Fichier app.js existe
❌ Application NON relancée

Raison :
→ "nohup node app.js &" exécuté qu'au 1er boot
→ Processus s'arrête au reboot
→ Aucun mécanisme de relance automatique
```

### Solution Professionnelle

**Option 1 : Systemd Service**

```bash
# Créer /etc/systemd/system/sample-app.service
[Unit]
Description=Sample Node.js App
After=network.target

[Service]
Type=simple
User=ec2-user
WorkingDirectory=/home/ec2-user
ExecStart=/usr/bin/node /home/ec2-user/app.js
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable sample-app
sudo systemctl start sample-app
```

**Option 2 : PM2 (Process Manager)**

```bash
npm install -g pm2
pm2 start app.js
pm2 startup
pm2 save
```

**Option 3 : Docker**

```dockerfile
FROM node:21-alpine
COPY app.js .
CMD ["node", "app.js"]
```

> 📚 **Le Chapitre 3 du livre couvre ces solutions !**

### Leçon

```
📚 Problèmes de l'approche simplifiée :
   - Pas de gestion de processus
   - Pas de survie au redémarrage
   - Pas de redémarrage auto en cas de crash
   - Pas de monitoring
   - Pas de logs centralisés

→ OK pour APPRENDRE
→ JAMAIS en production !
```

</details>

---

### Exercice 2 : Explorer CloudWatch

#### Objectif

Comprendre les outils de monitoring AWS et comparer avec PaaS.

#### Actions

**1. Accéder à CloudWatch**

```
AWS Console → "CloudWatch" → Cliquer
```

**2. Voir Métriques**

```
CloudWatch
→ Metrics
→ All metrics
→ EC2
→ Per-Instance Metrics
```

**3. Sélectionner Instance**

```
Chercher : i-xxxxxxxxxxxxxxxx
Cocher :
☑️ CPUUtilization
☑️ NetworkIn
☑️ NetworkOut
```

**4. Observer Graphiques**

```
Période : Last 1 hour
Intervalle : 1 minute
Type : Line
```

**5. Générer Trafic**

```
1. Rafraîchir http://VOTRE_IP plusieurs fois (F5)
2. Retourner CloudWatch
3. Attendre 1-2 minutes
4. Rafraîchir graphique
5. Observer augmentation NetworkIn/Out
```

---

#### Comparaison CloudWatch vs PaaS

<details>
<summary>💡 Cliquez pour voir la comparaison</summary>

| Aspect | AWS CloudWatch | Render/Heroku |
|--------|----------------|---------------|
| **Configuration** | ❌ Manuelle | ✅ Automatique |
| **Logs app** | ❌ Nécessite Agent | ✅ Immédiat |
| **Métriques système** | ✅ Très détaillées | ✅ Basiques |
| **Métriques custom** | ✅ Possibles (SDK) | ⚠️ Limitées |
| **Alertes** | ✅ Très flexibles | ✅ Basiques |
| **Courbe apprentissage** | ⚠️ Élevée | ✅ Simple |
| **Coût** | 💰 Pay-per-use | 💰 Inclus |
| **Rétention logs** | ⚠️ Configurable (coûte) | ✅ 7-30 jours |

### Philosophies

**AWS (IaaS)** : "Vous gérez tout"

```
✅ Contrôle total
✅ Flexibilité maximale
✅ Moins cher à grande échelle
❌ Configuration complexe
❌ Courbe d'apprentissage importante
```

**Render/Heroku (PaaS)** : "Nous gérons l'infra"

```
✅ Déploiement en clics
✅ Logs/monitoring auto
✅ Focus sur le code
❌ Moins de contrôle
❌ Plus cher à grande échelle
```

### Trade-off

```
IaaS (AWS EC2) :
→ Plus de contrôle = Plus de responsabilité
→ Moins cher long terme
→ Courbe apprentissage

PaaS (Render/Heroku) :
→ Moins de contrôle = Moins de responsabilité
→ Plus cher long terme
→ Simplicité
```

</details>

---

## 🔧 Dépannage

### Problème 1 : Connection Timeout

#### Symptôme

```
"Le délai d'attente est dépassé"
"Le serveur met trop de temps à répondre"
```

#### Causes

| Cause | Probabilité | Solution |
|-------|-------------|----------|
| Security Group bloque HTTP | ⭐⭐⭐⭐⭐ | Ajouter règle HTTP |
| Utilisé HTTPS au lieu de HTTP | ⭐⭐⭐⭐ | Utiliser `http://` |
| Instance pas prête | ⭐⭐⭐ | Attendre 3-4 min |
| Erreur User Data | ⭐⭐ | Vérifier logs |

---

#### Solution : Vérifier Security Group

**Étapes :**

```
1. EC2 → Instances
2. Cliquez sur instance
3. Onglet "Security"
4. Section "Security groups"
5. Cliquez sur nom du groupe
```

**Vérifier Règles Entrantes**

```
Section : "Inbound rules"
```

**Vous DEVEZ voir :**

| Type | Protocol | Port | Source |
|------|----------|------|--------|
| HTTP | TCP | 80 | 0.0.0.0/0 |

**Si règle absente :**

```
1. "Edit inbound rules"
2. "Add rule"
3. Type : HTTP
4. Source : 0.0.0.0/0
5. "Save rules"
6. Attendre 10-20s
7. Réessayer http://VOTRE_IP
```

---

### Problème 2 : ERR_SSL_PROTOCOL_ERROR

#### Symptôme

```
"This site can't provide a secure connection"
"ERR_SSL_PROTOCOL_ERROR"
```

#### Cause

Vous avez utilisé `https://` au lieu de `http://`

#### Solution

```
✅ http://VOTRE_IP
❌ https://VOTRE_IP
```

---

### Problème 3 : Instance Running mais App Inaccessible

#### Vérifications

**1. Status Checks**

```
Onglet "Status checks"
→ Devrait montrer : ✅ 2/2 checks passed
```

**2. User Data**

```
Actions → Instance settings → View/Change User Data
→ Vérifier que le script est présent
```

**3. Logs (Nécessite SSH)**

Si vous avez configuré une key pair :

```bash
ssh -i votre-cle.pem ec2-user@VOTRE_IP
cat /var/log/cloud-init-output.log
```

---

## 🧹 Nettoyage

### ⚠️ IMPORTANT : Éviter les Frais

**Toujours terminer l'instance après test !**

```
Instance qui tourne = FACTURATION
→ Même si vous ne l'utilisez pas
→ ~0.01€/heure ≈ 7€/mois
→ Peut s'accumuler rapidement
```

### Procédure

```
1. EC2 → Instances
2. Sélectionner instance (checkbox)
3. Instance state → Terminate instance
4. Confirmer la suppression
```

**Confirmation**

```
État passe à : Terminated
→ Instance définitivement supprimée
→ Facturation arrêtée ✅
```

---

## 🎓 Conclusion

### Ce Que Vous Avez Appris

```
✅ Fondamentaux du Cloud Computing
   → IaaS vs PaaS
   → Régions et zones de disponibilité
   → Pay-as-you-go

✅ Sécurité Cloud
   → IAM (utilisateurs, policies)
   → MFA (authentification multi-facteurs)
   → Security Groups (pare-feu)

✅ Infrastructure AWS
   → EC2 (serveurs virtuels)
   → VPC (réseaux virtuels)
   → AMI (images système)

✅ Automatisation
   → User Data (scripts de démarrage)
   → Infrastructure as Code (concepts)

✅ DevOps
   → Software delivery
   → Déploiement automatisé
   → Monitoring (CloudWatch)

✅ Développement Web
   → Architecture client-serveur
   → Protocole HTTP
   → Node.js / JavaScript
```

---

### Compétences DevOps Acquises

```
🔐 Security
   → Principe du moindre privilège
   → Authentification multi-facteurs
   → Gestion des identités (IAM)

🖥️ Infrastructure
   → Provisioning de serveurs
   → Configuration réseau
   → Gestion de firewall

📊 Monitoring
   → Métriques système
   → Logs d'application
   → Alertes

🔄 Automation
   → Scripts de déploiement
   → Configuration automatisée
   → Infrastructure as Code (initiation)

🚀 Deployment
   → Déploiement cloud
   → Gestion d'environnements
   → Cycle de vie des applications
```

---

### Prochaines Étapes

#### 🌟 Niveau Débutant

- [ ] Déployer une vraie app (Express.js, React)
- [ ] Configurer HTTPS avec Certificate Manager
- [ ] Utiliser un nom de domaine (Route 53)
- [ ] Ajouter une base de données (RDS)

#### 🚀 Niveau Intermédiaire

- [ ] Load Balancer pour haute disponibilité
- [ ] Auto Scaling pour scalabilité
- [ ] CI/CD avec GitHub Actions
- [ ] Containers avec Docker

#### 💎 Niveau Avancé

- [ ] Kubernetes (EKS)
- [ ] Infrastructure as Code (Terraform)
- [ ] Microservices
- [ ] Observabilité complète

---

### Pourquoi Ces Compétences Sont Cruciales

#### 📈 Tendances du Marché

```
94% des entreprises utilisent le cloud (Flexera 2023)
→ Demande croissante pour compétences cloud/DevOps

Salaires moyens (France, 2025) :
→ DevOps Engineer : 70-120k€/an
→ Cloud Architect : 80-150k€/an
→ Site Reliability Engineer : 75-130k€/an

Top 3 compétences tech recherchées (LinkedIn) :
1. Cloud Computing (AWS, Azure, GCP)
2. DevOps / CI/CD
3. Containers / Kubernetes
```

---

#### 🏢 Adoption Entreprise

```
Fortune 500 sur AWS :
→ Netflix (streaming à l'échelle mondiale)
→ Airbnb (millions de réservations/jour)
→ Spotify (200M+ utilisateurs)
→ NASA (recherche scientifique)

Pourquoi ?
✅ Scalabilité instantanée
✅ Disponibilité mondiale
✅ Pay-as-you-go (coûts optimisés)
✅ Innovation rapide
```

---

### 🎯 Méthodologie d'Apprentissage

**Ce tutoriel a suivi :**

```
1. Learning by Doing
   → Pas seulement théorie
   → Pratique immédiate
   → Projet concret

2. Progressive Disclosure
   → Concepts simples d'abord
   → Complexité progressive
   → Contexte pour chaque étape

3. Analogies & Visualisations
   → Concepts abstraits rendus concrets
   → Diagrammes d'architecture
   → Comparaisons avec monde réel

4. Error-Driven Learning
   → Anticiper les erreurs courantes
   → Comprendre POURQUOI ça ne marche pas
   → Développer réflexes de débogage
```

---

### 💡 Principes DevOps Illustrés

```
Automation :
→ User Data automatise installation
→ Élimine erreurs manuelles
→ Reproductibilité

Infrastructure as Code :
→ Configuration déclarative
→ Versionnable (Git)
→ Auditable

Security by Design :
→ IAM (permissions granulaires)
→ MFA (sécurité multi-couches)
→ Security Groups (zero-trust)

Monitoring & Observability :
→ CloudWatch (métriques)
→ Logs centralisés
→ Alertes proactives

Continuous Improvement :
→ Exercices révèlent limitations
→ Identification de problèmes
→ Évolution vers meilleures pratiques
```

---

## 📚 Ressources

### Documentation Officielle

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Node.js Documentation](https://nodejs.org/docs/)
- [AWS Free Tier](https://aws.amazon.com/free/)

### Tutoriels Recommandés

- [AWS Getting Started](https://aws.amazon.com/getting-started/)
- [Node.js Guides](https://nodejs.org/en/docs/guides/)
- [DevOps Roadmap](https://roadmap.sh/devops)

### Livres

- *Fundamentals of DevOps and Software Delivery* (source de ce tutoriel)
- *AWS Certified Solutions Architect Study Guide*
- *The DevOps Handbook*

### Cours en Ligne

- [AWS Skill Builder](https://skillbuilder.aws/) (Gratuit)
- [Linux Academy / A Cloud Guru](https://acloudguru.com/)
- [Udemy - AWS Certified Developer](https://www.udemy.com/)

### Communautés

- [AWS Community](https://aws.amazon.com/developer/community/)
- [r/aws (Reddit)](https://www.reddit.com/r/aws/)
- [Stack Overflow - AWS Tag](https://stackoverflow.com/questions/tagged/amazon-web-services)

---

## 📝 Notes Finales

### Sauvegardez Ce Projet

**Créer un dépôt GitHub :**

```bash
# Sur votre machine locale
mkdir aws-ec2-hello-world
cd aws-ec2-hello-world

# Créer les fichiers
echo "# AWS EC2 Hello World Deployment" > README.md
nano user-data.sh  # Coller le script User Data
nano app.js        # Coller le code Node.js

# Initialiser Git
git init
git add .
git commit -m "Initial commit - AWS EC2 deployment tutorial"

# Pousser sur GitHub
git remote add origin https://github.com/VOTRE-USERNAME/aws-ec2-hello-world.git
git push -u origin main
```

---

### Structure du Projet

```
aws-ec2-hello-world/
├── README.md           (Ce fichier)
├── user-data.sh        (Script de déploiement)
├── app.js              (Application Node.js)
├── docs/
│   ├── screenshots/    (Captures d'écran)
│   └── diagrams/       (Diagrammes d'architecture)
└── .gitignore
```

---

### Évolutions Futures

**Version 2.0 (À Implémenter) :**

- [ ] Systemd service pour persistence
- [ ] HTTPS avec Let's Encrypt
- [ ] Logging avec CloudWatch Logs
- [ ] Health checks avancés
- [ ] Load Balancer + Auto Scaling
- [ ] CI/CD avec GitHub Actions

---

## 🙏 Remerciements

Ce tutoriel est basé sur **"Fundamentals of DevOps and Software Delivery"** et adapté pour un apprentissage pratique complet.

### Contributeurs

- **samba-diallo** - Documentation et adaptation
- Communauté AWS
- Communauté Node.js

---

## 📄 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## 📞 Contact & Support

- **GitHub** : [@samba-diallo](https://github.com/samba-diallo)
- **Issues** : [Ouvrir une issue](https://github.com/samba-diallo/aws-ec2-hello-world/issues)

---

## ⭐ Si Ce Guide Vous a Aidé

```bash
# Donnez une étoile sur GitHub !
⭐ Star this repository

# Partagez avec d'autres débutants en DevOps
🔗 Share the knowledge

# Contribuez à l'améliorer
🤝 Pull requests welcome
```

---

<div align="center">

**Vous avez maintenant les bases du DevOps, de la livraison logicielle et du déploiement d'applications !**

🚀 **Continuez à apprendre, pratiquer et construire !** 🚀

---

*Dernière mise à jour : 27 octobre 2025*

</div>
