## Additional Notes

- **Permissions:** Use `sudo` cautiously to avoid permission conflicts, especially with Docker commands requiring elevated access.

---

## Appendix: Sail Commands for UNIT3D

### Docker Management

- **Start environment:**
    ```bash
    ./vendor/bin/sail up -d
    ```
    Starts Docker containers in detached mode.

- **Stop environment:**
    ```bash
    ./vendor/bin/sail down
    ```
    Stops and removes Docker containers.

- **Restart environment:**
    ```bash
    ./vendor/bin/sail restart
    ```
    Applies changes by restarting the Docker environment.

### Dependency Management

- **Install Composer dependencies:**
    ```bash
    ./vendor/bin/sail composer install
    ```
    Installs PHP dependencies.

- **Update Composer dependencies:**
    ```bash
    ./vendor/bin/sail composer update
    ```
    Updates PHP dependencies.

### Laravel Artisan

- **Run migrations:**
    ```bash
    ./vendor/bin/sail artisan migrate
    ```
    Executes database migrations.

- **Seed database:**
    ```bash
    ./vendor/bin/sail artisan db:seed
    ```
    Seeds the database.

- **Refresh database:**
    ```bash
    ./vendor/bin/sail artisan migrate:fresh --seed
    ```
    Resets and seeds the database.

- **Cache configurations:**
    ```bash
    ./vendor/bin/sail artisan set:all_cache
    ```
    Clears and caches configurations.

### NPM and Assets

- **Install Node.js dependencies:**
    ```bash
    ./vendor/bin/sail bun install
    ```
    Installs Node.js dependencies.

- **Compile assets:**
    ```bash
    ./vendor/bin/sail bun run build
    ```
    Compiles CSS and JavaScript assets.

### Database Operations

- **MySQL interaction:**
    ```bash
    ./vendor/bin/sail mysql -u root -p
    ```
    Opens MySQL CLI.
