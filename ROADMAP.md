# Feuille de route de compétences — projet d’études 2026-2027

## 1. À qui s’adresse ce document et comment l’utiliser

Cette feuille de route s’adresse à un étudiant de troisième année en systèmes et
réseaux qui doit mener le projet d’études sans disposer de toutes les
connaissances techniques nécessaires. Elle répond à quatre questions : quoi
apprendre, dans quel ordre, jusqu’à quel niveau, et ce que cet apprentissage
produit comme livrable.

Elle ne remplace ni le sujet du projet, ni les fiches de compétences, ni le
planning d’équipe. Elle les précède et les accompagne : chaque module prépare une
preuve attendue du projet, et chaque livrable de module peut devenir une pièce du
dossier de validation.

### Mode d’emploi

1. Lire la section 2 pour identifier les compétences absentes de la liste
   initiale, puis la section 3 pour choisir le module correspondant.
2. Suivre la séquence de la section 5. Un module se termine par son livrable et
   par les critères de passage de la section 6 ; sans ces deux éléments, il n’est
   pas acquis.
3. Tenir un journal d’apprentissage : date, module, commande exécutée, résultat
   obtenu, difficulté rencontrée. Ce journal alimente directement l’argumentaire
   de preuve attendu par le jury.

### Durée et découpage

La durée totale est de **368 heures** de travail personnel, hors production
d’équipe. Répartie sur **40 semaines**, elle représente une moyenne de
**9,2 heures par semaine**, avec des semaines plus chargées avant les jalons.

| Séquence | Semaines | Modules | Heures |
| --- | --- | --- | --- |
| Fondations | 1 à 8 | M0, M1, M4 | 48 |
| Planification | 9 à 12 | M2, M6 | 46 |
| Développement | 13 à 24 | M5, M3, M8, M7, M9 | 118 |
| Infrastructure | 25 à 32 | M9, M10, M11, M12 | 78 |
| Déploiement | 33 à 36 | M13, M14 | 42 |
| Finalisation | 37 à 40 | M14, M15 | 36 |
| **Total** | **40** | **M0 à M15** | **368** |

### Niveaux utilisés

| Niveau | Signification | Preuve attendue |
| --- | --- | --- |
| Découverte | sait expliquer une notion et reproduire un exemple guidé | schéma ou fiche explicative |
| Opérationnel | sait exécuter seul la tâche avec la documentation officielle | livrable reproductible et commande consignée |
| Autonome | sait concevoir, diagnostiquer et transmettre la méthode | livrable, mesure et compte rendu d’incident |

### Conventions de lecture

- **LAB** désigne le laboratoire, c’est-à-dire le parc fourni de trois serveurs (8 Go de mémoire, 4 à 8 cœurs, 250 Go de disque) et de sa baie de stockage partagée.
- Les codes **BC01** à **BC04** désignent les quatre blocs de compétences du référentiel ; le suffixe numérique identifie l’indicateur de preuve visé.
- **SI** désigne le système d’information, c’est-à-dire l’ensemble des services, des données et des moyens qui les supportent.

## 2. Ce que la liste initiale ne couvrait pas

La liste initiale de quatorze thèmes couvre les grandes familles techniques, mais
laisse de côté des compétences sans lesquelles le projet ne peut ni fonctionner
ni se prouver. Le tableau ci-dessous en identifie treize.

| Compétence manquante | Pourquoi elle est indispensable au projet | Module |
| --- | --- | --- |
| Réseau et protocoles | Sans adressage, DNS (système de noms de domaine), ports et transport, aucun service ne se relie ; le diagnostic d’incident commence toujours par la couche réseau. | M1 |
| Exposition HTTP (protocole de transfert hypertexte), TLS (sécurité de la couche transport) et autorité de certification | Le site est publié en HTTPS (HTTP sécurisé) avec des certificats internes ; sans poignée de main TLS ni certificat X.509 (format normalisé de certificat à clé publique), la sécurité annoncée n’est pas démontrable. | M1 |
| Système en exploitation | Les trois serveurs sont administrés sans interface graphique, par SSH (shell sécurisé), systemd, permissions et journaux ; c’est le socle de toute exploitation. | M2 |
| Sauvegarde et continuité | Un cluster n’est pas une sauvegarde ; les objectifs de reprise et le test de restauration sont des preuves distinctes de la disponibilité. | M13 |
| Sécurité applicative | L’application expose des formulaires et une authentification ; sans connaissance des risques web, les données et les sessions restent fragiles. | M9 |
| Gestion des secrets | Aucun secret ne doit figurer dans le dépôt ; le coffre, le chiffrement de déploiement et la rotation sont des gestes d’exploitation. | M9 |
| Observabilité | Sans métriques, journaux et traces, une panne est constatée par l’utilisateur, pas par l’équipe ; la supervision devient une preuve mesurable. | M13 |
| Tests | L’évaluation est binaire : une preuve absente ou non argumentée ne vaut pas le point ; les tests fournissent la preuve reproductible. | M5 et M15 |
| Conformité et gouvernance | Le projet traite des données personnelles et doit produire un registre, une base légale, une politique de sécurité et une analyse de risques. | M14 |
| Coûts et décision | Le choix auto-hébergé doit être défendu par un coût total de possession, un comparatif et une stratégie de réversibilité. | M14 |
| Interface utilisateur | Le projet doit être utilisable par un client ; HTML (langage de balisage hypertexte), CSS (feuilles de style en cascade), JavaScript et gabarits ne s’improvisent pas. | M7 |
| Données avancées | Un schéma lisible, des index utiles, des transactions et une bascule MySQL se démontrent par des plans d’exécution et des mesures. | M8 |
| Communication professionnelle | Une preuve non argumentée ne vaut rien ; rédaction, schémas, soutenance, anglais technique et veille font partie du livrable. | M14 et M15 |

## 3. Les seize modules

### M0 — Architecture machine et système d’exploitation

| Élément | Contenu |
| --- | --- |
| Objectif | Lire un schéma d’ordinateur et expliquer comment un programme devient un processus sur un serveur Linux. |
| Prérequis | Aucun. |
| Niveau | Découverte. |
| Durée | 14 heures. |
| Livrable | Une fiche de huit pages reliant chaque dossier de l’arborescence à un usage du LAB, avec un schéma de Von Neumann annoté. |
| Rattachement BC | BC01.5 (architecture du SI), BC03.1 (déployer l’infrastructure), BC03.8 (procédures et documentation technique). |

**Notions à maîtriser**

- Modèle de Von Neumann : mémoire, unité centrale, entrées-sorties, bus.
- Unité arithmétique et logique (ALU) : registres, cycle charger-décoder-exécuter.
- Hiérarchie mémoire : registres, mémoire cache, mémoire vive, stockage persistant.
- Noyau Linux : espace utilisateur, espace noyau, appels système, pilotes.
- Arborescence normalisée : `/etc`, `/var`, `/home`, `/usr`, `/proc`, `/sys`, `/dev`, `/tmp`.
- Processus, fils d’exécution, ordonnanceur, signaux.
- Amorçage : firmware, chargeur d’amorçage, noyau, `systemd`.
- Vocabulaire du dimensionnement : cœurs, mémoire résidente, disque, entrées-sorties.

### M1 — Réseau et protocoles

| Élément | Contenu |
| --- | --- |
| Objectif | Concevoir un plan d’adressage et diagnostiquer une connexion de bout en bout sur le réseau interne du LAB. |
| Prérequis | M0. |
| Niveau | Découverte. |
| Durée | 24 heures. |
| Livrable | Un plan d’adressage IPv4 et IPv6 du LAB, un schéma des flux par port et un compte rendu de diagnostic de trois pannes provoquées. |
| Rattachement BC | BC01.5 (architecture du SI), BC01.6 (garantir la disponibilité), BC03.10 (mesures contre les intrusions). |

**Notions à maîtriser**

- Modèle OSI (interconnexion des systèmes ouverts) en sept couches et modèle TCP/IP (protocole de contrôle de transmission et protocole Internet) en quatre couches ; encapsulation.
- IPv4 et IPv6 (versions 4 et 6 du protocole Internet) : masque, notation CIDR (routage inter-domaines sans classe), adresse réseau, diffusion, passerelle.
- DNS (système de noms de domaine) : résolution directe et inverse, enregistrements A (adresse IPv4), AAAA (adresse IPv6), CNAME (nom canonique) et MX (serveur de messagerie), durée de vie.
- DHCP (configuration dynamique des hôtes) : bail, plage, options ; configuration statique d’un serveur.
- NAT (traduction d’adresses réseau) et PAT (traduction d’adresses de port) : pourquoi le LAB n’en a pas besoin, quand un cloud y recourt.
- Routage : table, route par défaut, diagnostic avec `ip route`.
- TCP (protocole de contrôle de transmission) et UDP (protocole de datagramme utilisateur) : poignée de main, ports, états, usages.
- HTTP (protocole de transfert hypertexte) et HTTPS (HTTP sécurisé) : méthodes, codes, en-têtes, redirection.
- TLS (sécurité de la couche transport) : poignée de main, certificat X.509, autorité de certification, nom alternatif.
- Proxy inverse, répartition de charge de niveau 4 et de niveau 7, sondes de santé.
- Diagnostic : `ss`, `dig`, `curl`, `tcpdump`, `ping`, `traceroute`.

### M2 — Linux, ligne de commande et exploitation

| Élément | Contenu |
| --- | --- |
| Objectif | Administrer un serveur Ubuntu Server sans interface graphique et automatiser les tâches répétitives par des scripts Bash. |
| Prérequis | M0 et M1. |
| Niveau | Opérationnel. |
| Durée | 30 heures. |
| Livrable | Trois scripts Bash versionnés — inventaire réseau, vérification de services, rotation de journaux — et leur mode d’emploi d’une page. |
| Rattachement BC | BC03.1 (déployer l’infrastructure), BC03.2 (gestion de configuration), BC03.8 (procédures et documentation technique). |

**Notions à maîtriser**

- Navigation, fichiers, permissions Unix, `umask`, propriétaires, groupes.
- ACL (listes de contrôle d’accès) et attributs étendus.
- Processus : `ps`, `top`, `kill`, `nice`, limites, sortie standard et sortie d’erreur.
- `systemd` : unités de service, cibles, dépendances, redémarrage, journaux.
- Minuteries `systemd` et tâches planifiées par `cron` : différence et critère de choix.
- `journald` : requêtes, priorités, rotation, persistance.
- Paquets : `apt`, dépôts, versions, mises à jour de sécurité.
- SSH (shell sécurisé) par clés : paire, empreinte, trousseau local, `authorized_keys`, durcissement.
- Bash : variables, tests, conditions, boucles, fonctions, codes retour, `set -euo pipefail`.
- Scripts d’administration réseau : lecture de `ip`, `ss`, `dig`, boucle sur un inventaire, sortie lisible.
- Sauvegarde et restauration simples avec `rsync` et `tar`.
- Bonnes pratiques : idempotence, journalisation, séparation des privilèges.

### M3 — Virtualisation et conteneurisation

| Élément | Contenu |
| --- | --- |
| Objectif | Expliquer et mettre en œuvre les deux niveaux d’isolation — machine virtuelle et conteneur — puis choisir le bon selon le besoin. |
| Prérequis | M2. |
| Niveau | Opérationnel. |
| Durée | 18 heures. |
| Livrable | Une image Docker d’une application de démonstration, un fichier de construction commenté et un tableau de choix entre machine virtuelle et conteneur pour cinq cas du projet. |
| Rattachement BC | BC03.1 (déployer l’infrastructure), BC04.1 (déployer un service virtualisé ou cloud), BC04.3 (scalabilité et mises à jour). |

**Notions à maîtriser**

- Virtualisation : hyperviseur de type 1 et de type 2, machine virtuelle, allocation des ressources, instantané, réseau virtuel.
- Limite sur le parc fourni : les serveurs sont nus, aucun hyperviseur n’est déployé dans le LAB.
- Conteneur : isolation par espaces de noms et groupes de contrôle, image, couche, registre.
- Docker Engine : fichier de construction, étiquettes, réseau, volumes, limites de ressources.
- Cycle de vie d’une image : construction, analyse, publication, suppression.
- `containerd` et `k3s` : différence de rôle avec Docker, qui reste cantonné à l’intégration continue.
- Bonnes pratiques : image minimale, utilisateur non privilégié, aucune donnée dans l’image.
- Critères de choix : état, performance, portabilité, charge d’exploitation.

### M4 — Git et travail collaboratif

| Élément | Contenu |
| --- | --- |
| Objectif | Travailler à plusieurs sur un dépôt sans écraser le travail d’autrui et rendre l’historique lisible. |
| Prérequis | Aucun. |
| Niveau | Opérationnel. |
| Durée | 10 heures. |
| Livrable | Un dépôt d’équipe avec quatre branches, une demande de fusion revue, un historique lisible et un fichier `.gitignore` couvrant secrets et artefacts. |
| Rattachement BC | BC02.1 (circulation de l’information), BC03.2 (gestion de configuration), BC04.9 (versionner le code d’infrastructure). |

**Notions à maîtriser**

- Dépôt local, dépôt distant, arbre de travail, index, validation.
- Branches, fusion, rebasage, résolution de conflit.
- Demandes de fusion, revue de code, protection de branche.
- Étiquettes, versions, journal, recherche dans l’historique.
- Fichier `.gitignore`, hooks simples, signature des validations.
- Secrets hors dépôt : détection avant validation, gabarits, variables d’environnement.
- Organisation : une validation par unité de travail, message court et descriptif.
- Infrastructure décrite par le code : tout fichier de configuration versionné.

### M5 — Python, FastAPI et tests unitaires

| Élément | Contenu |
| --- | --- |
| Objectif | Écrire une API (interface de programmation) FastAPI authentifiée, validée et testée, puis l’exécuter derrière Uvicorn et Gunicorn. |
| Prérequis | M2 et M4. |
| Niveau | Autonome. |
| Durée | 36 heures. |
| Livrable | Une API de catalogue avec inscription, connexion, rôles, validation et vingt tests `pytest`, dont la couverture est mesurée et affichée. |
| Rattachement BC | BC03.1 (déployer l’infrastructure), BC03.5 (tests fonctionnels), BC03.6 (tests de performance et d’intégration), BC03.8 (procédures et documentation technique). |

**Notions à maîtriser**

- Python : types, structures, fonctions, modules, paquets, environnements virtuels.
- Dépendances : fichier de verrouillage, versions figées, reproductibilité.
- Programmation asynchrone : `async` et `await`, boucle d’événements, limites.
- FastAPI : routes, paramètres, corps, codes de réponse, dépendances, gestion des erreurs.
- Pydantic : schémas, validation, sérialisation, messages d’erreur.
- SQLAlchemy : session, modèles, requêtes, transactions.
- Alembic : révision, montée, descente, contrôle de dérive.
- Authentification : session signée, cookie, hachage Argon2id, rôles.
- Gabarits Jinja2 : héritage, variables transmises au gabarit, échappement automatique.
- Uvicorn et Gunicorn : processus de travail, journaux, arrêt propre.
- Tests unitaires avec `pytest` : fixtures, paramétrage, doublures, couverture.
- Tests d’intégration : base de test, client HTTP, isolation des données.
- Qualité : Ruff, mypy, typage, revue de code.

### M6 — Génie logiciel et architecture applicative

| Élément | Contenu |
| --- | --- |
| Objectif | Passer d’un besoin exprimé en langage courant à un modèle et à une architecture logicielle justifiée. |
| Prérequis | M0 et M4. |
| Niveau | Découverte. |
| Durée | 16 heures. |
| Livrable | Un dossier de conception de dix pages : quatre diagrammes UML (langage de modélisation unifié), un journal de trois décisions d’architecture et le contrat OpenAPI de l’API. |
| Rattachement BC | BC01.2 (concevoir une solution adaptée), BC01.3 (spécifications et cahier des charges), BC03.8 (procédures et documentation technique). |

**Notions à maîtriser**

- Cycle de vie logiciel, exigences fonctionnelles et non fonctionnelles.
- UML (langage de modélisation unifié) : cas d’usage, classes, séquence, activité, composants, déploiement.
- Architecture en couches, séparation des responsabilités, sens des dépendances.
- MVC (modèle-vue-contrôleur) et son adaptation au couple FastAPI et Jinja2.
- Monolithe modulaire, microservices, SOA (architecture orientée services) : avantages, coûts, critères de choix.
- Interface de programmation (API) : contrat, versionnement, documentation OpenAPI.
- Patrons simples : dépôt, service, injection de dépendance.
- Décisions d’architecture : journal des décisions, alternatives écartées, conséquences.
- Revue de conception : lisibilité, testabilité, exploitabilité.

### M7 — Interface utilisateur et gabarits

| Élément | Contenu |
| --- | --- |
| Objectif | Construire une interface accessible, adaptative et sans chaîne de construction JavaScript. |
| Prérequis | M5 et M6. |
| Niveau | Opérationnel. |
| Durée | 22 heures. |
| Livrable | Trois écrans — catalogue, fiche produit, administration — adaptatifs, accessibles au clavier, avec un formulaire validé côté serveur et côté navigateur. |
| Rattachement BC | BC01.2 (concevoir une solution adaptée), BC03.1 (déployer l’infrastructure). |

**Notions à maîtriser**

- HTML (langage de balisage hypertexte) sémantique : titres, formulaires, étiquettes, tableaux, régions structurantes.
- Accessibilité : contraste, navigation au clavier, alternatives textuelles, lecteurs d’écran.
- CSS (feuilles de style en cascade) : modèle de boîte, flux, grille, `flexbox` (boîtes flexibles), variables, requêtes de média.
- Tailwind CSS par exécutable autonome : configuration, purge, production du fichier statique.
- Adaptation : approche mobile d’abord, points de rupture, essai à trois largeurs.
- JavaScript vanilla (JavaScript standard) : DOM (modèle objet du document), événements, `fetch`, formulaires, validation côté client.
- Dégradation progressive : l’interface reste utilisable sans JavaScript.
- Jinja2 : héritage de gabarit, inclusions, macros, échappement automatique.
- Sécurité côté navigateur : CSP (politique de sécurité du contenu), cookies (témoins de connexion) `HttpOnly` et `SameSite`.
- Performance perçue : poids de page, images, cache du navigateur.

### M8 — Bases de données et données avancées

| Élément | Contenu |
| --- | --- |
| Objectif | Concevoir un schéma relationnel normalisé, l’interroger efficacement et expliquer comment MySQL assure la continuité. |
| Prérequis | M5. |
| Niveau | Opérationnel. |
| Durée | 30 heures. |
| Livrable | Un schéma normalisé de huit tables, dix requêtes avec leur plan d’exécution, une bascule de nœud MySQL chronométrée et une migration Alembic réversible. |
| Rattachement BC | BC01.6 (garantir la disponibilité), BC03.1 (déployer l’infrastructure), BC03.7 (gestion de la récurrence d’erreurs), BC04.7 (protéger les données cloud). |

**Notions à maîtriser**

- Modèle relationnel : tables, clés, contraintes, intégrité référentielle.
- SQL (langage de requête structuré) : sélection, jointures, agrégats, sous-requêtes, vues, index.
- Normalisation : formes normales, dénormalisation justifiée.
- Index : arbre B, index composite, coût d’écriture, plan d’exécution avec `EXPLAIN`.
- Transactions et ACID (atomicité, cohérence, isolation, durabilité).
- Niveaux d’isolation : lecture non validée, lecture validée, répétable, sérialisable ; anomalies associées.
- MySQL 8 : moteur InnoDB, utilisateurs, privilèges, paramètres, journaux.
- Réplication de groupe : primaire, secondaires, quorum, état du cluster.
- Bascule par MySQL Router : routage lecture et écriture, perte d’un nœud, reprise.
- NoSQL (bases non relationnelles) : familles document, clé-valeur, colonne, graphe ; cas d’usage et limites.
- Redis : cache clé-valeur, expiration, persistance ; présent seulement dans le paquet GitLab Omnibus, il n’est pas un composant du projet et n’est pas administré séparément.
- Migrations Alembic : révision, retour arrière, données de référence.

### M9 — Sécurité applicative et gestion des secrets

| Élément | Contenu |
| --- | --- |
| Objectif | Réduire la surface d’attaque de l’application et tenir les secrets hors du code et du dépôt. |
| Prérequis | M5 et M8. |
| Niveau | Autonome. |
| Durée | 22 heures. |
| Livrable | Un rapport de revue de sécurité : cinq vulnérabilités reproduites, leur correction, les tests associés et la procédure de rotation d’un secret. |
| Rattachement BC | BC03.9 (politique de sécurité de l’information), BC03.10 (mesures contre les intrusions), BC04.6 (sécurité cloud), BC04.8 (analyse des risques de sécurité). |

**Notions à maîtriser**

- Open Worldwide Application Security Project (OWASP) Top 10 : nature de chaque risque applicatif, exemple, contre-mesure.
- Injection SQL : requêtes paramétrées, moindre privilège en base.
- XSS (script inter-site) : échappement, CSP, contenu de confiance.
- CSRF (falsification de requête inter-site) : jeton, `SameSite`, vérification d’origine.
- Sessions et cookies signés : `HttpOnly`, `Secure`, `SameSite`, expiration, renouvellement.
- Hachage Argon2id : sel, paramètres de coût, migration de hachage.
- Validation des entrées : liste blanche, taille, type, normalisation, encodage de sortie.
- Moindre privilège : comptes de service, droits MySQL, droits système, conteneur non privilégié.
- Durcissement : en-têtes de sécurité, erreurs non bavardes, dépendances à jour, scan Trivy.
- Gestion des secrets : OpenBao hors cluster, `ansible-vault` pour le déploiement, variables d’environnement, rotation, révocation.

### M10 — Orchestration, répartition de charge et bascule

| Élément | Contenu |
| --- | --- |
| Objectif | Déployer le site sur un cluster k3s de trois nœuds et démontrer la reprise après la perte d’un nœud. |
| Prérequis | M2, M3 et M8. |
| Niveau | Autonome. |
| Durée | 26 heures. |
| Livrable | Un cluster k3s de trois nœuds, un déploiement reproductible du site, une mesure de bascule et un compte rendu de perte de nœud avec chronologie. |
| Rattachement BC | BC03.1 (déployer l’infrastructure), BC03.4 (automatiser l’infrastructure), BC04.1 (déployer un service virtualisé ou cloud), BC04.3 (scalabilité et mises à jour). |

**Notions à maîtriser**

- Kubernetes et k3s : nœud de contrôle, nœud de travail, objets, déclaratif, réconciliation ; les trois nœuds sont à la fois nœuds de contrôle et nœuds de travail.
- Objets : Pod (plus petite unité déployable), Deployment (déploiement), Service (service réseau), Ingress (entrée HTTP), ConfigMap (configuration), Secret (secret), DaemonSet (conteneur sur chaque nœud), volume persistant.
- Répartition de charge : service en mode cluster, entrée Nginx, algorithme par tourniquet.
- Niveau 4 et niveau 7 : différence, sondes de santé, affinité de session.
- Bascule : perte d’un nœud, réélection, replacement des pods, délai mesuré.
- Quorum : majorité, conséquence de la perte de deux nœuds sur trois.
- Certificats internes : `cert-manager`, autorité interne, renouvellement.
- Déploiement : mise à jour progressive, retour arrière, limites de ressources.
- Diagnostic : `kubectl`, journaux, événements, état des nœuds.
- Séparation des rôles : k3s pour le site, Docker pour l’intégration continue.

### M11 — Intégration continue, DevSecOps et journaux

| Élément | Contenu |
| --- | --- |
| Objectif | Automatiser la vérification, la construction et l’analyse du code, puis exploiter les journaux pour détecter un incident. |
| Prérequis | M4, M5 et M9. |
| Niveau | Autonome. |
| Durée | 24 heures. |
| Livrable | Une chaîne d’intégration qui échoue sur un test cassé et sur une vulnérabilité critique, plus une recherche Kibana qui reconstitue un incident en trois requêtes. |
| Rattachement BC | BC03.11 (surveillance de l’infrastructure), BC03.12 (détection d’intrusions), BC04.9 (versionner le code d’infrastructure), BC04.10 (surveiller l’infrastructure cloud). |

**Notions à maîtriser**

- Intégration continue et livraison continue (CI/CD) : étapes, déclencheurs, artefacts.
- GitLab CE (Community Edition) et GitLab Runner : fichier de chaîne, exécuteur, variables protégées, registre.
- GitHub Actions : flux de travail, déclencheurs, actions, secrets ; le LAB exécute ses chaînes sur GitLab Runner, GitHub Actions est étudié comme second modèle.
- DevSecOps (développement, sécurité et exploitation) : sécurité à chaque étape, analyse des dépendances, analyse statique, détection de secrets.
- Trivy : images, dépendances, configuration, seuils de blocage.
- Tests dans la chaîne : unitaires, intégration, contrat, rapport de couverture.
- Journaux : collecte Fluent Bit, transport, indexation Elasticsearch, consultation Kibana.
- Structure d’un journal : horodatage, hôte, service, niveau, identifiant de corrélation.
- Analyse : requête, tableau de bord, alerte, rétention, rotation.
- IDS (système de détection d’intrusion) et IPS (système de prévention d’intrusion) : signature, anomalie, placement réseau.
- Pourquoi ils ne sont pas déployés ici : Suricata et Wazuh sont écartés pour leur empreinte sur un parc de 24 Go ; la détection s’appuie sur les journaux, les alertes Prometheus et Trivy.

### M12 — Cloud, infrastructure décrite par le code et coûts

| Élément | Contenu |
| --- | --- |
| Objectif | Décrire l’infrastructure par du code et comparer le parc auto-hébergé à une offre cloud sur trois ans. |
| Prérequis | M2 et M10. |
| Niveau | Opérationnel. |
| Durée | 18 heures. |
| Livrable | Un cahier de procédures Ansible (playbook) qui configure les trois serveurs depuis un inventaire vide, et un comparatif chiffré sur trois ans entre le parc fourni et une offre cloud équivalente. |
| Rattachement BC | BC01.7 (veille technologique et réglementaire), BC04.1 (déployer un service virtualisé ou cloud), BC04.2 (optimiser les coûts), BC04.4 (configurer les services cloud). |

**Notions à maîtriser**

- Informatique en nuage (cloud) : IaaS (infrastructure en tant que service), PaaS (plateforme en tant que service), SaaS (logiciel en tant que service), responsabilité partagée.
- Services courants : machine virtuelle, conteneur, réseau virtuel, stockage objet, gestion des identités et des accès (IAM).
- IAM : comptes, rôles, politiques, moindre privilège, fédération ; dans le LAB, l’identité est portée par les sessions FastAPI et Argon2id, sans service d’identité séparé.
- S3 (service de stockage simple) : compartiment, objet, clé, versionnement, cycle de vie ; aucune API S3 n’est déployée dans le LAB.
- Réseau virtuel : sous-réseau, table de routage, groupe de sécurité, passerelle ; le LAB utilise un réseau interne physique, la notion sert à comprendre le cloud.
- Ansible : inventaire, playbook (cahier de procédures), rôle, variable, modèle, idempotence, mode `--check`.
- `ansible-vault` : chiffrement, édition, exécution avec un mot de passe hors dépôt.
- Infrastructure décrite par le code (IaC) : déclaratif, versionné, reproductible, revu.
- Approche déclarative de type OpenTofu ou Terraform : notion générale, non retenue ici car Ansible suffit pour trois serveurs sans API cloud.
- Coût total de possession (TCO) sur trois ans : matériel, licences, énergie, exploitation, formation.
- Comparatif auto-hébergé et cloud : coût, élasticité, réversibilité, souveraineté, dépendance.
- Réversibilité : export des données, formats ouverts, procédure de sortie.

### M13 — Sauvegarde, continuité et observabilité

| Élément | Contenu |
| --- | --- |
| Objectif | Prouver qu’une donnée peut être restaurée et qu’une panne peut être détectée avant l’utilisateur. |
| Prérequis | M8, M10 et M12. |
| Niveau | Autonome. |
| Durée | 26 heures. |
| Livrable | Un plan de sauvegarde et de reprise de six pages, une restauration complète chronométrée, et trois tableaux de bord Grafana avec deux alertes déclenchées sur une panne provoquée. |
| Rattachement BC | BC01.6 (garantir la disponibilité), BC03.11 (surveillance de l’infrastructure), BC03.12 (détection d’intrusions), BC04.7 (protéger les données cloud), BC04.10 (surveiller l’infrastructure cloud). |

**Notions à maîtriser**

- Stratégie 3-2-1 : trois copies, deux supports, une copie hors site.
- Objectif de point de reprise (RPO) et objectif de temps de reprise (RTO).
- Sauvegarde logique : `mysqlsh dump`, cohérence, compression, chiffrement.
- Journaux binaires : position, reprise à un instant précis, purge.
- XtraBackup : sauvegarde physique, préparation, restauration, exécution sur un nœud secondaire jamais sur le primaire.
- Restic : sauvegarde du système de fichiers des hôtes, dépôt, instantané, déduplication, vérification, politique de rétention.
- Test de restauration : procédure, durée mesurée, preuve datée.
- Plan de reprise d’activité et plan de continuité d’activité : périmètre, rôles, scénarios.
- Un cluster n’est pas une sauvegarde : la réplication protège la disponibilité, pas l’intégrité.
- Observabilité : métriques, journaux, traces ; les trois questions auxquelles elle répond.
- Prometheus : séries temporelles, étiquettes, collecte, exportateurs.
- PromQL (langage de requête Prometheus) : sélection, débit, agrégations, alertes.
- Grafana : sources de données, tableaux de bord, variables.
- Alertmanager : groupes, silences, routage, seuils.
- Fluent Bit, Elasticsearch et Kibana : collecte, indexation, recherche, rétention, rotation.

### M14 — Gestion de projet, conformité et communication

| Élément | Contenu |
| --- | --- |
| Objectif | Piloter le projet par des jalons, documenter les choix et défendre chaque preuve devant un jury. |
| Prérequis | M4 et M6. |
| Niveau | Autonome. |
| Durée | 28 heures. |
| Livrable | Un dossier de pilotage — planning, registre des risques, registre RGPD (règlement général sur la protection des données), politique de sécurité — et une soutenance blanche de vingt minutes avec l’argumentaire de trois preuves. |
| Rattachement BC | BC01.1 à BC01.10, BC02.1 à BC02.8, BC04.2 (optimiser les coûts), BC04.8 (analyse des risques de sécurité). |

**Notions à maîtriser**

- Cycle de vie de projet : cadrage, lot, jalon, livrable, recette.
- Méthodes agiles (Agile) : manifeste, itération, revue, rétrospective, tableau de suivi.
- Scrum : rôles, artefacts, événements, estimation, définition de terminé.
- Gestion classique : lotissement, chemin critique, diagramme de Gantt.
- Ressources, charges, risques, plan de communication, tableau de bord.
- Accord de niveau de service (SLA) : engagement, mesure, conséquence, tableau de bord.
- RGPD (règlement général sur la protection des données) : registre des traitements, base légale, droits des personnes, durée de conservation, sous-traitance.
- Analyse de risques : EBIOS (expression des besoins et identification des objectifs de sécurité), ISO 27005 (norme de gestion des risques de sécurité de l’Organisation internationale de normalisation), actif, menace, vulnérabilité, scénario, mesure.
- Politique de sécurité : périmètre, rôles, règles, exceptions, revue.
- Coûts : TCO, comparatif, réversibilité, souveraineté, traités en M12.
- Rédaction technique : plan, phrase courte, vocabulaire défini, schéma légendé.
- Communication : note de décision, compte rendu, argumentaire de preuve, soutenance, démonstration.
- Anglais technique : lecture de documentation, vocabulaire des RFC (documents de normalisation de l’Internet) et des interfaces.
- Veille : sources officielles, fréquence, fiche de lecture, décision.
- Cadres de méthode : les référentiels cités orientent l’analyse ; aucune certification n’est délivrée à l’issue du projet.

### M15 — Tests, validation et soutenance finale

| Élément | Contenu |
| --- | --- |
| Objectif | Démontrer, mesures en main, que le système livré satisfait chaque critère d’acceptation. |
| Prérequis | M5, M9 et M13. |
| Niveau | Autonome. |
| Durée | 24 heures. |
| Livrable | Un rapport de validation de quinze pages, une suite de tests reproductible, trois mesures de charge et une démonstration de dix minutes. |
| Rattachement BC | BC02.4 (relation client et satisfaction), BC02.6 (capitaliser les savoir-faire), BC03.5 (tests fonctionnels), BC03.6 (tests de performance et d’intégration). |

**Notions à maîtriser**

- Plan de tests : périmètre, stratégie, environnement, données, critères d’arrêt.
- Tests unitaires : isolation, doublure, cas limite, couverture de branche.
- Tests d’intégration : frontière base, frontière HTTP, jeu de données, nettoyage.
- Tests de bout en bout : parcours utilisateur, navigateur, reproductibilité.
- Tests de charge : scénario, montée en charge, latence au 95ᵉ centile, taux d’erreur, outil Locust.
- Tests de sécurité : revue OWASP, scan Trivy, essai d’authentification et d’autorisation.
- Tests de reprise : perte de nœud, restauration, bascule, chronométrage.
- Registre des anomalies : gravité, reproduction, correction, non-régression.
- Non-vacuité : casser volontairement le code, vérifier que le test échoue, réparer.
- Preuve : commande exacte, sortie datée, environnement, interprétation.
- Soutenance : plan, démonstration, gestion du temps, questions du jury.
- Argumentaire : relier la preuve à la compétence visée, distinguer ce qui est déployé de ce qui est seulement documenté.

## 4. Tableau récapitulatif des modules

| Module | Durée | Niveau | Prérequis | Livrable |
| --- | --- | --- | --- | --- |
| M0 | 14 h | Découverte | — | Fiche d’architecture machine et d’arborescence |
| M1 | 24 h | Découverte | M0 | Plan d’adressage, schéma des flux, diagnostic de pannes |
| M2 | 30 h | Opérationnel | M0, M1 | Trois scripts Bash et leur mode d’emploi |
| M3 | 18 h | Opérationnel | M2 | Image Docker et tableau de choix |
| M4 | 10 h | Opérationnel | — | Dépôt d’équipe avec fusion revue |
| M5 | 36 h | Autonome | M2, M4 | API FastAPI testée et mesurée |
| M6 | 16 h | Découverte | M0, M4 | Dossier de conception UML et OpenAPI |
| M7 | 22 h | Opérationnel | M5, M6 | Trois écrans accessibles et adaptatifs |
| M8 | 30 h | Opérationnel | M5 | Schéma normalisé, plans d’exécution, bascule |
| M9 | 22 h | Autonome | M5, M8 | Revue de sécurité et rotation d’un secret |
| M10 | 26 h | Autonome | M2, M3, M8 | Cluster k3s et mesure de bascule |
| M11 | 24 h | Autonome | M4, M5, M9 | Chaîne CI/CD et recherche d’incident |
| M12 | 18 h | Opérationnel | M2, M10 | Cahier de procédures Ansible et comparatif de coûts |
| M13 | 26 h | Autonome | M8, M10, M12 | Plan de sauvegarde, restauration, tableaux de bord |
| M14 | 28 h | Autonome | M4, M6 | Dossier de pilotage et soutenance blanche |
| M15 | 24 h | Autonome | M5, M9, M13 | Rapport de validation et démonstration |
| **Total** | **368 h** | — | — | **16 livrables** |

## 5. Séquence conseillée sur quarante semaines

La séquence suit les phases du projet : cadrage en septembre et octobre,
planification en novembre, développement de décembre à février, infrastructure en
mars et avril, déploiement en mai, finalisation en juin. Les semaines sont
numérotées de 1 à 40 ; la charge hebdomadaire indiquée est une cible, pas un
plancher.

| Semaines | Phase du projet | Modules travaillés | Heures | Heures par semaine |
| --- | --- | --- | --- | --- |
| 1 à 4 | Cadrage | M0, début de M4 | 18 | 4,5 |
| 5 à 8 | Cadrage | M1, fin de M4 | 30 | 7,5 |
| 9 à 12 | Planification | M2, M6 | 46 | 11,5 |
| 13 à 16 | Développement | début de M5, début de M3 | 38 | 9,5 |
| 17 à 20 | Développement | fin de M5, fin de M3, début de M8 | 34 | 8,5 |
| 21 à 24 | Développement | fin de M8, M7, début de M9 | 46 | 11,5 |
| 25 à 28 | Infrastructure | fin de M9, M10 | 36 | 9 |
| 29 à 32 | Infrastructure | M11, M12 | 42 | 10,5 |
| 33 à 36 | Déploiement | M13, début de M14 | 42 | 10,5 |
| 37 à 40 | Finalisation | fin de M14, M15 | 36 | 9 |
| **Total** | **Six phases** | **M0 à M15** | **368** | **9,2 en moyenne** |

### Points de vigilance sur la séquence

- Les semaines 9 à 12 et 21 à 24 sont les plus chargées : elles précèdent la
  planification détaillée et la fin du développement. Ne pas y ajouter de
  formation facultative.
- M3 commence pendant le développement parce que la conteneurisation sert
  d’abord à fabriquer et analyser les images, puis à préparer l’infrastructure.
- M9 commence en fin de développement et se termine pendant l’infrastructure :
  la revue de sécurité a besoin d’une application réelle et d’un début de
  déploiement.
- M14 est réparti entre le déploiement et la finalisation : le dossier de
  pilotage se remplit au fil du projet, il ne se rédige pas en juin.
- Une semaine manquée se rattrape sur la suivante, jamais en supprimant un
  livrable : les livrables sont les preuves.

## 6. Critères de passage

Un module est maîtrisé lorsque l’étudiant réalise les actions ci-dessous sans
aide, avec la documentation officielle pour seule référence. Une action non
réalisable en démonstration vaut échec du module.

| Module | Critère 1 | Critère 2 | Critère 3 |
| --- | --- | --- | --- |
| M0 | Nommer les parties du modèle de Von Neumann et expliquer le rôle de l’ALU. | Associer chaque dossier `/etc`, `/var`, `/home` à un usage du LAB. | Décrire le trajet d’un appel système du programme jusqu’au noyau. |
| M1 | Calculer un sous-réseau CIDR et vérifier le résultat avec `ipcalc`. | Expliquer une résolution DNS et une poignée de main TLS. | Localiser une panne avec `ss`, `dig` et `curl` sans aide. |
| M2 | Créer un utilisateur, poser des permissions et une ACL. | Écrire un script Bash idempotent qui vérifie trois services. | Retrouver une entrée dans `journald` et expliquer une unité `systemd`. |
| M3 | Distinguer machine virtuelle et conteneur sur trois critères. | Construire une image Docker minimale et la lancer sans privilège. | Expliquer pourquoi le LAB n’utilise pas d’hyperviseur. |
| M4 | Créer une branche, résoudre un conflit et fusionner sans perte. | Rédiger une demande de fusion et la faire relire. | Vérifier qu’aucun secret n’est présent dans l’historique. |
| M5 | Créer une route FastAPI validée par Pydantic et protégée par session. | Écrire un test `pytest` qui échoue quand la route est cassée. | Exécuter l’application derrière Uvicorn et Gunicorn et lire ses journaux. |
| M6 | Produire quatre diagrammes UML cohérents avec le besoin. | Justifier le choix du monolithe modulaire plutôt que des microservices. | Rédiger une décision d’architecture avec ses alternatives. |
| M7 | Produire un écran adaptatif et le vérifier à trois largeurs. | Valider un formulaire côté serveur et côté navigateur. | Expliquer l’échappement Jinja2 et le rôle de la CSP. |
| M8 | Normaliser un schéma jusqu’à la troisième forme normale. | Lire un plan d’exécution et ajouter l’index utile. | Provoquer une bascule MySQL Router et mesurer le délai. |
| M9 | Reproduire une injection SQL et une XSS, puis les corriger. | Configurer un cookie de session `HttpOnly` et `SameSite`. | Ranger un secret dans OpenBao et le faire tourner. |
| M10 | Déployer trois nœuds k3s et un service en trois réplicas. | Couper un nœud et expliquer la reprise. | Mesurer la répartition de charge sur trois requêtes successives. |
| M11 | Faire échouer une chaîne sur un test cassé puis sur une vulnérabilité critique. | Retrouver un incident dans Kibana en trois requêtes. | Expliquer pourquoi un IDS n’est pas déployé ici. |
| M12 | Écrire un cahier de procédures Ansible idempotent et le rejouer sans changement. | Décrire IAM, S3 et réseau virtuel sans confondre leurs rôles. | Produire un TCO sur trois ans avec ses hypothèses. |
| M13 | Restaurer une base et un fichier, puis chronométrer l’opération. | Expliquer RPO et RTO sur un scénario de panne. | Déclencher une alerte Prometheus et la retrouver dans Alertmanager. |
| M14 | Tenir un planning avec jalons et registre des risques. | Rédiger un registre RGPD et une politique de sécurité. | Soutenir dix minutes et répondre à trois questions de jury. |
| M15 | Exécuter la suite complète et publier le rapport de validation. | Casser volontairement le code et prouver que le test devient rouge. | Présenter trois preuves avec leur argumentaire. |

## 7. Ressources

Les ressources ci-dessous sont des documentations officielles, gratuites et
accessibles sans compte payant. Les adresses sont données à la racine du site :
la version à lire est celle de la branche stable au moment du projet. Aucun cours
propriétaire ni aucune formation payante n’est cité.

| Domaine | Ressource officielle | Usage dans la feuille de route |
| --- | --- | --- |
| Architecture machine et noyau | Documentation du noyau Linux (`kernel.org`), pages de manuel (`man7.org`) | M0, M2 |
| Système Ubuntu | Documentation d’Ubuntu Server (`ubuntu.com/server/docs`) | M0, M2 |
| Réseau et protocoles | RFC Editor (`rfc-editor.org`), pages de manuel `iproute2` | M1 |
| Nginx et proxy inverse | Documentation Nginx (`nginx.org/en/docs`) | M1, M10 |
| Git | Pro Git (`git-scm.com/book`) et documentation Git (`git-scm.com/doc`) | M4 |
| Docker | Documentation Docker (`docs.docker.com`) | M3, M11 |
| Python | Documentation Python (`docs.python.org`) | M5 |
| FastAPI | Documentation FastAPI (`fastapi.tiangolo.com`) | M5, M7 |
| Pydantic | Documentation Pydantic (`docs.pydantic.dev`) | M5 |
| SQLAlchemy et Alembic | Documentation SQLAlchemy (`docs.sqlalchemy.org`) et Alembic (`alembic.sqlalchemy.org`) | M5, M8 |
| Tests Python | Documentation pytest (`docs.pytest.org`) | M5, M15 |
| UML | Spécification UML de l’Object Management Group (OMG) (`omg.org/spec/UML`) | M6 |
| Web et accessibilité | MDN Web Docs (Mozilla Developer Network) (`developer.mozilla.org`), W3C (World Wide Web Consortium) WAI (Web Accessibility Initiative) (`w3.org/WAI`) | M7 |
| Tailwind CSS | Documentation Tailwind (`tailwindcss.com/docs`) | M7 |
| Jinja2 | Documentation Jinja2 (`jinja.palletsprojects.com`) | M5, M7 |
| MySQL | Documentation MySQL (`dev.mysql.com/doc`) | M8, M13 |
| XtraBackup | Documentation Percona (`docs.percona.com`) | M13 |
| Restic | Documentation Restic (`restic.readthedocs.io`) | M13 |
| Redis | Documentation Redis (`redis.io/docs`) | M8, comme notion générale |
| Ansible | Documentation Ansible (`docs.ansible.com`) | M12 |
| Kubernetes et k3s | Documentation Kubernetes (`kubernetes.io/docs`) et k3s (`docs.k3s.io`) | M10 |
| Entrée Kubernetes | Documentation Nginx Ingress (`kubernetes.github.io/ingress-nginx`) | M10 |
| Intégration continue | Documentation GitLab CI (intégration continue de GitLab) (`docs.gitlab.com/ee/ci`) et GitHub Actions (`docs.github.com/actions`) | M11 |
| Scan de vulnérabilités | Documentation Trivy (`aquasecurity.github.io/trivy`) | M9, M11 |
| Supervision | Documentation Prometheus (`prometheus.io/docs`), Grafana (`grafana.com/docs`) | M13 |
| Journaux | Documentation Fluent Bit (`fluentbit.io/documentation`) et Elastic (`elastic.co/guide`) | M11, M13 |
| Sécurité applicative | OWASP Top 10 (`owasp.org`) | M9 |
| Données personnelles | Guides de la Commission nationale de l’informatique et des libertés (CNIL) (`cnil.fr`) et Légifrance (`legifrance.gouv.fr`) | M14 |
| Analyse de risques | Guides de l’Agence nationale de la sécurité des systèmes d’information (ANSSI) (`cyber.gouv.fr`), dont EBIOS Risk Manager | M14 |
| Gestion de projet agile | Guide Scrum (`scrumguides.org`) | M14 |
| Notions cloud | Documentation publique d’un fournisseur cloud (`docs.aws.amazon.com`) pour IAM, S3 et réseau virtuel | M12 |
| Anglais technique | RFC Editor (`rfc-editor.org`) et documentations officielles en anglais | M1, M11, M14 |

### Règle de lecture

Une ressource ne remplace pas une manipulation. Pour chaque notion, l’étudiant
lit la page officielle, exécute la commande ou reproduit l’exemple, puis consigne
le résultat dans son journal d’apprentissage. Une notion lue sans exécution reste
au niveau découverte ; elle ne peut pas servir de preuve.
