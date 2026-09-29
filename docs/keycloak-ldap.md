# Keycloak — Intégration LDAP & client Wiki.js

## 1. Déploiement

Keycloak est déployé via Docker Compose (voir [`configs/docker/docker-compose.yml.example`](../configs/docker/docker-compose.yml.example)), avec une base PostgreSQL dédiée.

```bash
cd /opt/projet-keycloak
docker compose up -d
docker ps
```

## 2. Import des utilisateurs depuis l'Active Directory (LDAP)

Dans l'interface Keycloak, realm `example` → **User Federation → Add LDAP provider** :

| Paramètre | Valeur (exemple) |
|-----------|------------------|
| Connection URL | `ldap://192.168.x.10:389` |
| Bind Type | `simple` |
| Bind DN | `CN=Administrateur,CN=Users,DC=example,DC=local` |
| Bind Credentials | `<MOT_DE_PASSE_ADMIN>` |
| Edit Mode | `READ_ONLY` |
| Users DN | `OU=Utilisateurs,DC=example,DC=local` |
| Username LDAP attribute | `sAMAccountName` |
| Import Users | `On` |

Import : **Action → Sync all users**.

## 3. Création du client Keycloak pour Wiki.js

Dans **Clients → Create Client** :

| Paramètre | Valeur (exemple) |
|-----------|------------------|
| Client ID | `wikijs` |
| Root URL | `http://wikijs.example.local:8082` |
| Valid redirect URIs | `http://wikijs.example.local:8082/login/<ID>/callback` |
| Web origins | `*` |
| Client authentication | `On` |
| Standard Flow | Activé |
| Direct access grants | Activé |

> Le **Client Secret** est récupéré dans l'onglet **Credentials** du client créé — il sera utilisé côté Wiki.js.
