# Contexte du Projet

## Présentation

| Élément | Détail |
|---------|--------|
| **Laboratoire** | Devincilab (devincilab.fr) |
| **Client** | materielsgaming.com |
| **Secteur** | Grossiste / détaillant matériel informatique, gaming, GPU, mini datacenter IA |
| **Cibles** | Gamers, particuliers, entreprises IA, PME (mini datacenter) |
| **Stack** | Python (FastAPI + Django) + Vue.js 3 |
| **Infra** | Infrastructure sur site (serveurs propriétaires du client) |
| **Conformité** | RGPD + Politique de sécurité documentée |
| **Équipe** | 2 à 5 personnes |
| **Durée** | Septembre 2026 → Juin 2027 (environ 10 mois) |

---

## Présentation de materielsgaming.com

materielsgaming.com est un acteur spécialisé dans la vente de matériel informatique haut de gamme avec trois cibles principales :

1. **Gamers / Particuliers** : Cartes graphiques, périphériques gaming, accessoires
2. **Entreprises spécialisées en IA** : GPU professionnels (NVIDIA A100, H100), serveurs IA
3. **PME souhaitant leur propre mini datacenter** : Solutions complètes d’infrastructure IA on-premise

### Positionnement stratégique
- **Souveraineté numérique** : L’entreprise souhaite maîtriser l’ensemble de sa chaîne technique
- **Indépendance** : Ne pas dépendre des grands fournisseurs cloud (AWS, Azure, GCP)
- **Open source** : Utiliser des solutions open source pour remplacer les services propriétaires cloud
- **Conformité** : RGPD, politique de sécurité documentée

---

## Besoins identifiés

### Besoins fonctionnels
- Plateforme e-commerce B2B + B2C
- Catalogue multi-catégories (gaming, IA pro, mini datacenter)
- Système de panier et de commande
- Authentification multi-rôles (client, pro, admin)
- Gestion des stocks et approvisionnements
- Tableau de bord administrateur
- Recherche avancée de produits
- Intégration paiement sécurisé

### Besoins non fonctionnels
- **Disponibilité** : 99,9 % (SLA)
- **Performance** : Temps de réponse < 200 ms pour la recherche
- **Sécurité** : Chiffrement, authentification forte, piste d’audit
- **Scalabilité** : Montée en charge pour pics de ventes (Black Friday, soldes)
- **Conformité** : RGPD, souveraineté des données
- **Souveraineté** : Infrastructure 100 % sur site, aucune donnée hors UE

---

## Objectifs du projet

1. Développer la plateforme e-commerce complète
2. Déployer l’infrastructure on-premise avec des solutions open source
3. Mettre en place une politique de sécurité documentée
4. Garantir la conformité RGPD
5. Documenter l’ensemble pour la certification RNCP38478
6. Assurer la transition et la formation de l’équipe cliente
