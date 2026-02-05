# 🚀 Projet Bibox : Infrastructure Cloud Haute Disponibilité

Ce dépôt contient l'étude et la conception d'une infrastructure cloud hyper-échelle pour **Pleasure**, une plateforme mondiale de streaming multimédia et d'e-commerce.

## 📝 Contexte et Objectifs

L'entreprise, basée à Dubaï, opère avec 15 collaborateurs en télétravail complet. Le projet vise à reconstruire une infrastructure capable de répondre aux enjeux suivants :

* **Charge de pointe :** Soutenir 1 million de visiteurs par heure.
* **Résilience absolue :** Garantir un RTO de 5 minutes et un RPO de 30 minutes.
* **Performance :** Résoudre les problèmes de latence, notamment pour les utilisateurs aux USA.
* **Sécurité :** Assurer un site sécurisé avec un minimum de coupures.

---

## 🏗️ Architecture Cible

L'architecture repose sur un modèle **Multi-Régions Actif-Actif** distribué sur trois plaques : Amérique du Nord, Europe et Asie de l'Est.

[Image of multi-region active-active cloud infrastructure diagram]

### 1. Stratégie Edge & Livraison
* **Entrée unique :** Utilisation de l'Anycast IP pour un basculement réseau en moins de 30 secondes.
* **Sécurité :** Cloudflare WAF pour bloquer les attaques DDoS et les "Scalper Bots".
* **CDN Spécialisés :** Partitionnement entre Akamai (Vidéo HD), Fastly (Logique Edge) et Cloudflare (API/Web).
* **Origin Shield :** Protection contre le phénomène de "Thundering Herd".

### 2. Dimensionnement Kubernetes (EKS sur AWS)
L'infrastructure est déployée sur **AWS** pour son expertise média et son outil de scaling **Karpenter**.

| Node Pool | Rôle Stratégique | Puissance VM (AWS) | Nb VM (Est.) |
| :--- | :--- | :--- | :--- |
| **NP_SYS** | Poste de commandement | m7g.large | 3 |
| **NP_CPU** | Logique métier, Shop | m7g.4xlarge | 250 |
| **NP_GPU** | Transcodage Live, IA | g5.2xlarge | 60 |
| **NP_SPOT** | Archivage, Logs | c7g.4xlarge | 40 |

---

## 🛠️ Concepts Technologiques

### Virtualisation vs Émulation vs Conteneurisation
* **Virtualisation :** Création d'une couche d'abstraction pour faire fonctionner plusieurs OS isolés sur une machine.
* **Émulation :** Imitation logicielle d'un matériel différent pour exécuter du code non natif.
* **Conteneurisation :** Partage du noyau de l'hôte pour isoler uniquement les applications (léger et rapide).

### Analyse des Hyperviseurs de Type 1
* **VMware vSphere (ESXi) :** Standard historique, mais coûteux depuis les changements de licence Broadcom.
* **Microsoft Hyper-V :** Idéal pour les entreprises utilisant déjà Windows Server.
* **Proxmox VE :** Alternative Open Source très performante sur le stockage avec ZFS/Ceph.

---

## 🛡️ Résilience et Continuité (DRP)
* **Data Layer :** Transactions SQL répliquées via le protocole Raft avec un RPO de 0.
* **Stockage :** Vidéos répliquées mondialement via S3 RTC en moins de 15 min.
* **Stratégie de sauvegarde :** Snapshots de VM et export bi-quotidien des bases de données vers un NAS.

---

## ⚙️ Automatisation et Maintenance
* **Ansible :** Configuration orchestrée par rôles (webserver, database, etc.) pour une gestion granulaire.
* **Scripting :** Scripts Bash/PowerShell pour les sauvegardes et les tests de santé (Healthcheck).

---
*Date du projet : 05/02/2026*
