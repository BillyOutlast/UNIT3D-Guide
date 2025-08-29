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
### Ubuntu
```bash
sudo apt update
sudo apt install -y docker.io docker-compose-v2
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


## 3. Clone the UNIT3D Repository

```bash
git clone https://github.com/HDInnovations/UNIT3D.git
cd UNIT3D
```


## 4. Install Composer

### Ubuntu

```bash 
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"
php composer-setup.php
php -r "unlink('composer-setup.php');"

sudo mv composer.phar /usr/local/bin/composer

```

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


For troubleshooting MySQL root user errors, see [Fix MySQL root user error](./Troubleshooting.md#fix-mysql-root-user-error).


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
