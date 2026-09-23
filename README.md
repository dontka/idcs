# IDCS

Bienvenue dans le projet IDCS.

## Contribution

Suivez les étapes ci-dessous pour configurer et lancer le projet localement.

### Prérequis

Avant de commencer, vérifiez que vous avez installé :

- Git
- PHP
- Composer
- Node.js et npm
- Un moteur de base de données compatible (MySQL, SQLite, etc.)

### Étapes de mise en place

1. Clonez le dépôt :

   ```bash
   git clone https://github.com/dontka/schor.git
   ```

2. Accédez au dossier du projet :

   ```bash
   cd idcs
   ```

3. Installez les dépendances PHP :

   ```bash
   composer install
   ```

4. Installez les dépendances JavaScript :

   ```bash
   npm install
   ```

5. Configurez les variables d'environnement :

   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

6. Compilez les assets front :

   ```bash
   npm run dev
   ```

7. Créez et migrez la base de données :

   ```bash
   php artisan migrate
   ```

8. Lancez l'application :

   ```bash
   php artisan serve
   ```

### Accès

Ouvrez ensuite l’URL affichée dans le terminal, généralement :

```text
http://127.0.0.1:8000
```

### Commandes utiles

- Pour arrêter le serveur : `Ctrl + C`
- Pour reconstruire les assets en production :

  ```bash
  npm run build
  ```

- Pour relancer les migrations :

  ```bash
  php artisan migrate:fresh
  ```

---

Merci de contribuer à ce projet et de respecter les bonnes pratiques du dépôt.
