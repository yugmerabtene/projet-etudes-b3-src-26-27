# Référentiel de pile technique

Ce document fixe la pile arrêtée pour le projet. Il décrit ce qui est retenu,
pourquoi, et ce qui est écarté. Les valeurs de mémoire sont des ordres de
grandeur destinés au dimensionnement ; ce ne sont pas des mesures.

## 1. Principe et grille de sélection

La pile est **entièrement composée de logiciels libres, auto-hébergée et sans
dépendance à un service cloud externe**. Une technologie n’est retenue que si elle respecte les
six critères ci-dessous. Les critères sont appliqués couche par couche ; entre
deux candidates qui les respectent, l’arbitrage se fait sur l’empreinte et sur
la charge d’exploitation.

| Critère | Exigence | Vérification |
| --- | --- | --- |
| Licence OSI | licence approuvée par l’Open Source Initiative (MIT, Apache 2.0, BSD, GPL, MPL, AGPL) | la licence est citée dans le tableau de la section 12 |
| Maintenance | version publiée ou correctif dans les douze derniers mois | la date de publication est vérifiable sur le dépôt du projet |
| Empreinte | ≤ 300 Mo de mémoire résidente, sauf justification écrite | les exceptions sont justifiées dans la section 12 |
| Dépendances | chaîne de dépendances courte et maîtrisée | aucune dépendance à un service payant ou à un cloud externe |
| Standard ouvert | format ou protocole documenté et remplaçable (OCI, SQL, OpenAPI, exposition Prometheus) | la technologie reste substituable sans réécrire l’application |
| Remplaçabilité | données et configuration exportables | une procédure de sortie est documentée |

Quatre exceptions à la règle d’empreinte sont assumées : le moteur de base de
données, la pile de journalisation, la forge et l’orchestration. Leur rôle ne peut pas être tenu
par un composant plus léger sans perdre une fonction demandée. Le détail figure
dans la section 12.

## 2. Application

L’application est écrite en **Python** avec **FastAPI**. Elle est servie par
**Uvicorn**, serveur ASGI (interface serveur asynchrone pour Python), derrière
**Gunicorn** lorsque plusieurs processus sont nécessaires.
La persistance passe par **SQLAlchemy** et **Alembic** ; le pilote MySQL est
**PyMySQL** ; la validation des données est assurée par **Pydantic**.

L’interface utilise **Tailwind CSS par exécutable autonome** : la feuille de
style est produite à partir d’un binaire, sans chaîne Node ni Vite. Le
JavaScript reste **vanilla**, c’est-à-dire du JavaScript standard écrit à la
main, sans cadre applicatif. L’authentification repose sur des **sessions
FastAPI** et sur un hachage des mots de passe en **Argon2**. Le back-office est
construit avec des **routes et des gabarits FastAPI**, sans second framework.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| FastAPI | cadre de l’API et des routes du back-office | MIT | 80 à 150 Mo par processus de travail |
| Uvicorn | serveur ASGI | BSD 3-Clause | 20 à 40 Mo par processus de travail |
| Gunicorn | gestionnaire de processus | MIT | 20 à 40 Mo par processus de travail |
| SQLAlchemy | accès aux données par correspondance objet-relationnel (ORM) | MIT | inclus dans le processus |
| Alembic | migrations de schéma | MIT | négligeable, exécution ponctuelle |
| PyMySQL | pilote MySQL | MIT | moins de 20 Mo |
| Pydantic | validation et sérialisation | MIT | inclus dans le processus |
| argon2-cffi | hachage Argon2id des mots de passe | MIT | moins de 20 Mo |
| Tailwind CSS | feuille de style produite hors ligne | MIT | 0 Mo à l’exécution (fichier statique) |

Le back-office ne nécessite ni second framework ni service d’identité séparé :
les sessions signées de FastAPI et les rôles applicatifs suffisent pour le
besoin.

## 3. Orchestration et déploiement

Le site est orchestré par **k3s**, distribution légère de Kubernetes qui
embarque **containerd** comme moteur de conteneurs. Le cluster compte **trois
nœuds**, chacun à la fois **server** (nœud de contrôle) et **agent** (nœud de
travail), ce qui forme le quorum, c’est-à-dire la majorité nécessaire pour
décider, et permet la reprise sur panne d’un nœud.

**Docker Engine** n’est pas l’orchestrateur du site. Il est cantonné à
l’intégration continue : **GitLab Runner**, construction des images et
exécution de **Trivy**. Les deux moteurs ont des rôles disjoints : k3s porte le
sans-état, Docker fabrique et analyse les images. Aucune couche d’hyperviseur :
les serveurs sont fournis nus.

Le déploiement et la configuration des hôtes sont automatisés par **Ansible**.
Les **secrets de déploiement** sont chiffrés par **ansible-vault**, la fonction
de coffre intégrée à Ansible ; les **secrets applicatifs et de service** sont
gardés par **OpenBao**, hors cluster (section 9).

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| k3s | orchestration du site | Apache 2.0 | 400 à 600 Mo par nœud |
| containerd | moteur de conteneurs de k3s | Apache 2.0 | 100 à 150 Mo par nœud |
| Docker Engine | construction et analyse des images, CI uniquement | Apache 2.0 | 150 à 250 Mo |
| Ansible | configuration des hôtes et déploiement | GPLv3 | négligeable, exécution ponctuelle |
| ansible-vault | chiffrement des secrets de déploiement | GPLv3 | inclus dans Ansible |

## 4. Données et continuité

La base applicative est **MySQL 8 Community**. La continuité est assurée par
**MySQL InnoDB Cluster** : **Group Replication** pour la réplication,
**MySQL Shell** pour l’administration du cluster et **MySQL Router** pour
l’aiguillage des connexions. Chaque serveur porte **un nœud** ; les **trois
nœuds** forment le quorum et permettent une bascule.

Les sauvegardes combinent trois mécanismes :

- **`mysqlsh dump`** pour un export logique daté ;
- les **journaux binaires** pour la reprise à un instant précis ;
- **XtraBackup** pour une sauvegarde physique, pris **sur un nœud secondaire**,
  jamais sur le primaire.

**Restic** sauvegarde le système de fichiers des hôtes. **Le cluster n’est pas
une sauvegarde** : la réplication protège la disponibilité, pas contre une
suppression ou une corruption logique. Les deux preuves sont produites
séparément.

**MariaDB** est la variante documentée, avec le même rôle et les mêmes
procédures. Elle n’est pas déployée en parallèle.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| MySQL 8 Community | base relationnelle | GPLv2 (exception FOSS) | 1,0 à 1,5 Go par nœud |
| MySQL Shell | administration du cluster | GPLv2 | négligeable, exécution ponctuelle |
| MySQL Router | aiguillage des connexions | GPLv2 | 30 à 50 Mo par nœud |
| XtraBackup | sauvegarde physique sur un secondaire | GPLv2 | négligeable, exécution ponctuelle |
| Restic | sauvegarde du système | BSD 2-Clause | négligeable, exécution ponctuelle |
| MariaDB | variante documentée | GPLv2 | 1,0 à 1,5 Go par nœud |

## 5. Entrée et exposition

L’entrée du site est un **Nginx Ingress Controller déployé en DaemonSet**,
c’est-à-dire un conteneur sur chaque nœud. Il écoute les ports **80 et 443 sur
les trois nœuds**. Les certificats sont
délivrés par **cert-manager** à partir d’une **autorité interne**.

Aucun équilibreur externe, aucune adresse virtuelle, aucune adresse MetalLB ni
ServiceLB n’est utilisé. L’exposition se fait sur le **réseau interne du
LAB** : le nom DNS pointe vers les adresses des nœuds, et le client bascule
d’un nœud à l’autre. La bascule n’est donc pas instantanée ; elle dépend du
renvoi DNS et de la reprise du client.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| Nginx Ingress Controller | entrée HTTP et HTTPS en DaemonSet | Apache 2.0 | 50 à 100 Mo par nœud |
| cert-manager | délivrance et renouvellement des certificats | Apache 2.0 | 50 à 80 Mo |

## 6. Journalisation

La journalisation est **EFK** : **Elasticsearch** stocke et indexe,
**Kibana** interroge et affiche, **Fluent Bit** collecte les journaux sur les
nœuds. Elasticsearch et Kibana sont sous **AGPLv3 depuis la version 8.16** ;
Fluent Bit est sous **Apache 2.0**.

Le **tas Elasticsearch**, c’est-à-dire la mémoire allouée à la machine
virtuelle Java, **est limité à 2 Go** ; la **rétention visée est de douze mois**.
Le volume estimé est de **1 à 2 Go bruts par jour** pour les trois nœuds, soit
**365 à 730 Go bruts sur douze mois**. Avec une compression de l’ordre de
**2:1**, le volume indexé tombe à **180 à 365 Go**. En retenant **200 Go utiles**
sur les 250 Go de SRV-2, la rétention de douze mois n’est tenue que dans
l’hypothèse basse (365 Go bruts → 180 Go compressés, sous 200 Go) et pas dans
l’hypothèse haute (730 Go bruts → 365 Go compressés, au-delà). La rétention
effective est donc **bornée par la capacité** : index par jour et suppression des
plus anciens, avec une cible de douze mois pouvant descendre à six à huit mois si
le volume atteint 2 Go par jour.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| Elasticsearch | stockage et indexation des journaux | AGPLv3 (depuis 8.16) | 2,5 à 3 Go, tas limité à 2 Go |
| Kibana | consultation des journaux | AGPLv3 (depuis 8.16) | 250 Mo (avec ressources limitées) |
| Fluent Bit | collecte sur les nœuds | Apache 2.0 | 30 à 60 Mo par nœud |

## 7. Supervision

La supervision s’appuie sur **kube-prometheus-stack** : **Prometheus** collecte
les métriques, **node_exporter** expose celles des hôtes, **Grafana** les
affiche et **Alertmanager** route les alertes. L’ensemble est déployé dans le
cluster.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| Prometheus | collecte et stockage des métriques | Apache 2.0 | 250 à 290 Mo, rétention courte |
| node_exporter | métriques des hôtes | Apache 2.0 | 10 à 20 Mo par nœud |
| Grafana | tableaux de bord | AGPLv3 | 150 à 250 Mo |
| Alertmanager | routage des alertes | Apache 2.0 | 30 à 60 Mo |

## 8. Forge et intégration continue

La forge est **GitLab CE en paquet Omnibus**, avec son **Runner**, **hors du
cluster**. Elle porte le dépôt, les tickets, les pipelines et le registre
d’images. Le paquet Omnibus embarque son propre **PostgreSQL** et son propre
**Redis** ; ces deux composants ne sont pas administrés séparément.

GitLab est réglé à **4 Go** sur SRV-1, alors que l’éditeur recommande 8 Go :
les pipelines doivent rester courts. C’est un arbitrage assumé, lié à la
capacité du parc.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| GitLab CE Omnibus | forge, pipelines, registre | MIT | 3,5 à 4,5 Go, réglé à 4 Go |
| GitLab Runner | exécution des pipelines | MIT | 100 à 200 Mo hors exécution |

## 9. Secrets

Les secrets sont gérés par **OpenBao**, déployé **hors du cluster sur SRV-1**.
OpenBao garde les **secrets applicatifs et de service** hors cluster. Les
**secrets de déploiement** sont chiffrés par **ansible-vault** (inclus dans
Ansible, GPLv3). OpenBao est le remplacement direct de HashiCorp Vault, avec la
même API et la même interface en ligne de commande, sous **MPL 2.0**.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| OpenBao | coffre de secrets hors cluster | MPL 2.0 | 100 à 200 Mo |

## 10. Scan de vulnérabilités

Le scan est assuré par **Trivy** : images, dépendances et configuration. Trivy
s’exécute dans le pipeline Docker. **Nuclei** reste une **option** pour les
sondes applicatives ; il n’est pas exigé.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| Trivy | scan des images, dépendances et configuration | Apache 2.0 | négligeable, exécution ponctuelle |
| Nuclei | sondes applicatives (option) | MIT | négligeable, exécution ponctuelle |

## 11. Filtrage réseau des hôtes

Le filtrage des hôtes repose sur **UFW** et **Fail2ban**. Trois règles sont
indispensables pour que le cluster conserve son quorum :

- autoriser le port **6443** (API de k3s) entre les nœuds ;
- autoriser le port **10250** (kubelet) entre les nœuds ;
- autoriser le port **8472/udp** (recouvrement réseau de k3s) entre les nœuds.

Le **réseau des pods**, c’est-à-dire le réseau interne des conteneurs, doit
également être autorisé. Enfin, la chaîne de règles **`DOCKER-USER`** est
restreinte : Docker contourne UFW par défaut, et sans cette règle le pare-feu
devient décoratif. UFW reste l’outil retenu pour sa
lisibilité ; un filtrage direct plus fin n’est pas retenu.

| Composant | Rôle | Licence | Empreinte indicative |
| --- | --- | --- | --- |
| UFW | pare-feu des hôtes | GPLv3 | négligeable (noyau) |
| Fail2ban | blocage des tentatives répétées | GPLv2 | 20 à 40 Mo |

## 12. Licences des outils retenus

Les empreintes sont des ordres de grandeur, jamais des mesures. Le plafond de
300 Mo s’applique par composant ; les paquets de déploiement comme
kube-prometheus-stack n’ont pas d’empreinte propre, leurs composants sont comptés
séparément. Les valeurs dépassant 300 Mo sont justifiées, pour les composants
déployés, par le rôle : base de données, journalisation, forge et orchestration.

| Outil | Licence | Empreinte indicative | Justification de l’exception |
| --- | --- | --- | --- |
| FastAPI | MIT | 80 à 150 Mo par processus de travail | — |
| Uvicorn | BSD 3-Clause | 20 à 40 Mo par processus de travail | — |
| Gunicorn | MIT | 20 à 40 Mo par processus de travail | — |
| SQLAlchemy | MIT | inclus | — |
| Alembic | MIT | négligeable | — |
| PyMySQL | MIT | moins de 20 Mo | — |
| Pydantic | MIT | inclus | — |
| argon2-cffi | MIT | moins de 20 Mo | — |
| Tailwind CSS | MIT | 0 Mo à l’exécution | — |
| k3s | Apache 2.0 | 400 à 600 Mo par nœud | orchestrateur du site |
| containerd | Apache 2.0 | 100 à 150 Mo par nœud | — |
| Docker Engine | Apache 2.0 | 150 à 250 Mo | — |
| Nginx Ingress Controller | Apache 2.0 | 50 à 100 Mo par nœud | — |
| cert-manager | Apache 2.0 | 50 à 80 Mo | — |
| MySQL 8 Community | GPLv2 (exception FOSS) | 1,0 à 1,5 Go par nœud | base de données |
| MySQL Shell | GPLv2 | négligeable | — |
| MySQL Router | GPLv2 | 30 à 50 Mo par nœud | — |
| XtraBackup | GPLv2 | négligeable | — |
| Restic | BSD 2-Clause | négligeable | — |
| MariaDB | GPLv2 | 1,0 à 1,5 Go par nœud | variante non déployée, non comptée |
| Elasticsearch | AGPLv3 (depuis 8.16) | 2,5 à 3 Go | journalisation, tas limité à 2 Go |
| Kibana | AGPLv3 (depuis 8.16) | 250 Mo | avec ressources limitées |
| Fluent Bit | Apache 2.0 | 30 à 60 Mo par nœud | — |
| kube-prometheus-stack | Apache 2.0 | inclus dans les composants ci-dessous | paquet de déploiement, aucune empreinte propre |
| Prometheus | Apache 2.0 | 250 à 290 Mo | rétention courte, sous le plafond |
| node_exporter | Apache 2.0 | 10 à 20 Mo par nœud | — |
| Grafana | AGPLv3 | 150 à 250 Mo | — |
| Alertmanager | Apache 2.0 | 30 à 60 Mo | — |
| GitLab CE Omnibus | MIT | 3,5 à 4,5 Go, réglé à 4 Go | forge |
| GitLab Runner | MIT | 100 à 200 Mo hors exécution | — |
| OpenBao | MPL 2.0 | 100 à 200 Mo | — |
| Trivy | Apache 2.0 | négligeable | — |
| Nuclei | MIT | négligeable | option, non comptée |
| UFW | GPLv3 | négligeable | — |
| Fail2ban | GPLv2 | 20 à 40 Mo | — |
| Ansible | GPLv3 | négligeable | — |
| ansible-vault | GPLv3 | inclus dans Ansible | chiffrement des secrets de déploiement |

## 13. Outils écartés et motif

| Outil écarté | Licence | Motif de l’écart |
| --- | --- | --- |
| Podman | Apache 2.0 | doublon d’orchestration : k3s porte le site et Docker Engine la CI |
| Proxmox VE | AGPLv3 | couche d’hyperviseur inutile, les serveurs sont fournis nus |
| PostgreSQL (applicatif) | PostgreSQL License | le client a retenu MySQL ; PostgreSQL n’apparaît que dans GitLab Omnibus |
| Keycloak | Apache 2.0 | service d’identité trop lourd ; sessions FastAPI et Argon2 suffisent |
| nftables (filtrage direct) | GPLv2 | UFW retenu pour sa lisibilité ; le filtrage direct est plus puissant mais moins lisible pour un jury |
| Vue | MIT | impose une chaîne Node ; remplacé par JavaScript vanilla et Tailwind autonome |
| Vite | MIT | outil de construction Node, supprimé avec Vue |
| Pinia | MIT | gestion d’état liée à Vue, sans objet sans Vue |
| Node | MIT | chaîne de construction JavaScript évitée |
| Django | BSD 3-Clause | second framework pour le même back-office ; FastAPI couvre le besoin |
| Celery | BSD 3-Clause | file de tâches sans besoin identifié |
| RabbitMQ | MPL 2.0 | courtier de messages inutile sans Celery |
| Meilisearch | MIT | moteur de recherche hors périmètre |
| Valkey | BSD 3-Clause | cache non nécessaire ; évite un service de plus |
| Garage | AGPLv3 | stockage objet hors périmètre |
| MinIO | AGPLv3 | stockage objet hors périmètre |
| Caddy | Apache 2.0 | remplacé par Nginx Ingress et cert-manager |
| ModSecurity | Apache 2.0 | pare-feu applicatif trop lourd pour 24 Go ; Trivy couvre le scan |
| Loki | AGPLv3 | le client a demandé EFK |
| Promtail | AGPLv3 | Fluent Bit retenu pour la collecte |
| Wazuh | GPLv2 | SIEM (gestion des journaux de sécurité) trop lourd ; EFK et Trivy couvrent les preuves |
| Suricata | GPLv2 | détection réseau trop lourde ; Trivy et les journaux suffisent |
| OpenVAS | AGPLv3 | 1,5 à 3 Go pour un résultat couvert par Trivy |
| Matomo | GPLv3 | mesure d’audience hors périmètre |
| Jaeger | Apache 2.0 | traçage distribué hors périmètre |
| Varnish | BSD 2-Clause | cache HTTP local sans besoin |
| Ceph | LGPL 2.1 | stockage distribué trop lourd ; disques locaux et Restic |
| SOPS | MPL 2.0 | remplacé par OpenBao, coffre centralisé |
| HashiCorp Vault | BUSL 1.1 | licence non approuvée par l’OSI ; OpenBao retenu |
| BorgBackup | BSD 3-Clause | Restic retenu comme outil unique de sauvegarde |
| OpenTofu | MPL 2.0 | Ansible suffit pour trois serveurs sans API cloud |
| Traefik | MIT | remplacé par Nginx Ingress |
| ServiceLB | Apache 2.0 | suppose une répartition par adresse de nœud ; aucune adresse virtuelle |
| MetalLB | Apache 2.0 | exige une plage d’adresses virtuelle, non disponible |
| Logstash | Elastic 2.0 | licence non couverte par l’option AGPL |
| MailHog | MIT | outil de développement, aucun service de messagerie en périmètre |
| Haraka | MIT | serveur de messagerie hors périmètre |
| MongoDB | SSPL 1.0 | licence non approuvée par l’OSI |

## 14. Dimensionnement du parc

Le parc compte **trois serveurs par équipe**, chacun avec **8 Go de mémoire**,
**4 à 8 cœurs** et **250 Go de disque**, sur le **réseau interne du LAB**. Une
**baie de stockage partagée** complète le parc, soit **quatre machines au total** ;
elle ne porte aucun service et n’entre pas dans le calcul de mémoire.
Le total disponible est de **24 Go** ; les valeurs ci-dessous sont des ordres de
grandeur, pas des mesures.

Chaque composant n’est compté qu’une fois : **containerd** est compté
séparément de **k3s** ; **MySQL Router**, **Fluent Bit** et **node_exporter**
sont comptés une fois sur chacun des trois nœuds. Les postes agrégés (« système
et agents », « CI », « application ») sont des sommes de processus dont aucun ne
dépasse 300 Mo ; les composants au-delà du plafond sont les quatre exceptions
justifiées à la section 12.

### SRV-1 — serveur saturé

Rôle : forge, intégration continue, secrets et premier nœud MySQL.

| Poste | Services | Consommation estimée | Marge |
| --- | --- | --- | --- |
| Système et agents | système, Fail2ban, Ansible | 0,6 Go | — |
| Orchestration | k3s (server et agent) | 0,5 Go | — |
| Orchestration | containerd | 0,15 Go | — |
| Entrée | Nginx Ingress | 0,1 Go | — |
| Forge | GitLab CE Omnibus réglé à 4 Go | 4,0 Go | — |
| CI | Docker Engine et GitLab Runner | 0,3 Go | — |
| Données | nœud MySQL 1 | 1,2 Go | — |
| Données | MySQL Router | 0,05 Go | — |
| Journalisation | Fluent Bit | 0,05 Go | — |
| Supervision | node_exporter | 0,02 Go | — |
| Secrets | OpenBao | 0,15 Go | — |
| **Total** | | **7,12 Go** | **0,88 Go** |

SRV-1 est le serveur saturé : il porte la forge à 4 Go réglés, un nœud MySQL et
l’entrée. Toute addition de service impose le retrait d’un autre.

### SRV-2 — journalisation

Rôle : journalisation et deuxième nœud MySQL.

| Poste | Services | Consommation estimée | Marge |
| --- | --- | --- | --- |
| Système et agents | système, Fail2ban, Ansible | 0,6 Go | — |
| Orchestration | k3s (server et agent) | 0,5 Go | — |
| Orchestration | containerd | 0,15 Go | — |
| Entrée | Nginx Ingress | 0,1 Go | — |
| Données | nœud MySQL 2 | 1,2 Go | — |
| Données | MySQL Router | 0,05 Go | — |
| Journalisation | Elasticsearch, tas limité à 2 Go | 2,8 Go | — |
| Journalisation | Kibana | 0,25 Go | — |
| Journalisation | Fluent Bit | 0,05 Go | — |
| Supervision | node_exporter | 0,02 Go | — |
| **Total** | | **5,72 Go** | **2,28 Go** |

### SRV-3 — supervision et application

Rôle : supervision, application et troisième nœud MySQL.

| Poste | Services | Consommation estimée | Marge |
| --- | --- | --- | --- |
| Système et agents | système, Fail2ban, Ansible | 0,6 Go | — |
| Orchestration | k3s (server et agent) | 0,5 Go | — |
| Orchestration | containerd | 0,15 Go | — |
| Entrée | Nginx Ingress | 0,1 Go | — |
| Données | nœud MySQL 3 | 1,2 Go | — |
| Données | MySQL Router | 0,05 Go | — |
| Application | FastAPI servi par k3s : Gunicorn et deux processus Uvicorn | 0,35 Go | — |
| Journalisation | Fluent Bit | 0,05 Go | — |
| Supervision | Prometheus | 0,3 Go | — |
| Supervision | Grafana | 0,2 Go | — |
| Supervision | Alertmanager | 0,05 Go | — |
| Supervision | node_exporter | 0,02 Go | — |
| **Total** | | **3,57 Go** | **4,43 Go** |

L’application est hébergée par **SRV-3** dans ce dimensionnement : deux processus
de travail à 0,15 Go chacun (FastAPI 0,12 Go et Uvicorn 0,03 Go), plus Gunicorn à
0,03 Go, soit 0,33 Go calculés et 0,35 Go retenus en ordre de grandeur
conservateur. k3s peut la replanifier sur un autre nœud dans la marge
disponible.

**Total engagé du parc : 16,41 Go sur 24 Go**, soit une marge de **7,59 Go**. La marge
de SRV-3 absorbe les pics de supervision et de l’application ; celle de SRV-2
absorbe la croissance des journaux dans la limite de la capacité disponible et de
la politique de rétention décrite à la section 6. SRV-1 reste le serveur saturé,
avec **0,88 Go** de marge.

## 15. Catalogue d’options non obligatoire

Les outils ci-dessous ne sont **pas exigés**. Chacun est une **option** que
l’équipe peut activer si le besoin apparaît, sans modifier la pile arrêtée.

| Couche | Outil | Licence | Intérêt | Statut |
| --- | --- | --- | --- | --- |
| Application | HTMX | MIT | interactions serveur sans cadre JavaScript lourd | option |
| Application | Alpine.js | MIT | réactivité légère dans la page | option |
| Application | httpx | BSD 3-Clause | client HTTP pour les tests | option |
| Application | pytest | MIT | tests automatisés | option |
| Application | Ruff | MIT | analyse statique et formatage | option |
| Application | mypy | MIT | vérification des types | option |
| Application | OpenTelemetry SDK | Apache 2.0 | traces applicatives | option |
| Application | Locust | MIT | tests de charge | option |
| Orchestration | Helm | Apache 2.0 | paquets de déploiement | option |
| Orchestration | Kustomize | Apache 2.0 | surcharges de configuration | option |
| Orchestration | Argo CD | Apache 2.0 | déploiement piloté par le dépôt Git | option |
| Orchestration | Flux | Apache 2.0 | déploiement piloté par le dépôt Git | option |
| Orchestration | Longhorn | Apache 2.0 | stockage bloc dans le cluster | option |
| Données | ProxySQL | GPLv3 | réserve de connexions MySQL | option |
| Données | SeaweedFS | Apache 2.0 | stockage objet compatible S3 | option |
| Données | Percona Toolkit | GPLv2 | maintenance de MySQL | option |
| Données | mydumper | GPLv3 | exports parallèles | option |
| Entrée | HAProxy | GPLv2 | répartition de charge | option |
| Entrée | Envoy | Apache 2.0 | proxy programmable | option |
| Entrée | Coraza | Apache 2.0 | pare-feu applicatif | option |
| Entrée | external-dns | Apache 2.0 | mise à jour automatique du DNS | option |
| Journalisation | Vector | MPL 2.0 | collecte de journaux | option |
| Journalisation | OpenSearch | Apache 2.0 | recherche dans les journaux | option |
| Journalisation | Grafana Alloy | Apache 2.0 | collecte de journaux et métriques | option |
| Supervision | Uptime Kuma | MIT | surveillance de disponibilité | option |
| Supervision | Blackbox exporter | Apache 2.0 | sondes HTTP et réseau | option |
| Supervision | mysqld_exporter | Apache 2.0 | métriques MySQL | option |
| Supervision | kube-state-metrics | Apache 2.0 | état des objets du cluster | option |
| Supervision | ntfy | Apache 2.0 | notifications | option |
| Forge | Forgejo | GPLv3 | forge légère | option |
| Forge | Woodpecker CI | Apache 2.0 | intégration continue | option |
| Forge | Renovate | AGPLv3 | mises à jour de dépendances | option |
| Forge | Syft | Apache 2.0 | liste des composants logiciels (SBOM) | option |
| Secrets | External Secrets Operator | Apache 2.0 | synchronisation des secrets | option |
| Secrets | Sealed Secrets | Apache 2.0 | secrets chiffrés dans Git | option |
| Scan | Grype | Apache 2.0 | vulnérabilités des images | option |
| Scan | Semgrep | LGPL 2.1 | analyse statique du code | option |
| Scan | Gitleaks | MIT | détection de secrets dans Git | option |
| Réseau des hôtes | CrowdSec | MIT | protection collaborative | option |
| Réseau des hôtes | WireGuard | GPLv2 | tunnel chiffré | option |
| Sauvegarde | Kopia | Apache 2.0 | sauvegarde de fichiers | option |
| Sauvegarde | Velero | Apache 2.0 | sauvegarde du cluster | option |
| Sauvegarde | resticprofile | MIT | profils de sauvegarde Restic | option |
| Documentation | MkDocs | BSD 2-Clause | documentation statique | option |
