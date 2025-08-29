# Unit3d Setup Guide


## Windows Users: Enable WSL Integration

To install WSL with Ubuntu 24.04, open PowerShell as Administrator and run:

```powershell
wsl --install -d Ubuntu-24.04
```

After installation, restart your computer if prompted. Then, launch Ubuntu from the Start menu to complete setup.

If you are using Windows, ensure that WSL integration is enabled in Docker Desktop for containers to work properly:

1. Open Docker Desktop.
2. Go to **Settings** > **Resources** > **WSL Integration**.
3. Enable integration for your desired WSL distributions (e.g., Ubuntu).

### Enable Mirrored Networking Mode in WSL

To improve network compatibility for Docker containers on Windows, enable "mirrored" networking mode in WSL:

1. Open Docker Desktop.
2. Go to **Settings** > **WSL** > **Networking**.
3. Set **Network Mode** to **Mirrored**.

This ensures containers have better access to network resources and services.

This allows Docker containers to run seamlessly with WSL on Windows.

### 1. Confirm MySQL Host and Port

- If using Laravel Sail, set `DB_HOST` to `mysql` (the service name in `docker-compose.yml`), not `127.0.0.1`.
- Example for Sail:

    ```env
    DB_HOST=mysql
    ```

- Ensure your `.env` file has the correct Redis host configuration for Sail:

    ```env
    REDIS_HOST=redis
    ```



- If you encounter "Connection refused", verify the Redis container is running:

    ```bash
    ./vendor/bin/sail ps
    ./vendor/bin/sail up -d redis
    ```






## 1. Install Docker and Docker Compose on Linux
### Ubuntu/Debian
```bash
sudo apt update
sudo apt install -y docker.io docker-compose-v2
```


### Arch Linux
```bash
sudo pacman -Syu
sudo pacman -S docker docker-compose
```

# Enable and Start Docker

After installing Docker, enable and start the Docker service:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

## 2. Install PHP 8.4

### Ubuntu

```bash
# Add the ondrej/php repository.
sudo apt update
sudo apt install -y software-properties-common
sudo LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php -y
sudo apt update

# Install PHP.
sudo apt-get install -y php8.4 php8.4-curl php8.4-zip php8.4-xml php8.4-bcmath php8.4-intl php8.4-mysql php8.4-redis php8.4-mbstring mariadb-client-core unzip
```
### Debian

```bash
# Add the packages.sury.org/php repository.
sudo apt-get update
sudo apt-get install -y lsb-release ca-certificates apt-transport-https curl
sudo curl -sSLo /tmp/debsuryorg-archive-keyring.deb https://packages.sury.org/debsuryorg-archive-keyring.deb
sudo dpkg -i /tmp/debsuryorg-archive-keyring.deb
sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/debsuryorg-archive-keyring.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
sudo apt-get update

# Install PHP.
sudo apt-get install -y php8.4 php8.4-curl php8.4-zip php8.4-xml php8.4-bcmath php8.4-intl php8.4-mysql php8.4-redis php8.4-mbstring mariadb-client-core unzip
```

### Arch Linux

```bash
# Update system and install PHP with required extensions and Composer.
sudo pacman -Syu
sudo pacman -S php php-fpm php-gd php-intl php-pgsql php-sqlite php-redis php-imagick composer
```

## 3. Clone the UNIT3D Repository

```bash
git clone https://github.com/HDInnovations/UNIT3D.git
cd UNIT3D
```


## 4. Install Composer

### Ubuntu/Debian

```bash 
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"
php composer-setup.php
php -r "unlink('composer-setup.php');"

sudo mv composer.phar /usr/local/bin/composer

```

> **Note:** On Arch Linux, Composer is installed via `pacman` in the previous step and does not require manual installation.

## 5. Enable PHP Extensions

To enable required PHP extensions on Ubuntu, run:

```bash
sudo sed -i '/^;zend_extension=opcache/s/^;//' /etc/php/8.4/cli/php.ini \
    && sudo sed -i '/^;extension=iconv/s/^;//' /etc/php/8.4/cli/php.ini \
    && sudo sed -i '/^;extension=bcmath/s/^;//' /etc/php/8.4/cli/php.ini \
    && sudo sed -i '/^;extension=redis/s/^;//' /etc/php/8.4/cli/php.ini \
    && sudo sed -i '/^;extension=intl/s/^;//' /etc/php/8.4/cli/php.ini \
    && sudo sed -i '/^;extension=mysqli/s/^;//' /etc/php/8.4/cli/php.ini \
    && sudo sed -i '/^;extension=pdo_mysql/s/^;//' /etc/php/8.4/cli/php.ini
```

To enable required PHP extensions on Arch Linux, run:

```bash
sudo sed -i '/^;zend_extension=opcache/s/^;//' /etc/php/php.ini \
    && sudo sed -i '/^;extension=iconv/s/^;//' /etc/php/php.ini \
    && sudo sed -i '/^;extension=bcmath/s/^;//' /etc/php/php.ini \
    && sudo sed -i '/^;extension=redis/s/^;//' /etc/php/php.ini \
    && sudo sed -i '/^;extension=intl/s/^;//' /etc/php/php.ini \
    && sudo sed -i '/^;extension=mysqli/s/^;//' /etc/php/php.ini \
    && sudo sed -i '/^;extension=pdo_mysql/s/^;//' /etc/php/php.ini
```

> **Note:** Adjust the path to `php.ini` if your PHP installation uses a different location


## 6. Copy the Environment File

After cloning the repository, copy the example environment file:

```bash
cp .env.example .env
```

## 7. Install Dependencies with Composer

Run the following commands inside the `UNIT3D` directory to install and update PHP dependencies:

```bash
composer update
composer install
```

## 8. Start UNIT3D with Laravel Sail

Before starting UNIT3D, add your user to the Docker group to run Docker commands without `sudo`:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

or Log out and back in for the group change to take effect.

9. To start the application using Laravel Sail, run:

```bash
./vendor/bin/sail up -d
```

> **Tip:** The MySQL container may need to be restarted multiple times during initial setup. If you encounter connection issues, run:
>
> ```bash
> ./vendor/bin/sail restart mysql
> ```
>
> Wait a few moments after each restart before retrying your commands.

This will start the required Docker containers in detached mode.


## 10. Generate Application Key

Before generating the Laravel application key, ensure your `.env` file contains an `APP_KEY` variable (it can be empty):

```bash
./vendor/bin/sail artisan key:generate
```

This command sets the `APP_KEY` value in your `.env` file, which is required for application security.


## 11. Run Database Migrations and Seed Data

To set up the database schema and seed initial data, run:

```bash
./vendor/bin/sail artisan migrate:fresh --seed
```

## 12. Manage Node.js Dependencies and Compile Assets

To install Node.js dependencies and build frontend assets within the Docker environment, run:

```bash
./vendor/bin/sail bun install
./vendor/bin/sail bun pm untrusted
./vendor/bin/sail bun pm trust --all
./vendor/bin/sail bun install
./vendor/bin/sail bun run build
```

If you need to refresh the Node.js environment (e.g., after updating dependencies), use:

```bash
./vendor/bin/sail rm -rf node_modules && bun pm cache rm && bun install && bun run build
```

## 13. Application Cache Configuration

Optimize performance by setting up the cache:

```bash
./vendor/bin/sail artisan set:all_cache
```

## 14. Environment Restart

Apply new configurations or restart the environment:

```bash
./vendor/bin/sail restart && ./vendor/bin/sail artisan queue:restart
```

---

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

- **Fix MySQL root user error:**
    If you encounter `ERROR 1396 (HY000) ... CREATE USER failed for 'root'@'%'`, run the following inside the MySQL CLI:
    To execute MySQL commands directly inside the running MySQL container, use:

    ```bash
    ./vendor/bin/sail exec mysql mysql -u root -p
    ```

    This opens a MySQL shell as the root user. Enter your password when prompted.
    ```sql
    ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'your_root_password';
    FLUSH PRIVILEGES;
    ```
    Replace `'your_root_password'` with your desired password.

- **Fix time zone warnings:**
    To resolve time zone warnings, install the time zone info in your MySQL container:
    ```bash
    ./vendor/bin/sail exec mysql bash -c "apt-get update && apt-get install -y tzdata && mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root -pYOUR_PASSWORD mysql"
    ```
    Replace `YOUR_PASSWORD` with your MySQL root password.





### Queue Management

- **Restart queue workers:**
    ```bash
    ./vendor/bin/sail artisan queue:restart
    ```
    Restarts queue workers.

### Troubleshooting

- **View logs:**
    ```bash
    ./vendor/bin/sail logs
    ```
    Displays Docker container logs.

- **Run PHPUnit (PEST) tests:**
    ```bash
    ./vendor/bin/sail artisan test
    ```
    Runs PEST tests for the application.


    