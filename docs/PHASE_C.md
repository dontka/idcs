# INSTALLATION Gestion des Rôles & Permissions (indispensable pour distinguer Admin, Collecteur, Analyste, Utilisateur)


  ```bash
 composer require spatie/laravel-permission
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
  ```

# Authentification & Sécurité API (pour les futurs collecteurs mobile/offline) :

  ```bash
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
  ```

# Composants d'interface (Tailwind / Alpine) :

  ```bash
npm run dev
  ```
