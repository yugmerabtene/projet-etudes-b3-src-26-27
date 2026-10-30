# Conformité — RGPD & Politique de Sécurité

---

## 1. RGPD — Règlement Général sur la Protection des Données

### 1.1 Principes RGPD appliqués

| Principe | Application | Preuve |
|----------|-------------|--------|
| **Licéité** | Consentement explicite inscription/newsletter | Formulaire inscription + CGU |
| **Limitation des finalités** | Données collectées = livraison + compte | Politique confidentialité |
| **Minimisation** | Seules les données nécessaires collectées | Formulaire inscription |
| **Exactitude** | Modification profil par l’utilisateur | Endpoint API profil |
| **Limitation conservation** | Durée de conservation définie | Politique de rétention |
| **Intégrité et confidentialité** | Chiffrement + access control | Keycloak + TLS + LUKS + SOPS/age |
| **Responsabilité** | DPO désigné, registre de traitements | Document RGPD |

### 1.2 Registre des traitements

| Traitement | Finalité | Base légale | Données | Durée | Destinataires |
|-----------|----------|-------------|---------|-------|---------------|
| Gestion compte | Fourniture du service | Contractuelle | Nom, email, mot de passe, adresse | Durée compte + 1 an | Devincilab (hébergeur) |
| Commandes | Livraison produits | Contractuelle | Adresse livraison, téléphone | 5 ans (comptabilité) | Transporteur |
| Newsletter | Communication marketing | Consentement | Email | Jusqu’à désinscription | Haraka (auto-hébergé) |
| Paiement | Règlement commandes | Contractuelle | Carte bancaire (via Stripe uniquement) | Durée transaction | Stripe (PCI-DSS) |
| Logs sécurité | Prévention fraudes | Intérêt légitime | IP, User-Agent, dates | 12 mois | Wazuh |
| Analytics | Amélioration service | Intérêt légitime | Pages visitées, clics | 13 mois | Matomo (auto-hébergé) |

### 1.3 Droits des utilisateurs

| Droit | Mise en œuvre | Preuve |
|-------|---------------|--------|
| **Droit d’accès** (Art. 15) | Endpoint API : GET /api/me/export | Code API |
| **Droit de rectification** (Art. 16) | Endpoint API : PUT /api/me | Code API |
| **Droit à l’effacement** (Art. 17) | Endpoint API : DELETE /api/me + script anonymisation | Script Django |
| **Droit à la portabilité** (Art. 20) | Export JSON/CSV des données | Code API |
| **Droit d’opposition** (Art. 21) | Désinscription newsletter automatique | Celery task |
| **Droit à la limitation** (Art. 18) | Indicateur « compte limité » dans la base de données | Migration BDD |

### 1.4 Mesures techniques RGPD

| Mesure | Technologie | Documentation |
|--------|------------|---------------|
| Chiffrement at rest | LUKS (disques) + pgcrypto (BDD) | `infra/security/luks.md` |
| Chiffrement transit | TLS 1.3 géré par Caddy (Let’s Encrypt) | Config Caddy |
| Chiffrement secrets | SOPS + age (fichiers chiffrés) | `infra/security/sops/` |
| Anonymisation | Script de pseudonymisation des logs | `scripts/anonymize_logs.py` |
| Consentement | Formulaire double opt-in | Django model Consent |
| Suppression | Tâche Celery de nettoyage automatique | `app/backend/tasks/gdpr_cleanup.py` |
| Audit | Wazuh audit trail complet | Config Wazuh |
| Sauvegarde sécurisée | BorgBackup chiffré (cle age) | `infra/backup/` |
| PRA | Procédure restauration documentée | `docs/BC03/procedures.md` |

### 1.5 Plan de conformité RGPD

| Action | Responsable | Échéance | Statut |
|--------|-------------|----------|--------|
| Désignation DPO | Direction | Sept 2026 | À faire |
| Registre traitements | DPO + Dev | Oct 2026 | À faire |
| Politique de confidentialité | DPO + Legal | Oct 2026 | À faire |
| Mise en place consentement | Dev Backend | Déc 2026 | À faire |
| Droits utilisateurs (API) | Dev Backend | Fév 2027 | À faire |
| Formation équipe RGPD | DPO | Jan 2027 | À faire |
| Audit interne | DPO + Admin | Mars 2027 | À faire |
| Test exercice droits | DPO + Dev | Avril 2027 | À faire |
| Revue annuelle | DPO | Juin 2027 | À faire |

---

## 2. Politique de Sécurité de l’Information

> Document exigé par BC03.9 (compétence officielle BC03.4) — Référence aux normes applicables (ISO 27001 comme standard de bonnes pratiques, RGPD comme obligation légale)

### 2.1 Gestion des accès et authentification

| Règle | Mise en œuvre | Preuve |
|-------|---------------|--------|
| Principe du moindre privilège | Keycloak RBAC avec rôles définis | Config Keycloak |
| Authentification forte | MFA obligatoire pour admins | Keycloak MFA |
| Politique de mots de passe | 12+ caractères, complexité, rotation 90j | Keycloak policy |
| Gestion des comptes | Comptes personnels, pas de comptes partagés | Procédure |
| Révocation des accès | Désactivation immédiate en cas de départ | Procédure RH |
| Sessions | Timeout 30min inactivité, refresh token limité | Config API |

### 2.2 Chiffrement des données

| Périmètre | Technologie | Standard |
|-----------|------------|----------|
| Disques (at rest) | LUKS | AES-256 |
| Base de données (at rest) | pgcrypto | AES-256 |
| Secrets (fichiers) | SOPS + age | X25519 + AES-256-GCM |
| Transit réseau | TLS 1.3 (Let’s Encrypt) | RSA 2048+ / ECDSA |
| Sauvegardes | BorgBackup chiffré | AES-256-CTR + HMAC |

### 2.3 Sécurité réseau et pare-feu

| Règle | Mise en œuvre | Preuve |
|-------|---------------|--------|
| Pare-feu host | nftables (politique DENY par défaut) | Config nftables |
| Segmentation réseau | 4 VLANs (DMZ, App, Data, Admin) | Schéma architecture |
| Ports ouverts | Uniquement 80/443 vers l’extérieur | Règles nftables |
| Accès SSH | Clés certifiées, port custom, pas de password | Config SSH |
| Anti-brute force | Fail2ban (SSH, HTTP, API) | Config Fail2ban |
| WAF | ModSecurity + OWASP CRS | Config ModSecurity |

### 2.4 Surveillance et audit

| Règle | Mise en œuvre | Preuve |
|-------|---------------|--------|
| Logs centralisés | Loki + Promtail (tous les services) | Config Loki |
| Métriques | Prometheus + Grafana (CPU, RAM, disque, réseau) | Dashboard Grafana |
| SIEM | Wazuh (corrélation d’événements sécurité) | Config Wazuh |
| Alertes | Alertmanager (seuils CPU, RAM, disque, service down) | Config Alertmanager |
| Uptime | Uptime Kuma (vérification HTTP périodique) | Config Uptime Kuma |
| Audit trail | Wazuh + logs PostgreSQL (qui a fait quoi, quand) | Config Wazuh |
| IDS/IPS | Suricata (détection d’intrusions réseau) | Config Suricata |
| Scan vulnérabilités | OpenVAS (scan mensuel) | Rapports OpenVAS |

### 2.5 Sauvegarde et restauration

| Règle | Mise en œuvre | Fréquence |
|-------|---------------|-----------|
| Backup BDD | BorgBackup (PostgreSQL dump + chiffrage) | Quotidien |
| Backup fichiers | BorgBackup (Garage data, configs) | Quotidien |
| Rétention sauvegardes | 7 jours (quotidien), 4 semaines (hebdo), 12 mois (mensuel) | — |
| Backup externe | Restic vers NAS externe chiffré | Hebdomadaire |
| Test restauration | Procédure testée et documentée | Mensuel |
| PRA | Procédure de reprise documentée | `docs/BC03/procedures.md` |

### 2.6 Gestion des incidents

| Règle | Mise en œuvre | Preuve |
|-------|---------------|--------|
| Détection | Wazuh + Suricata + Fail2ban | Config outils |
| Escalade | Procédure de notification (admin → DPO → direction) | `docs/BC03/erreurs-recurrentes.md` |
| Analyse racine | Root Cause Analysis pour chaque incident majeur | Document par incident |
| Communication | Notification client sous 72h si fuite de données (RGPD Art. 33) | Procédure |
| Capitalisation | Retour d’expérience documenté après chaque incident | Leçons apprises |

### 2.7 Gestion des correctifs (patch management)

| Règle | Mise en œuvre | Fréquence |
|-------|---------------|-----------|
| OS | Mises à jour sécurité Ubuntu 22.04 | Mensuel |
| Applications | Mises à jour dépendances (Renovate/Dependabot) | Continu |
| Sécurité | Correctifs critiques appliqués sous 48h | À réception |
| Test | Tests en pré-production avant application en production | À chaque mise à jour |
| Rollback | Procédure de retour arrière documentée | À chaque mise à jour |

---

## 3. Analyse des Risques Sécurité

> Exigée par BC04.8 — « Grille d’analyse des risques (menace, impact, contre-mesure) » avec références aux normes (ISO 27005, RGPD)

| ID | Menace | Impact (1-5) | Probabilité (1-5) | Risque (I×P) | Contre-mesure | Statut | Réf. |
|----|--------|:---:|:---:|:---:|--------------|--------|------|
| R01 | Accès non autorisé aux données clients | 5 | 3 | **15** | Keycloak RBAC + MFA + audit logs Wazuh | Traité | RGPD Art.32 |
| R02 | Attaque DDoS sur la plateforme | 4 | 3 | **12** | ModSecurity WAF + rate limiting + Fail2ban | Traité | ISO 27005 |
| R03 | Fuite de données personnelles | 5 | 2 | **10** | Chiffrement LUKS/pgcrypto + RGPD Art.33 | Traité | RGPD Art.33-34 |
| R04 | Panne matérielle serveur principal | 5 | 2 | **10** | RAID + BorgBackup + PRA documenté | Traité | ISO 27005 |
| R05 | Attaque par injection SQL | 4 | 3 | **12** | ORM (SQLAlchemy) + prepared statements + WAF | Traité | OWASP Top 10 |
| R06 | Compromission clé SSH | 4 | 2 | **8** | Clés certifiées + Fail2ban + rotation | Traité | ISO 27005 |
| R07 | Ransomware / chiffrement données | 5 | 2 | **10** | Backup immutable (BorgBackup) + segmentation VLAN | Traité | ISO 27005 |
| R08 | Exploitation CVE dépendances | 3 | 4 | **12** | Renovate/Dependabot + scans OpenVAS mensuels | En cours | ISO 27005 |
| R09 | Indisponibilité prolongée (>4h) | 4 | 2 | **8** | HA (2 serveurs) + PRA + Uptime Kuma alertes | Traité | SLA |
| R10 | Non-conformité RGPD (amende) | 5 | 2 | **10** | DPO + registre traitement + consentement | Traité | RGPD Art.83 |
