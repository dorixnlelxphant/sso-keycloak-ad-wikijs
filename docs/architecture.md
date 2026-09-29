# Architecture technique & plan d'adressage

## Vue d'ensemble

L'infrastructure est entièrement virtualisée sous **VMware Workstation Pro**, sur un sous-réseau en mode **NAT**. Deux machines virtuelles :

- **VM-WINDOWS** : Windows Server 2022, rôles **Active Directory DS** et **DNS** (contrôleur du domaine `example.local`).
- **VM-DEBIAN** : Debian 12, héberge via **Docker Compose** les services **Keycloak**, **Wiki.js** et leurs bases **PostgreSQL**.

## Plan d'adressage (exemple)

| Machine | IP | Rôle |
|---------|-----|------|
| SRV-WINDOWS | `192.168.x.10` | AD DS, DNS, contrôleur de domaine |
| SRV-DEBIAN | `192.168.x.20` | Docker, Keycloak, Wiki.js, PostgreSQL |
| Passerelle NAT | `192.168.x.2` | Accès Internet (VMware NAT) |
| Poste client | hôte local | Accès aux interfaces web (fichier hosts) |

## Résolution de noms

Un DNS interne (ou le fichier `hosts` du client) assure la résolution :

```
192.168.x.20  →  keycloak.example.local
192.168.x.20  →  wikijs.example.local
```

## Flux d'authentification SSO (OpenID Connect)

1. L'utilisateur tente d'accéder à Wiki.js (HTTP).
2. Wiki.js redirige vers Keycloak (SSO – OpenID Connect).
3. Keycloak soumet les identifiants à l'Active Directory (LDAP, TCP 389).
4. L'AD répond (Bind) + renvoie les attributs utilisateur (mail).
5. Keycloak génère un token JWT (identité, groupes/rôles, validité, signature).
6. Wiki.js valide le token et donne accès à l'application.

## Principes mis en œuvre

- **Centralisation des identités** : un seul annuaire source (AD), pas de doublons de comptes.
- **Authentification fédérée & standardisée** : OpenID Connect.
- **Modularité** : chaque service dans son conteneur, bases PostgreSQL séparées.
- **Gestion des droits** : rôles et permissions appliqués dans l'application.
