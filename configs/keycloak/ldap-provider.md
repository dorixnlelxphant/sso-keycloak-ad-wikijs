# Paramètres du fournisseur LDAP Keycloak (référence rapide)

| Paramètre | Valeur (exemple) |
|-----------|------------------|
| Connection URL | `ldap://192.168.x.10:389` |
| Bind Type | `simple` |
| Bind DN | `CN=Administrateur,CN=Users,DC=example,DC=local` |
| Bind Credentials | `<MOT_DE_PASSE_ADMIN>` |
| Edit Mode | `READ_ONLY` |
| Users DN | `OU=Utilisateurs,DC=example,DC=local` |
| Username LDAP attribute | `sAMAccountName` |
| RDN LDAP attribute | `cn` |
| UUID LDAP attribute | `objectGUID` |
| User Object Classes | `person, organizationalPerson, user` |
| Import Users | `On` |

> Après configuration : **Action → Sync all users** pour importer les comptes de l'AD.
