# Wiki.js — Authentification SSO (OpenID Connect)

## Accès et activation

Accès à l'interface web : `http://wikijs.example.local:8082`.

1. Installation initiale et création du compte administrateur local.
2. **Administration → Authentication → + ADD STRATEGY → Generic OpenID Connect / OAuth2**.

## Paramètres OpenID Connect

| Paramètre | Valeur (exemple) |
|-----------|------------------|
| Client ID | `wikijs` |
| Client Secret | `<CLIENT_SECRET récupéré dans Keycloak>` |
| Authorization Endpoint URL | `http://keycloak.example.local:8080/realms/example/protocol/openid-connect/auth` |
| Token Endpoint URL | `http://keycloak.example.local:8080/realms/example/protocol/openid-connect/token` |
| User Info Endpoint URL | `http://keycloak.example.local:8080/realms/example/protocol/openid-connect/userinfo` |
| Issuer | `http://keycloak.example.local:8080/realms/example` |
| Email Claim | `email` |

## Résultat

Sur la page de connexion Wiki.js, un choix apparaît : **Local** ou **Keycloak**.
En choisissant Keycloak, l'utilisateur est redirigé vers la mire d'authentification du realm, saisit ses identifiants **Active Directory**, puis accède à Wiki.js avec les droits associés à son compte.

## Gestion des permissions

Les droits (lecture, écriture, commentaires, administration) sont gérés dans Wiki.js via les **groupes** et **rôles**, applicables aux utilisateurs importés depuis l'AD.
