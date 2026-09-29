# Active Directory & DNS (Windows Server 2022)

## 1. Configuration réseau (statique)

| Paramètre | Valeur (exemple) |
|-----------|------------------|
| Adresse IP | `192.168.x.10` |
| Masque | `255.255.255.0` |
| Passerelle | `192.168.x.2` |
| DNS | `192.168.x.10` (lui-même, une fois l'AD installé) |

## 2. Installation du rôle AD DS

Gestionnaire de serveur → **Gérer → Ajouter des rôles et fonctionnalités → AD DS**.

Promotion en contrôleur de domaine — **Ajout d'une nouvelle forêt** :

| Paramètre | Valeur (exemple) |
|-----------|------------------|
| Nom de domaine | `example.local` |
| NetBIOS | `EXAMPLE` |

Après redémarrage, connexion avec le compte `EXAMPLE\Administrateur`.

## 3. Unités d'organisation & utilisateurs

Création d'une OU « Utilisateurs » contenant des sous-OU (ex : IT, RH, Sales).

| Nom complet | Login (sAMAccountName) | E-mail | OU |
|-------------|------------------------|--------|-----|
| Utilisateur 1 | `user1` | `user1@example.com` | IT |
| Utilisateur 2 | `user2` | `user2@example.com` | RH |
| Utilisateur 3 | `user3` | `user3@example.com` | Sales |

## 4. Configuration du DNS

Dans la zone `example.local` :

- Nouvel hôte (A) : `srv-ad` → `192.168.x.10`
- Ajout du redirecteur : `8.8.8.8`

Tests de résolution :

```bash
ping srv-ad.example.local
nslookup srv-ad.example.local
ping google.com
```
