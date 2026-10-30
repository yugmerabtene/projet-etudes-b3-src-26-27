# Planning Prévisionnel — Sept 2026 à Juin 2027

## Vue d’ensemble

| Phase | Mois | Focus | Points RNCP |
|-------|------|-------|-------------|
| **Phase 1 — Cadrage** | Sept-Oct 2026 | Analyse besoins, CdC, veille, architecture | BC01.1, BC01.2, BC01.3, BC01.5, BC01.6, BC01.7 |
| **Phase 2 — Planification** | Nov 2026 | Gantt, ressources, risques, SLA, équipe | BC01.4, BC01.8, BC01.9, BC01.10, BC02.1, BC02.2, BC02.5 |
| **Phase 3 — Développement** | Déc 2026 — Fév 2027 | Dev app (FastAPI + Vue.js), Podman | BC03.1, BC03.2, BC03.3, BC03.5, BC03.6, BC03.7, BC03.8, BC03.9, BC02.3, BC04.9 |
| **Phase 4 — Infra & Sécurité** | Mars — Avr 2027 | Ansible, sécurité, IDS/IPS, monitoring | BC03.4, BC03.9, BC03.10, BC03.11, BC03.12, BC04.1, BC04.4, BC04.6, BC04.7, BC04.10 |
| **Phase 5 — On-Prem & Migration** | Mai 2027 | Déploiement on-prem, comparatif, migration | BC04.2, BC04.3, BC04.5, BC04.8, BC02.4 |
| **Phase 6 — Finalisation** | Juin 2027 | Tests, docs, soutenance, présentation client | BC02.4, BC02.6, BC02.7, BC02.8, BC03.5, BC03.6 |

---

## Détail par mois

### Septembre 2026 — Cadrage initial

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S1 | Entretien client materielsgaming.com (besoins) | Chef de projet | BC01.1 |
| S1 | Identification parties prenantes | Chef de projet | BC01.1 |
| S2 | Analyse fonctionnelle (besoins B2B/B2C) | Chef de projet | BC01.1 |
| S2 | Diagramme de contexte UML | Dev Backend | BC01.2 |
| S3 | Veille technologique (GPU, e-commerce, open source) | Dev Backend | BC01.7 |
| S3 | Veille concurrentielle | Dev Frontend | BC01.7 |
| S4 | Veille réglementaire (RGPD) | Chef de projet | BC01.7 |
| S4 | Synthèse veille documentée | Tous | BC01.7 |

### Octobre 2026 — Spécifications

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S5 | Rédaction specs fonctionnelles (SF-001→010) | Dev Backend | BC01.3 |
| S5 | Rédaction specs techniques (ST-001→008) | DevOps | BC01.3 |
| S6 | Cahier des charges complet | Chef de projet | BC01.3 |
| S6 | Architecture SI (schéma en couches) | DevOps | BC01.5 |
| S7 | Justification architecture + disponibilité | DevOps | BC01.5, BC01.6 |
| S7 | PRA/PCA définis | DevOps | BC01.6 |
| S8 | Validation client des spécifications | Chef de projet | BC01.3 |

### Novembre 2026 — Planification

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S9 | Choix méthodologie (Scrum justifié) | Chef de projet | BC01.4 |
| S9 | Création diagramme Gantt (15+ tâches) | Chef de projet | BC01.4, BC01.9 |
| S10 | Matrice des ressources | Chef de projet | BC01.8 |
| S10 | Matrice des risques (10 risques) | DevOps | BC01.8 |
| S11 | Plan d’atténuation SLA | DevOps | BC01.10 |
| S11 | Répartition rôles équipe | Chef de projet | BC02.2 |
| S12 | Planning communication | Chef de projet | BC02.5 |
| S12 | Configuration GitHub Issues et Actions CI | DevOps | BC02.1 |

### Décembre 2026 — Développement Sprint 1-2

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S13 | Setup projet : FastAPI + Vue.js + Podman Compose | DevOps | BC03.1, BC03.2 |
| S13 | Data models (SQLAlchemy + Django) | Dev Backend | BC03.1 |
| S14 | API Auth (Keycloak integration) | Dev Backend | BC03.1 |
| S14 | Frontend catalogue (Vue.js) | Dev Frontend | BC03.1 |
| S15 | API Produits + recherche | Dev Backend | BC03.1 |
| S15 | Podman Compose production (3 serveurs) | DevOps | BC03.2 |
| S16 | Tests fonctionnels Sprint 1 | Tous | BC03.5 |
| S16 | CR Sprint 1 | Chef de projet | BC02.3 |
| S16 | Configuration pipeline GitHub Actions | DevOps | BC04.9 |

### Janvier 2027 — Développement Sprint 3-4

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S17 | API Panier + Commandes | Dev Backend | BC03.1 |
| S17 | Frontend panier + checkout | Dev Frontend | BC03.1 |
| S18 | Intégration paiement Stripe | Dev Backend | BC03.1 |
| S18 | Admin dashboard (Django) | Dev Backend | BC03.1 |
| S19 | Celery tasks (emails, stock, RGPD) | Dev Backend | BC03.1 |
| S19 | Frontend admin | Dev Frontend | BC03.1 |
| S20 | Tests fonctionnels Sprint 3-4 | Tous | BC03.5 |
| S20 | Plan déploiement pas-à-pas | DevOps | BC03.3 |

### Février 2027 — Développement Sprint 5 + Tests

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S21 | RGPD API (droits utilisateurs) | Dev Backend | BC03.9 |
| S21 | Procédures erreurs récurrentes | DevOps | BC03.7 |
| S22 | README complet + procédures | DevOps | BC03.8 |
| S22 | Procédures démarrage/arrêt/backup | DevOps | BC03.8 |
| S23 | Tests performance (Locust) | DevOps | BC03.6 |
| S23 | Rapport test performance | DevOps | BC03.6 |
| S24 | Tests fonctionnels complets | Tous | BC03.5 |
| S24 | CR Sprint 5 | Chef de projet | BC02.3 |

### Mars 2027 — Infra & Sécurité

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S25 | Ansible roles (web, db, monitoring, security) | DevOps | BC03.4 |
| S25 | Politique sécurité | Chef de projet | BC03.9 |
| S26 | Configuration nftables + Fail2ban | DevOps | BC03.10 |
| S26 | Déploiement Suricata IDS/IPS | DevOps | BC03.12 |
| S27 | Configuration Prometheus + Grafana | DevOps | BC03.11 |
| S27 | Configuration Loki + Promtail (logs) | DevOps | BC03.11 |
| S28 | Scan OpenVAS + corrections | DevOps | BC03.12 |
| S28 | Configuration Wazuh (SIEM) | DevOps | BC03.12 |

### Avril 2027 — Infra & Sécurité (suite)

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S29 | OpenTofu infra on-prem | DevOps | BC04.1 |
| S29 | Schéma architecture infra (VLAN) | DevOps | BC04.4 |
| S30 | Keycloak IAM complet + RBAC | DevOps | BC04.6 |
| S30 | ModSecurity WAF + OWASP CRS | DevOps | BC04.6 |
| S31 | Chiffrement LUKS + pgcrypto | DevOps | BC04.7 |
| S31 | Backup BorgBackup + Restic | DevOps | BC04.7 |
| S32 | Capture monitoring opérationnel | DevOps | BC03.11, BC04.10 |

### Mai 2027 — Déploiement & Migration

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S33 | Comparatif coûts AWS vs Azure vs On-Prem | Chef de projet | BC04.2 |
| S33 | Analyse TCO 3 ans | Chef de projet | BC04.2 |
| S34 | Plan migration documenté (7 étapes) | DevOps | BC04.3 |
| S34 | Migration données vers infra on-prem | DevOps | BC04.3 |
| S35 | Tests post-migration | Tous | BC04.5 |
| S35 | Preuve fonctionnement | Tous | BC04.5 |
| S36 | Grille analyse risques sécurité (10 risques) | Chef de projet | BC04.8 |
| S36 | Réunion client validation | Chef de projet | BC02.4 |

### Juin 2027 — Finalisation

| Semaine | Tâche | Responsable | Compétence |
|---------|-------|-------------|-----------|
| S37 | Tests finaux complets | Tous | BC03.5, BC03.6 |
| S37 | Bilan dynamique équipe | Chef de projet | BC02.6 |
| S38 | Réunion prenante externe | Chef de projet | BC02.7 |
| S38 | Synthèse conduite changement | Chef de projet | BC02.8 |
| S39 | Préparation support présentation | Tous | BC02.4 |
| S39 | Finalisation documentation | Tous | Tous |
| S40 | Soutenance / présentation finale | Tous | BC02.4 |
| S40 | Remise livrables complets | Chef de projet | Tous |

---

## Jalons

| Jalon | Date | Description | Validation |
|-------|------|-------------|-----------|
| J0 | Sept 2026 | Entretien client | CR signé |
| J1 | Oct 2026 | CdC validé | Approbation client |
| J2 | Nov 2026 | Planning validé | Approbation client |
| J3 | Déc 2026 | MVP fonctionnel | Démo Sprint 1 |
| J4 | Fév 2027 | Application complète | Démo Sprint 5 |
| J5 | Avr 2027 | Infra sécurisée | Audit sécurité |
| J6 | Mai 2027 | Migration terminée | Tests validés |
| J7 | Juin 2027 | Livraison finale | Soutenance |

---

## Dépendances critiques

```
Analyse besoins (BC01.1)
    └──→ Specs (BC01.3)
            └──→ Architecture (BC01.5)
                    └──→ Développement (BC03.1)
                            └──→ Tests (BC03.5-6)
                                    └──→ Infra sécurité (BC03.9-12)
                                            └──→ Migration (BC04.3)
                                                    └──→ Soutenance
```
