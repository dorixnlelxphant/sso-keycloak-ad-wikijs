# 🔑 SSO Keycloak intégré à Active Directory pour Wiki.js

> Projet réalisé dans le cadre de l'épreuve **E6 – Administration des systèmes et des réseaux** du **BTS SIO option SISR** (Session 2026).

Mise en place d'une solution d'**authentification centralisée (Single Sign-On)** basée sur **Keycloak**, intégrée à un **Active Directory** via LDAP, permettant aux utilisateurs de se connecter à une application web (**Wiki.js**) avec leurs identifiants AD — sans duplication des comptes.

---

## 🎯 Objectifs

- Déployer un serveur **Active Directory** (Windows Server 2022) et un **DNS interne**
- Installer et configurer **Keycloak** via Docker (base PostgreSQL)
- Intégrer Keycloak à l'Active Directory via **LDAP**
- Déployer **Wiki.js** et le connecter à Keycloak en **SSO (OpenID Connect)**
- Gérer les permissions et droits par utilisateur

---

## 🧱 Architecture

```
  ┌──────────────── VM-WINDOWS (Windows Server 2022) ────────────────┐
  │   Active Directory DS  +  DNS  (domaine : example.local)         │
  └───────────────────────────────┬──────────────────────────────────┘
                                   │ LDAP (TCP 389)
                                   │ authentification + attributs (mail)
  ┌────────────────────────────────┴─────────────────────────────────┐
  │                     VM-DEBIAN (Docker Compose)                    │
  │  ┌──────────┐   ┌───────────────┐        ┌───────────────┐         │
  │  │ Keycloak │───│ PostgreSQL x2 │        │    Wiki.js    │         │
  │  │  (IdP)   │   └───────────────┘        │  (OpenID C.)  │         │
  │  └────┬─────┘                            └───────┬───────┘         │
  └───────┼──────────────────────────────────────────┼───────────────┘
          │  OpenID Connect (JWT)                     │
          └──────────── Utilisateur (navigateur) ─────┘
```

> Schéma détaillé : [`docs/architecture.md`](docs/architecture.md)

**Flux SSO :** l'utilisateur accède à Wiki.js → redirection vers Keycloak → Keycloak vérifie les identifiants auprès de l'AD (LDAP) → génération d'un token JWT → accès à Wiki.js.

---

## 🛠️ Stack technique

| Composant | Version | Rôle |
|-----------|---------|------|
| **Windows Server** | 2022 Standard | Active Directory DS + DNS + contrôleur de domaine |
| **Debian** | 12 | Hôte des services conteneurisés |
| **VMware Workstation Pro** | — | Virtualisation (réseau NAT) |
| **Docker / Docker Compose** | — | Déploiement des services |
| **Keycloak** | latest | Fournisseur d'identité (IdP) — SSO |
| **PostgreSQL** | 15 | Base de données (Keycloak & Wiki.js) |
| **Wiki.js** | 2 | Application web intégrée au SSO |

---

## 📂 Contenu du dépôt

```
.
├── docs/
│   ├── architecture.md          # Schéma & plan d'adressage
│   ├── active-directory.md      # Rôle AD DS, OU, utilisateurs, DNS
│   ├── keycloak-ldap.md         # Intégration LDAP + client Wiki.js
│   └── wikijs-oidc.md           # Configuration OpenID Connect côté Wiki.js
└── configs/
    ├── docker/docker-compose.yml.example
    ├── debian/interfaces.example
    └── keycloak/ldap-provider.md
```

---

## 🔑 Compétences mises en œuvre

- Administrer un serveur **Windows Server** et un **Active Directory**
- Configurer un service **DNS** interne
- Déployer des services réseau via **Docker / Docker Compose**
- Mettre en place une **authentification centralisée SSO**
- Intégrer un annuaire **LDAP** à un fournisseur d'identité
- Gérer les droits d'accès utilisateurs dans une application web

---

## ✅ Tests réalisés

- Connexion des utilisateurs via Keycloak → OK
- Import automatique des comptes depuis l'Active Directory (LDAP) → OK
- Accès à Wiki.js via SSO (OpenID Connect) → OK
- Validation des flux d'authentification (LDAP, OpenID Connect) → OK
- Gestion des droits utilisateurs dans l'application → OK

---

> ⚠️ **Note de sécurité** — Noms de domaine, adresses IP, identifiants et secrets présents dans ce dépôt sont **anonymisés / génériques**. Aucun mot de passe ni secret réel n'est exposé.

## 👤 Auteur

**Dorian Ponsada** — BTS SIO SISR 2026
Objectif : analyste SOC / cybersécurité
