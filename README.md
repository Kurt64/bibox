# 🚀 Projet Bibox : Infrastructure Cloud Haute Disponibilité

Ce dépôt contient l'étude et la conception d'une infrastructure cloud hyper-échelle pour **Pleasure**, une plateforme mondiale de streaming multimédia et d'e-commerce[cite: 6].

## 📝 Contexte et Objectifs

[cite_start]L'entreprise, basée à Dubaï, opère avec 15 collaborateurs en télétravail complet[cite: 7, 8]. Le projet vise à reconstruire une infrastructure capable de répondre aux enjeux suivants :

* [cite_start]**Charge de pointe :** Soutenir 1 million de visiteurs par heure[cite: 59, 108].
* [cite_start]**Résilience absolue :** Garantir un RTO de 5 minutes et un RPO de 30 minutes[cite: 60, 109].
* [cite_start]**Performance :** Résoudre les problèmes de latence, notamment pour les utilisateurs aux USA[cite: 119].
* [cite_start]**Sécurité :** Assurer un site sécurisé avec un minimum de coupures[cite: 123, 124].

---

## 🏗️ Architecture Cible

[cite_start]L'architecture repose sur un modèle **Multi-Régions Actif-Actif** distribué sur trois plaques : Amérique du Nord, Europe et Asie de l'Est[cite: 61].


### 1. Stratégie Edge & Livraison
* [cite_start]**Entrée unique :** Utilisation de l'Anycast IP pour un basculement réseau en moins de 30 secondes[cite: 64].
* [cite_start]**Sécurité :** Cloudflare WAF pour bloquer les attaques DDoS et les "Scalper Bots"[cite: 65].
* [cite_start]**CDN Spécialisés :** Partitionnement entre Akamai (Vidéo HD), Fastly (Logique Edge) et Cloudflare (API/Web)[cite: 70, 71, 73].
* [cite_start]**Origin Shield :** Protection contre le phénomène de "Thundering Herd"[cite: 75].

### 2. Dimensionnement Kubernetes (EKS sur AWS)
[cite_start]L'infrastructure est déployée sur **AWS** pour son expertise média et son outil de scaling **Karpenter**[cite: 93, 94, 95].

| Node Pool | Rôle Stratégique | Puissance VM (AWS) | Nb VM (Est.) |
| :--- | :--- | :--- | :--- |
| **NP_SYS** | Poste de commandement | m7g.large | [cite_start]3 [cite: 80] |
| **NP_CPU** | Logique métier, Shop | m7g.4xlarge | [cite_start]250 [cite: 80] |
| **NP_GPU** | Transcodage Live, IA | g5.2xlarge | [cite_start]60 [cite: 80] |
| **NP_SPOT** | Archivage, Logs | c7g.4xlarge | [cite_start]40 [cite: 80] |

---

## 🛠️ Concepts Technologiques

### Virtualisation vs Émulation vs Conteneurisation
* [cite_start]**Virtualisation :** Création d'une couche d'abstraction pour faire fonctionner plusieurs OS isolés[cite: 13].
* [cite_start]**Émulation :** Imitation logicielle d'un matériel différent pour exécuter du code non natif[cite: 14].
* [cite_start]**Conteneurisation :** Partage du noyau de l'hôte pour isoler uniquement les applications (léger et rapide)[cite: 15].

### Analyse des Hyperviseurs de Type 1
* [cite_start]**VMware vSphere (ESXi) :** Standard historique, mais très onéreux depuis le rachat par Broadcom[cite: 23, 28].
* [cite_start]**Microsoft Hyper-V :** Idéal pour les entreprises déjà sous licence Windows Server[cite: 32, 42].
* [cite_start]**Proxmox VE :** Alternative Open Source montante, performante sur le stockage (ZFS/Ceph)[cite: 43, 47].

---

## 🛡️ Résilience et Continuité (DRP)
* [cite_start]**Data Layer :** Transactions SQL répliquées via le protocole Raft (RPO=0)[cite: 87].
* [cite_start]**Stockage :** Vidéos répliquées mondialement via S3 RTC (< 15 min)[cite: 89, 90].
* [cite_start]**Stratégie de sauvegarde :** Snapshots avant toute modification et export bi-quotidien des bases de données vers un NAS[cite: 143, 144].

---

## ⚙️ Automatisation et Maintenance
* [cite_start]**Ansible :** Orchestration de la configuration par rôles (webserver, database, etc.) pour une réutilisation granulaire[cite: 155, 156].
* [cite_start]**Scripting :** Utilisation de scripts Bash/PowerShell pour les sauvegardes et les tests de disponibilité (Healthcheck)[cite: 162, 163].

---
[cite_start]*Date de rendu : 05/02/2026 [cite: 3]*
