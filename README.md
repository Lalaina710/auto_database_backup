# Auto Database Backup (Odoo 18)

Module de backup automatique de base de données Odoo avec stockage vers multiples destinations.

## Origine

Module original par **Cybrosys Technologies** (Odoo Apps Store), adapté pour la production SOPROMER.

## Fonctionnalités

- Backup automatique via cron (daily/weekly/monthly)
- Format **ZIP** (base + filestore) ou **DUMP** (base seule)
- Rétention automatique (suppression des anciens backups)
- Notifications email (succès/échec)
- Test de connexion avant activation

### Destinations supportées

| Destination | Package requis |
|-------------|---------------|
| Local | aucun |
| FTP | aucun |
| SFTP | `paramiko` |
| Google Drive | aucun (OAuth2) |
| Dropbox | `dropbox` |
| OneDrive | aucun (OAuth2) |
| NextCloud | `nextcloud-api-wrapper` |
| Amazon S3 | `boto3` |

## Adaptations SOPROMER

### Sécurité (ACL)

Accès restreint à `base.group_system` (admin technique uniquement). Le module original donnait accès à tous les utilisateurs internes, exposant le master password.

### Imports conditionnels

Les packages optionnels (`boto3`, `dropbox`, `paramiko`, `nextcloud`) sont importés en `try/except`. Le module charge et fonctionne même si ces packages ne sont pas installés — erreur claire uniquement si on tente d'utiliser une destination dont le package est absent.

## Configuration recommandée (SOPROMER)

```
Destination : Local (+ rsync externe recommandé)
Format      : ZIP (inclut filestore)
Fréquence   : Daily
Rétention   : 14 jours
Backup path : /opt/odoo18/backups/ (volume Docker monté)
Cron        : 02:00 UTC (05:00 heure locale Madagascar)
Notification: Oui, vers admin
```

## Installation

1. Copier le dossier dans `addons_path`
2. Mettre à jour la liste des modules
3. Installer "Automatic Database Backup"
4. Configurer via Settings > Database Backup

### Docker

- Vérifier que `pg_dump` est disponible : `docker exec odoo-dev which pg_dump`
- Monter un volume pour les backups locaux
- Le filestore est automatiquement trouvé via la config Odoo

## Prérequis

- Odoo 18 Community ou Enterprise
- Module `base`, `mail`

## Licence

LGPL-3

## Auteurs

- **Cybrosys Technologies** — module original
- **SOPROMER** — adaptations sécurité et production
