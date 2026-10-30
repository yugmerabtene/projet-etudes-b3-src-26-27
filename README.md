# Grille de notation — Projet d’études B3 SR 2026-2027

## Évaluation par preuves — Référencement direct au RNCP38478

### Règle unique
Chaque ligne est un **indicateur de preuve** rattaché à l’une des **25 compétences officielles** du RNCP38478 (8 pour BC01, 5 pour BC02, 8 pour BC03, 4 pour BC04). Un indicateur est validé lorsque deux éléments sont réunis : la **preuve** demandée (fichier, capture, outil, dépôt) et l’**argumentaire** qui établit le lien entre cette preuve et la compétence visée. Une preuve non argumentée n’est pas recevable. Une compétence officielle est acquise lorsque tous ses indicateurs sont validés.

### Répartition des points
Les points sont calculés à raison de 0,5 point par indicateur de preuve ; la difficulté qualifie le niveau du bloc.

| Bloc | Difficulté | Points |
|------|-----------|:------:|
| BC01 — Piloter un projet informatique | Modérée (conception, planification) | **5,0** |
| BC02 — Coordonner une équipe projet | Facile (communication, coordination) | **4,0** |
| BC03 — Superviser la mise en œuvre d’un projet informatique | **Très élevée** (déploiement, sécurité, IaC, automatisation) | **6,0** |
| BC04 — Orchestrer l’infrastructure cloud | **Élevée** (cloud, migration, supervision) | **5,0** |
| | **Total** | **20,0** |

> Chaque indicateur vaut 0,5 point ; une compétence officielle est acquise lorsque tous ses indicateurs sont validés.

---

## BC01 — Piloter un projet informatique *(5,0 pts)*
Bloc commun obligatoire — RNCP38478 BC01

| Réf. | Indicateur de preuve | Compétence officielle | Preuve à fournir | Pts | ✓/✗ | Obtenu |
|:---:|---|---|---|:---:|:---:|:---:|
| BC01.1 | Analyser les besoins du client (appel d’offres ou commande) | BC01.1 | Compte rendu d’entretien client signé OU analyse écrite de l’appel d’offres | 0,5 | | |
| BC01.2 | Concevoir une solution informatique adaptée | BC01.1 | Diagramme de contexte ou cas d’utilisation (acteurs, périmètre, flux) | 0,5 | | |
| BC01.3 | Déterminer les spécifications fonctionnelles et techniques et le cahier des charges | BC01.2 | Spécifications numérotées (min. 5) + cahier des charges complet | 0,5 | | |
| BC01.4 | Concevoir via une méthode de gestion de projet | BC01.3 | Justification méthodologique + diagramme de Gantt | 0,5 | | |
| BC01.5 | Définir l’architecture du système d’information | BC01.4 | Schéma d’architecture du SI (réseau, composants, flux, couches) | 0,5 | | |
| BC01.6 | Garantir la disponibilité du système d’information | BC01.4 | Document justifiant chaque choix d’architecture | 0,5 | | |
| BC01.7 | Veille technologique, concurrentielle et réglementaire | BC01.5 | Synthèse de veille documentée | 0,5 | | |
| BC01.8 | Affecter les ressources nécessaires au projet | BC01.6 | Matrice des ressources + matrice des risques (min. 5 risques) | 0,5 | | |
| BC01.9 | Ordonnancer les tâches | BC01.7 | Planning opérationnel (Gantt) | 0,5 | | |
| BC01.10 | Réduire les interruptions de service (SLA) | BC01.8 | Plan d’atténuation préventif et curatif | 0,5 | | |
| | **Sous-total BC01** | | | **5,0** | | |

---

## BC02 — Coordonner une équipe projet *(4,0 pts)*
Bloc commun obligatoire — RNCP38478 BC02

| Réf. | Indicateur de preuve | Compétence officielle | Preuve à fournir | Pts | ✓/✗ | Obtenu |
|:---:|---|---|---|:---:|:---:|:---:|
| BC02.1 | Circulation et accessibilité de l’information | BC02.1 | Outil de gestion de projet actif (min. 5 tickets) | 0,5 | | |
| BC02.2 | Compréhension et adhésion au projet | BC02.1 | Répartition des rôles et des tâches documentée | 0,5 | | |
| BC02.3 | Mobiliser les parties prenantes (approche inclusive) | BC02.4 | Comptes rendus de réunions (min. 2) avec ordre du jour, participants et décisions | 0,5 | | |
| BC02.4 | Relation client, satisfaction et fidélisation | BC02.2 | Support de présentation client + compte rendu post-échange | 0,5 | | |
| BC02.5 | Animer les collaborations et la communication | BC02.3 | Planning de communication défini | 0,5 | | |
| BC02.6 | Capitaliser les savoir-faire et les méthodes | BC02.5 | Bilan de dynamique d’équipe (freins, moteurs, communication) | 0,5 | | |
| BC02.7 | Amélioration continue du groupe | BC02.5 | Preuve de réunion avec une partie prenante externe | 0,5 | | |
| BC02.8 | Conduite du changement et fédération | BC02.5 | Synthèse du pilotage d’équipe | 0,5 | | |
| | **Sous-total BC02** | | | **4,0** | | |

---

## BC03 — Superviser la mise en œuvre d’un projet informatique *(6,0 pts)*
Bloc commun obligatoire — RNCP38478 BC03

| Réf. | Indicateur de preuve | Compétence officielle | Preuve à fournir | Pts | ✓/✗ | Obtenu |
|:---:|---|---|---|:---:|:---:|:---:|
| BC03.1 | Déployer l’infrastructure (systèmes d’exploitation, serveurs, logiciels, bases de données) | BC03.1 | Capture de déploiement fonctionnel OU accès direct | 0,5 | | |
| BC03.2 | Gestion de configuration (IaC) | BC03.1 | Playbook Ansible, Dockerfile ou équivalent | 0,5 | | |
| BC03.3 | Documentation technique de mise en production | BC03.1 | Plan de déploiement pas à pas | 0,5 | | |
| BC03.4 | Automatiser l’infrastructure (outils spécifiques) | BC03.3 | Scripts IaC (Ansible, Terraform, bash, Python) | 0,5 | | |
| BC03.5 | Vérifier la performance — tests fonctionnels | BC03.6 | Tableau de tests (min. 3 cas) avec résultat daté | 0,5 | | |
| BC03.6 | Vérifier la performance — rapport d’intégration | BC03.6 | Rapport de test de performance et d’intégration (temps de réponse, charge) | 0,5 | | |
| BC03.7 | Gestion de la récurrence des erreurs | BC03.7 | Procédure des erreurs récurrentes (causes et solutions) | 0,5 | | |
| BC03.8 | Procédures et documentation technique | BC03.8 | README + procédures d’exploitation | 0,5 | | |
| BC03.9 | Politique de sécurité de l’information | BC03.4 | Document de politique de sécurité | 0,5 | | |
| BC03.10 | Mesures contre les intrusions, les fraudes et les fuites | BC03.5 | Configuration de sécurité + surveillance | 0,5 | | |
| BC03.11 | Surveillance de l’infrastructure (supervision, journaux) | BC03.2 | Capture de supervision opérationnelle | 0,5 | | |
| BC03.12 | Détection des intrusions — journaux et alertes | BC03.5 | Journaux IDS/IPS + rapports de scan de vulnérabilités | 0,5 | | |
| | **Sous-total BC03** | | | **6,0** | | |

---

## BC04 — Orchestrer l’infrastructure cloud *(5,0 pts)*
Bloc optionnel de spécialisation cloud — RNCP38478 BC04

| Réf. | Indicateur de preuve | Compétence officielle | Preuve à fournir | Pts | ✓/✗ | Obtenu |
|:---:|---|---|---|:---:|:---:|:---:|
| BC04.1 | Déployer un service par virtualisation ou dans le cloud (IaC) | BC04.1 | Code IaC livré (Terraform, Ansible, CloudFormation) | 0,5 | | |
| BC04.2 | Optimiser les coûts informatiques du cloud | BC04.1 | Comparatif de fournisseurs (min. 2) + analyse des coûts | 0,5 | | |
| BC04.3 | Scalabilité, collaboration et mises à jour cloud | BC04.1 | Plan de migration cloud documenté | 0,5 | | |
| BC04.4 | Configurer les services cloud (qualité et maintenance) | BC04.2 | Schéma d’architecture cloud complet | 0,5 | | |
| BC04.5 | Qualité et maintenance cloud — fonctionnement | BC04.2 | Preuve de fonctionnement (tests OU captures) | 0,5 | | |
| BC04.6 | Sécurité cloud — outils et techniques | BC04.3 | Capture des règles de sécurité (IAM, HTTPS, certificats) | 0,5 | | |
| BC04.7 | Protéger les données cloud | BC04.3 | Politique de sécurité cloud (chiffrement, accès, sauvegardes) | 0,5 | | |
| BC04.8 | Analyse des risques de sécurité cloud | BC04.4 | Grille d’analyse des risques (menace, impact, contre-mesure) | 0,5 | | |
| BC04.9 | Versionner le code d’infrastructure | BC04.1 | Dépôt Git avec historique (min. 5 commits, au moins 3 dates) | 0,5 | | |
| BC04.10 | Surveiller l’infrastructure cloud | BC04.2 | Capture de supervision cloud (CPU, mémoire, disponibilité) | 0,5 | | |
| | **Sous-total BC04** | | | **5,0** | | |

---

## Tableau de notation vierge (à imprimer)

| Bloc | Réf. | Compétence officielle | Preuve | Pts | ✓/✗ | Obtenu |
|------|:---:|---|---|:---:|:---:|:------:|
| **BC01** | BC01.1 | BC01.1 | Compte rendu d’entretien ou analyse de l’appel d’offres | 0,5 | | |
| | BC01.2 | BC01.1 | Diagramme de contexte ou cas d’utilisation | 0,5 | | |
| | BC01.3 | BC01.2 | Spécifications + cahier des charges complet | 0,5 | | |
| | BC01.4 | BC01.3 | Méthodologie + planning | 0,5 | | |
| | BC01.5 | BC01.4 | Schéma d’architecture du SI | 0,5 | | |
| | BC01.6 | BC01.4 | Justification des choix d’architecture | 0,5 | | |
| | BC01.7 | BC01.5 | Synthèse de veille documentée | 0,5 | | |
| | BC01.8 | BC01.6 | Matrice des ressources + risques | 0,5 | | |
| | BC01.9 | BC01.7 | Planning opérationnel | 0,5 | | |
| | BC01.10 | BC01.8 | Plan d’atténuation des interruptions (SLA) | 0,5 | | |
| | | **Sous-total BC01** | | **5,0** | | |
| **BC02** | BC02.1 | BC02.1 | Outil de gestion de projet + 5 tickets | 0,5 | | |
| | BC02.2 | BC02.1 | Répartition des rôles et des tâches | 0,5 | | |
| | BC02.3 | BC02.4 | Comptes rendus de réunions (min. 2) | 0,5 | | |
| | BC02.4 | BC02.2 | Support de présentation + compte rendu client | 0,5 | | |
| | BC02.5 | BC02.3 | Planning de communication | 0,5 | | |
| | BC02.6 | BC02.5 | Bilan de dynamique d’équipe | 0,5 | | |
| | BC02.7 | BC02.5 | Réunion avec une partie prenante externe | 0,5 | | |
| | BC02.8 | BC02.5 | Synthèse du pilotage d’équipe | 0,5 | | |
| | | **Sous-total BC02** | | **4,0** | | |
| **BC03** | BC03.1 | BC03.1 | Capture d’infrastructure fonctionnelle | 0,5 | | |
| | BC03.2 | BC03.1 | Configuration, playbook ou Dockerfile | 0,5 | | |
| | BC03.3 | BC03.1 | Plan de déploiement pas à pas | 0,5 | | |
| | BC03.4 | BC03.3 | Scripts d’automatisation | 0,5 | | |
| | BC03.5 | BC03.6 | Tableau de tests (min. 3) | 0,5 | | |
| | BC03.6 | BC03.6 | Rapport de test de performance | 0,5 | | |
| | BC03.7 | BC03.7 | Procédure des erreurs récurrentes | 0,5 | | |
| | BC03.8 | BC03.8 | README + procédures d’exploitation | 0,5 | | |
| | BC03.9 | BC03.4 | Document de politique de sécurité | 0,5 | | |
| | BC03.10 | BC03.5 | Configuration de sécurité + surveillance | 0,5 | | |
| | BC03.11 | BC03.2 | Capture de supervision | 0,5 | | |
| | BC03.12 | BC03.5 | Journaux IDS/IPS + rapports de scan | 0,5 | | |
| | | **Sous-total BC03** | | **6,0** | | |
| **BC04** | BC04.1 | BC04.1 | Code IaC livré | 0,5 | | |
| | BC04.2 | BC04.1 | Comparatif de fournisseurs + coûts | 0,5 | | |
| | BC04.3 | BC04.1 | Plan de migration cloud | 0,5 | | |
| | BC04.4 | BC04.2 | Schéma d’architecture cloud | 0,5 | | |
| | BC04.5 | BC04.2 | Preuve de fonctionnement | 0,5 | | |
| | BC04.6 | BC04.3 | Règles de sécurité appliquées | 0,5 | | |
| | BC04.7 | BC04.3 | Politique de sécurité cloud | 0,5 | | |
| | BC04.8 | BC04.4 | Grille d’analyse des risques | 0,5 | | |
| | BC04.9 | BC04.1 | Dépôt Git avec historique | 0,5 | | |
| | BC04.10 | BC04.2 | Capture de supervision cloud | 0,5 | | |
| | | **Sous-total BC04** | | **5,0** | | |
| | | **TOTAL /20** | | **20,0** | | |

---

## Bonus — Qualité de présentation *(+2,0 pts max)*

> **Hors barème RNCP38478.** Note complémentaire facultative.

| # | Critère | Preuve | Pts | ✓/✗ | Obtenu |
|:-:|---------|--------|:---:|:---:|:---:|
| B1 | [Qualité du support de présentation](competences/BC02.md) — design, structure, clarté visuelle | Support PDF ou PPT livré | +1,0 | | |
| B2 | [Qualité de la présentation orale](competences/BC02.md) — élocution, posture, gestion du temps | Observation du jury | +1,0 | | |
| | **Sous-total Bonus** | | **+2,0** | | |

---

## Récapitulatif de validation RNCP38478

| Bloc | Intitulé RNCP38478 | Statut | Compétences RNCP | Indicateurs | Pts /20 | Difficulté |
|------|--------------------|--------|:----------------:|:-----------:|:-------:|:----------:|
| BC01 | Piloter un projet informatique | Obligatoire | 8 | 10 | 5,0 | Modérée |
| BC02 | Coordonner une équipe projet | Obligatoire | 5 | 8 | 4,0 | Facile |
| BC03 | Superviser la mise en œuvre d’un projet informatique | Obligatoire | 8 | 12 | 6,0 | Très élevée |
| BC04 | Orchestrer l’infrastructure cloud | 1 optionnel parmi 3 | 4 | 10 | 5,0 | Élevée |
| | **Total RNCP** | | **25** | **40** | **20,0** | |
| | **Bonus** | | — | — | **+2,0** | |
| | **Total final** | | **25** | **40** | **22,0** | |

---

## Couverture des compétences officielles RNCP38478

### Contrôle de couverture

| Bloc | Compétences officielles | Indicateurs | Couverture |
|------|:-----------------------:|:-----------:|:----------:|
| BC01 | 8 | 10 | 8 / 8 |
| BC02 | 5 | 8 | 5 / 5 |
| BC03 | 8 | 12 | 8 / 8 |
| BC04 | 4 | 10 | 4 / 4 |
| **Total** | **25** | **40** | **25 / 25** |

> Les 25 compétences officielles du RNCP38478 sont couvertes par 40 indicateurs de preuve.

---

## Modalités d’évaluation (RNCP38478)

| Bloc | Type d’épreuve | Livrable | Correcteur |
|------|---------------|----------|------------|
| BC01 | Écrit | Document de cadrage (60 %) + introspection (40 %) | Correcteur externe |
| BC02 | Oral | Soutenance et démonstration (80 %) + rendez-vous de cadrage (20 %) | Jury ou responsable pédagogique |
| BC03 | Écrit | Livrable final (100 %) | Correcteur externe (même que BC01) |
| BC04 | Pratique | Livrables techniques : IaC, architecture cloud, sécurité, supervision, versionnement | Correcteur externe |

## Détail des modalités RNCP par bloc

### BC01 — Mise en situation professionnelle reconstituée
À partir d’un **appel d’offres reconstitué** portant sur l’organisation d’un système d’information, le candidat conçoit un projet informatique : spécifications fonctionnelles et techniques, architecture du SI, planification, veille, prévention des risques, dimension responsable du projet.

### BC02 — Deux mises en situation professionnelle reconstituées
1. **Épreuve collective** : à partir de documents de réponse à un appel d’offres d’une entreprise de services du numérique, l’équipe prépare et anime une réunion de présentation au client (avant-vente).
2. **Épreuve individuelle** : analyse de la dynamique du groupe projet — qu’est-ce qui a donné du sens ? Qu’est-ce qui a été fédérateur ? Quels freins ? Quelles techniques collaboratives et inclusives ?

### BC03 — Mise en situation professionnelle reconstituée
À partir de documents de réponse à un appel d’offres d’une entreprise de services du numérique (spécifications, architecture, planning), le candidat élabore une **documentation technique pour la mise en production** de l’infrastructure.

### BC04 — Deux mises en situation professionnelle reconstituées
1. **Conception et planification cloud** : architecture cloud, migration, automatisation, supervision, amélioration continue, ressources, sécurité, redondance et disponibilité.
2. **Planification et sécurité cloud** : choix des fournisseurs, coûts, délais, risques de migration, tests d’automatisation, indisponibilités, sécurisation de l’infrastructure.

## Rappel des voies d’accès à la certification

| Voie | Conditions |
|------|-----------|
| **Formation** | Validation des 3 blocs communs (BC01 + BC02 + BC03) + 1 bloc optionnel (BC04, BC05 ou BC06) + évaluations complémentaires de la période en entreprise |
| **VAE** | Validation d’un ou plusieurs blocs par dossier + soutenance orale |
| **Mixte** | Formation + VAE |

> **Prérequis BC04** : avoir validé au préalable le bloc BC03 (Superviser la mise en œuvre d’un projet informatique).

---

*Document mis à jour le 29/10/2026 — Grille par preuves, 25 compétences officielles et 40 indicateurs du RNCP38478 (/20) + bonus de présentation (+2). Répartition des points.*
