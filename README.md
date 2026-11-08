# Gestion des congés : site fonctionnel (PHP + SQLite démo / MySQL)

Le site fonctionne **tout de suite** en mode démo (base SQLite créée automatiquement dans `data/`), puis se branche sur votre base MySQL en changeant une ligne.

## Démarrage (démo)
1. Copiez le dossier dans `htdocs` (XAMPP) ou lancez `php -S localhost:8000` dans le dossier.
2. Ouvrez `http://localhost/site-conges/` : connexion avec `demo1234` et
   `aminata.sow@entreprise.com` (Employé), `ibrahima.diop@entreprise.com` (Responsable), `khady.fall@entreprise.com` (RH).
3. Prérequis : PHP 7.4+ avec les extensions `pdo_sqlite` (démo) et `pdo_mysql` (MySQL), activées par défaut dans XAMPP.

## Passer sur votre base MySQL
1. phpMyAdmin → créez la base `WORKFLOW` (interclassement `utf8mb4_general_ci`).
2. Onglet **Importer** → `database/schema_mysql.sql` (structure + données de démo ; retirez les `INSERT` si vous ne voulez pas les comptes de test).
3. Dans `Configuration/config.php` : `'mode' => 'mysql'` puis vos identifiants.
4. **Si votre base existe déjà** avec d'autres noms de colonnes : adaptez uniquement `includes/Repo.php` (toutes les requêtes SQL y sont). Vérifiez `statut` (`EN_ATTENTE`, `APPROUVEE`, `REFUSEE`), `lu` (`N`/`O`) et les noms de rôles (`Employé`, `Responsable`, `RH`).
5. Les anciens mots de passe en clair sont acceptés une fois, puis remplacés automatiquement par un hash.

## Ce qui est inclus
Connexion sécurisée (jeton CSRF, blocage après 5 échecs, session régénérée) · tableau de bord par rôle · nouvelle demande (jours ouvrés, solde, chevauchement) · mes demandes · validation (contrôle d'accès serveur, transaction, notification, historique) · solde · calendrier d'équipe · notifications · profil et changement de mot de passe · gestion des employés, rôles, types de congés, historique, rapports (RH).

## Structure
```
Configuration/config.php   mode demo|mysql et identifiants
includes/Repo.php          toutes les requêtes SQL
includes/bootstrap.php     session, aides, sécurité
includes/layout.php        menu et en-tête communs
database/                  gabarit de schéma + version MySQL
CSS.css, animations.css, site.css, Scripts.js
conges/ solde/ calendrier/ employes/ notifications/ historique/ rapports/ parametres/
```
En production : `'debug' => false`, HTTPS, utilisateur MySQL dédié (voir `GUIDE_BASE_DE_DONNEES.md`).
