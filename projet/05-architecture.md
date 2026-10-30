# Architecture Technique Complète

## 1. Vue d’ensemble

```
                          ┌─────────────────────┐
                          │    INTERNET          │
                          └──────────┬──────────┘
                                     │
                          ┌──────────▼──────────┐
                          │   PARE-FEU           │
                          │   nftables           │
                          │   (ports 80, 443)    │
                          └──────────┬──────────┘
                                     │
                          ┌──────────▼──────────┐
                          │   IDS/IPS            │
                          │   Suricata           │
                          └──────────┬──────────┘
                                     │
                          ┌──────────▼──────────┐
                          │   WAF                │
                          │   ModSecurity/Nginx  │
                          └──────────┬──────────┘
                                     │
              ┌──────────────────────┼──────────────────────┐
              │                      │                      │
     ┌────────▼────────┐  ┌─────────▼─────────┐  ┌────────▼────────┐
      │   REVERSE PROXY  │  │   REVERSE PROXY    │  │   UPTIME KUMA   │
      │ Caddy (principal)│  │ Caddy (secondaire)│  │   (Monitoring)  │
      │   + Let's Encrypt│  │   (Failover)       │  │   Port 3001     │
     └────────┬────────┘  └─────────┬─────────┘  └─────────────────┘
              │                      │
              └──────────┬───────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼───────┐ ┌──────▼──────┐ ┌──────▼──────┐
│  PODMAN       │ │  PODMAN     │ │  PODMAN     │
│  NETWORK      │ │  NETWORK    │ │  NETWORK    │
│  APP          │ │  DATA       │ │  MONITORING │
└───────┬───────┘ └──────┬──────┘ └──────┬──────┘
        │                │                │
```

---

## 2. Réseau

### 2.1 Topologie

```
Internet
   │
   ▼
┌──────────────────────────────────────────────────────────┐
│                    RESEAU PUBLIC (DMZ)                     │
│  VLAN 10: 192.168.10.0/24                                │
│  - Caddy (Reverse Proxy)         .10                     │
│  - Uptime Kuma (Monitoring ext)  .11                     │
│  - Suricata (IDS/IPS)            .12                     │
└──────────────────────┬───────────────────────────────────┘
                       │
              ┌────────▼────────┐
│   PARE-FEU      │
│   nftables      │
│   VLAN 10 → 20  │
└────────┬────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    RESEAU APPLICATION                     │
│  VLAN 20: 192.168.20.0/24                                │
│  - FastAPI (Backend API)        .20                      │
│  - Django (Admin)               .21                      │
│  - Vue.js (Frontend)            .22                      │
│  - Celery Workers               .23                      │
│  - RabbitMQ                     .24                      │
│  - Keycloak (IAM)               .25                      │
└──────────────────────┬───────────────────────────────────┘
                       │
              ┌────────▼────────┐
              │   PARE-FEU      │
              │   nftables      │
              │   VLAN 20 → 30  │
              └────────┬────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    RESEAU DONNEES                         │
│  VLAN 30: 192.168.30.0/24                                │
│  - PostgreSQL                .30                         │
│  - Valkey (Cache)            .31                         │
│  - Garage (Stockage objets)  .32                         │
│  - Meilisearch (Recherche)   .33                         │
└──────────────────────┬───────────────────────────────────┘
                       │
              ┌────────▼────────┐
              │   PARE-FEU      │
              │   nftables      │
              │   VLAN 30 → 40  │
              └────────┬────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    RESEAU ADMINISTRATION                  │
│  VLAN 40: 192.168.40.0/24                                │
│  - Prometheus                .40                         │
│  - Grafana                   .41                         │
│  - Loki                      .42                         │
│  - Wazuh (SIEM)              .43                         │
│  - Ansible (Config Mgmt)     .44                         │
│  - GitLab CE (service cliente) .45                        │
└──────────────────────────────────────────────────────────┘
```

### 2.2 Règles pare-feu (nftables)

```
# VLAN 10 (DMZ) → VLAN 20 (App) : HTTP/HTTPS uniquement
# VLAN 20 (App) → VLAN 30 (Data) : ports BDD/cache uniquement
# VLAN 40 (Admin) → tous les VLANs : SSH + monitoring
# Bloquer tout le reste par défaut
```

---

## 3. Architecture applicative

### 3.1 Backend API (FastAPI)

```
app/backend/api/
├── main.py              # Point d'entrée FastAPI
├── config.py            # Configuration (env vars)
├── database.py          # Connexion PostgreSQL
├── models/              # SQLAlchemy ORM
│   ├── user.py
│   ├── product.py
│   ├── category.py
│   ├── cart.py
│   ├── order.py
│   └── payment.py
├── schemas/             # Pydantic schemas
│   ├── user.py
│   ├── product.py
│   └── order.py
├── routers/             # Routes API
│   ├── auth.py          # Inscription, connexion, JWT
│   ├── users.py         # Profil, adresses
│   ├── products.py      # Catalogue, recherche
│   ├── cart.py          # Panier
│   ├── orders.py        # Commandes
│   └── admin.py         # Routes admin
├── middleware/           # Middleware
│   ├── auth.py          # JWT validation
│   ├── cors.py
│   └── logging.py       # Audit trail
├── tasks/               # Celery tasks
│   ├── email.py
│   ├── stock.py
│   └── gdpr.py
└── tests/
    ├── test_auth.py
    ├── test_products.py
    └── test_orders.py
```

### 3.2 Backend Admin (Django)

```
app/backend/admin/
├── config/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── catalog/             # Gestion catalogue
├── orders/              # Gestion commandes
├── analytics/           # Tableaux de bord
├── users/               # Gestion utilisateurs
└── gdpr/                # Module RGPD
```

### 3.3 Frontend (Vue.js 3)

```
app/frontend/
├── src/
│   ├── views/
│   │   ├── Home.vue
│   │   ├── Catalogue.vue
│   │   ├── Product.vue
│   │   ├── Cart.vue
│   │   ├── Checkout.vue
│   │   ├── Account.vue
│   │   └── admin/
│   │       ├── Dashboard.vue
│   │       ├── Products.vue
│   │       └── Orders.vue
│   ├── components/
│   │   ├── Header.vue
│   │   ├── ProductCard.vue
│   │   ├── CartDrawer.vue
│   │   └── SearchBar.vue
│   ├── stores/          # Pinia stores
│   ├── router/          # Vue Router
│   └── utils/
├── package.json
└── vite.config.js
```

---

## 4. Architecture de déploiement

### 4.1 Serveurs

| Serveur | Rôle | OS | RAM | CPU | Stockage |
|---------|------|-----|-----|-----|----------|
| **SRV-APP** | Application (Podman) | Ubuntu 22.04 | 32GB | 8 cœurs | 500GB SSD |
| **SRV-DATA** | BDD + Cache + Stockage | Ubuntu 22.04 | 64GB | 16 cœurs | 2TB NVMe |
| **SRV-MONIT** | Monitoring + CI/CD | Ubuntu 22.04 | 16GB | 4 cœurs | 500GB SSD |

### 4.2 Podman Compose (Production)

```yaml
# SRV-APP — Services d'application (podman compose)
services:
  caddy:        # Reverse proxy + terminaison TLS (Apache 2.0)
  fastapi:      # API Backend
  django:       # Admin Backend
  vuejs:        # Frontend (build statique)
  celery:       # Workers tâches async
  rabbitmq:     # File de messages (MPL 2.0)
  keycloak:     # IAM / Authentification (Apache 2.0)

# SRV-DATA — Services de données
services:
  postgresql:   # Base de données principale
  valkey:       # Cache + sessions (BSD, fork de Redis)
  garage:       # Stockage objets (AGPL, API S3-compatible)
  meilisearch:  # Moteur de recherche (MIT)

# SRV-MONIT — Services d'administration
services:
  prometheus:   # Collecte métriques (Apache 2.0)
  grafana:      # Dashboards (AGPL)
  loki:         # Logs centralisés (AGPL)
  alertmanager: # Alertes (Apache 2.0)
  wazuh:        # SIEM / Audit (GPLv2)
  gitlab:       # Git + CI/CD + Registry — service de l'infrastructure cliente (MIT)
  gitlab-runner: # Runner CI/CD pour exécuter les pipelines
```

---

## 5. GitLab CI/CD — service de l’infrastructure cliente

GitHub Actions porte l’intégration continue du projet. GitLab CE est le service auto-hébergé de l’infrastructure cliente ; les pipelines décrits ci-dessous concernent le déploiement de cette infrastructure.

### 5.1 Pipeline `.gitlab-ci.yml`

```yaml
stages:
  - test
  - build
  - deploy

# Tests automatiques
test-backend:
  stage: test
  image: python:3.11-slim
  script:
    - pip install -r requirements.txt
    - pytest tests/ --cov=app --cov-report=xml
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage.xml

test-frontend:
  stage: test
  image: node:20-alpine
  script:
    - npm ci
    - npm run test:unit
    - npm run lint

# Build des images conteneurs
build-backend:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - podman build -t $CI_REGISTRY_IMAGE/backend:$CI_COMMIT_SHA ./app/backend/api
    - podman push $CI_REGISTRY_IMAGE/backend:$CI_COMMIT_SHA
  only:
    - main

build-frontend:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - podman build -t $CI_REGISTRY_IMAGE/frontend:$CI_COMMIT_SHA ./app/frontend
    - podman push $CI_REGISTRY_IMAGE/frontend:$CI_COMMIT_SHA
  only:
    - main

# Déploiement
deploy-production:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | ssh-add -
  script:
    - ssh $DEPLOY_USER@$DEPLOY_HOST "cd /opt/materielsgaming && podman-compose pull && podman-compose up -d"
  only:
    - main
  when: manual
```

### 5.2 GitLab Container Registry

GitLab CE intègre un registry Docker/Podman pour stocker les images conteneurs :
- URL : `registry.gitlab.${DOMAIN}`
- Authentification : Jetons d’accès GitLab
- Utilisation : `podman push registry.gitlab.${DOMAIN}/group/project/image:tag`

### 5.3 GitLab Runner

Le GitLab Runner exécute les jobs CI/CD :
- **Type** : Shell executor (pour les déploiements) + Docker executor (pour les tests)
- **Installation** : Conteneur Podman sur SRV-MONIT
- **Registration** : Token GitLab + configuration `.gitlab-runner/config.toml`

---

## 6. Flux de données

```
Utilisateur → Internet
     │
     ▼
  [nftables] → Filter traffic
     │
     ▼
  [Suricata] → Detect intrusions
     │
     ▼
  [ModSecurity] → WAF rules
     │
     ▼
  [Caddy] → TLS termination + reverse proxy
     │
     ├── /api/* ──→ FastAPI (Port 8000)
     │                │
     │                ├── PostgreSQL (queries)
     │                ├── Valkey (cache)
     │                ├── Meilisearch (search)
     │                ├── Garage (images)
     │                └── Stripe (paiement externe)
     │
     ├── /admin/* ──→ Django (Port 8001)
     │
     ├── /* ──→ Vue.js statique (Nginx)
     │
     └── /auth/* ──→ Keycloak (Port 8080)

  [Prometheus] ← métriques de tous les services
  [Loki] ← logs de tous les services
  [Wazuh] ← audit trail + alertes sécurité
```

---

## 7. Haute disponibilité

| Composant | Stratégie HA | Détail |
|-----------|-------------|--------|
| Reverse Proxy | 2 instances Caddy | Keepalived + VIP |
| Application | 2 conteneurs FastAPI | Répartition de charge circulaire |
| BDD PostgreSQL | Streaming replication | Primary + Standby |
| Cache Valkey | Sentinel | 1 primary + 2 replicas |
| Stockage Garage | Réplication | 3 nœuds, tolérance 1 panne |
| Monitoring | Instance unique | Backup quotidienne |

---

## 8. Plan de reprise d’activité (PRA)

| Scénario | Délai reprise | Procédure |
|----------|--------------|-----------|
| Panne service applicatif | < 5 min | Redémarrage auto (Podman healthcheck + systemd) |
| Panne serveur BDD | < 15 min | Promotion standby → primary |
| Panne disque | < 30 min | Remplacement disque + restauration BorgBackup |
| Panne serveur complet | < 2h | Restauration sur serveur de secours |
| Catastrophe (incendie, inondation) | < 24h | Restauration depuis backup externe chiffré |
