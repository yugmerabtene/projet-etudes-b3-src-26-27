# Comparatif Cloud Propriétaire vs On-Premise Open Source

## Objectif

Étude comparative pour justifier le choix du cloud on-premise open source d’un point de vue technique, financier et stratégique.

---

## 1. Comparatif technique

### 1.1 Compute (Serveurs)

| Critère | AWS EC2 | Azure VM | On-Prem (Proxmox + KVM) |
|---------|---------|----------|-------------------------|
| Coût initial | €€€ (mensuel) | €€€ (mensuel) | €€€€ (investissement) |
| Coût récurrent | Élevé (horaires) | Élevé (horaires) | Faible (électricité + maintenance) |
| Scalabilité | Instantanée | Instantanée | Limitée (capacité physique) |
| Maîtrise matérielle | Non | Non | **Oui** |
| Localisation données | Configurable (régions) | Configurable (régions) | **Totale** (on-premise) |
| Latence réseau | Variable | Variable | **Faible** (réseau local) |

### 1.2 Base de données

| Critère | Amazon RDS | Azure PostgreSQL | PostgreSQL on-premise |
|---------|-----------|------------------|----------------------|
| Backup auto | Oui | Oui | **BorgBackup** (configurable) |
| Réplication | Multi-AZ | HA config | Streaming replication |
| Performances | Limitées par instance | Limitées par tier | **Maximum** (matériel dédié) |
| Coût/mois (100GB) | ~150-300 € | ~150-300 € | Électricité (~10-20 €) |
| Chiffrement | KMS | TDE | pgcrypto + LUKS |
| Conformité | Partagée | Partagée | **Souveraine** |

### 1.3 Stockage objets

| Critère | Amazon S3 | Azure Blob | Garage (Deuxfleurs) |
|---------|----------|------------|-------------------|
| API compatible | S3 | Blob | **S3-native** (compatibilité partielle) |
| Licence | Propriétaire | Propriétaire | **AGPL 3.0** (100 % open source) |
| Coût/GB/mois | ~0,023 € | ~0,018 € | Hardware uniquement |
| Durabilité | 99.999999999% | 99.999999999% | Dépend de la config (réplication) |
| Bande passante | Payante | Payante | **Gratuite** (réseau local) |
| Taille max objet | 5TB | 4.75TB | **Illimitée** |
| Souveraineté | Étrangère | Étrangère | **100 % française** (Deuxfleurs) |

### 1.4 Monitoring

| Critère | CloudWatch | Azure Monitor | Prometheus + Grafana |
|---------|-----------|---------------|---------------------|
| Coût (100 métriques) | ~30 €/mois | ~30 €/mois | **Gratuit** |
| Custom metrics | Payant | Payant | **Gratuit** |
| Dashboard | Basique | Basique | **Très riche** |
| Alertes | Payantes | Payantes | **Gratuites** (Alertmanager) |
| Logs | ~0,50 €/GB | ~0,50 €/GB | **Gratuit** (Loki) |
| Intégrations | 150+ | 100+ | **500+** (exporters) |

---

## 2. Comparatif financier (TCO sur 3 ans)

### Hypothèse
- Application e-commerce modérée
- 3 serveurs (applicatif, données, supervision) + 1 baie de stockage
- 100GB BDD, 500GB stockage images
- 100GB logs/mois, 200 métriques personnalisées

### Solution Cloud (AWS)

| Poste | Coût mensuel | Coût 3 ans |
|-------|-------------|------------|
| EC2 (4x m5.large) | 480 € | 17 280 € |
| RDS PostgreSQL (db.r5.large) | 280 € | 10 080 € |
| S3 (500GB) | 12 € | 432 € |
| CloudWatch | 100 € | 3 600 € |
| Data transfer | 50 € | 1 800 € |
| WAF + Shield | 50 € | 1 800 € |
| Certificats | 0 € (ACM) | 0 € |
| **Total AWS** | **~972 €/mois** | **~34 992 €** |

### Solution Cloud (Azure)

| Poste | Coût mensuel | Coût 3 ans |
|-------|-------------|------------|
| VM (4x Standard_B2s) | 420 € | 15 120 € |
| Azure Database PostgreSQL | 260 € | 9 360 € |
| Blob Storage (500GB) | 10 € | 360 € |
| Azure Monitor | 90 € | 3 240 € |
| Data transfer | 45 € | 1 620 € |
| WAF | 40 € | 1 440 € |
| Certificats | 0 € (Key Vault) | 0 € |
| **Total Azure** | **~865 €/mois** | **~31 140 €** |

### Solution On-Premise Open Source

| Poste | Coût initial | Coût 3 ans |
|-------|-------------|------------|
| Serveurs (3x Dell PowerEdge : applicatif, données, supervision) + baie de stockage | 12 000 € | 12 000 € |
| Électricité (4 machines, 350W) | — | 4 500 € |
| Maintenance (1j/mois admin) | — | 9 000 € |
| Réseau (switch, câbles) | 500 € | 500 € |
| Logiciels | **0 €** | **0 €** |
| | | |
| **Dont logiciels 100 % open source :** | | |
| Proxmox VE (virtualisation) | — | 0 € (AGPL) |
| Podman + Podman Compose (conteneurs) | — | 0 € (Apache 2.0) |
| Caddy (reverse proxy et TLS) + Nginx (frontend statique et WAF) | — | 0 € (Apache 2.0 / BSD) |
| PostgreSQL + Valkey (données) | — | 0 € (PostgreSQL / BSD) |
| Garage (stockage objets) | — | 0 € (AGPL) |
| Keycloak (IAM) | — | 0 € (Apache 2.0) |
| Prometheus + Grafana (monitoring) | — | 0 € (Apache 2.0 / AGPL) |
| Suricata + Wazuh (sécurité) | — | 0 € (GPLv2) |
| OpenTofu + Ansible (IaC) | — | 0 € (MPL 2.0 / GPLv3) |
| GitLab CE (service de l’infrastructure cliente : Git + CI/CD + registry + runner) | — | 0 € (MIT) |
| BorgBackup + Restic (backup) | — | 0 € (BSD) |
| SOPS + age (secrets) | — | 0 € (Mozilla 2.0 / BSD) |
| **Total On-Prem** | **12 500 €** | **26 000 €** |

### Synthèse financière

| Solution | Coût 1 an | Coût 3 ans |
|----------|----------|------------|
| AWS | 11 664 € | 34 992 € |
| Azure | 10 380 € | 31 140 € |
| **On-Prem Open Source** | **8 667 €** | **26 000 €** |

> **Économie sur 3 ans** : 5 140 € face à Azure (16,5 %) et 8 992 € face à AWS (25,7 %), avec une **souveraineté totale** des données. Le coût annuel de la solution sur site correspond à 26 000 € divisés par 3.

---

## 3. Comparatif souveraineté

| Critère | AWS/Azure | On-Premise |
|---------|-----------|------------|
| Localisation données | Configurable mais étranger | **100 % local** |
| Juridiction applicable | US CLOUD Act (AWS) / US law | **Droit français/UE** |
| Accès gouvernemental | Possible (CLOUD Act) | **Impossible** |
| Dépendance au fournisseur | Forte (verrouillage propriétaire) | **Nulle** |
| Maîtrise des mises à jour | Partielle | **Totale** |
| Audit de sécurité | Via le fournisseur | **Interne, maîtrisé** |
| Portabilité | Complexe (migration) | **Simple** (standard open source) |

---

## 4. Recommandation

### Choix retenu : Cloud On-Premise Open Source

**Justification :**

1. **Souveraineté numérique** : Aucune donnée hors du contrôle de l’entreprise
2. **Indépendance** : Pas de dépendance aux géants du cloud (AWS, Azure, GCP)
3. **Conformité** : Facilité de mise en conformité RGPD
4. **Coût** : Économie de 16,5 % face à Azure et de 25,7 % face à AWS sur 3 ans
5. **Performance** : Latence minimale, performances matérielles maximales
6. **Flexibilité** : Choix des logiciels, configuration libre
7. **Vision stratégique** : Adéquate pour un vendeur de matériel informatique/IA
8. **Licences** : 100 % open source, aucune contrainte légale (Podman, OpenTofu, Garage, Valkey…)

### Plan de migration proposé

| Étape | Description | Durée |
|-------|------------|-------|
| 1 | Inventaire matériel existant | 1 jour |
| 2 | Installation Proxmox + virtualisation | 2 jours |
| 3 | Déploiement conteneurs (Podman + Podman Compose) | 2 jours |
| 4 | Configuration sécurité (Suricata, Fail2ban, Wazuh) | 3 jours |
| 5 | Déploiement application (FastAPI + Vue.js) | 2 jours |
| 6 | Tests et validation | 2 jours |
| 7 | Documentation et formation | 2 jours |
| **Total** | | **~14 jours** |
