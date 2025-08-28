# Unit3d Setup Guide

## 1. Install Docker and Docker Compose on Linux
### Ubuntu/Debian
sudo apt update
sudo apt install -y docker.io docker-compose



### Arch Linux
sudo pacman -Syu
sudo pacman -S docker docker-compose

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
sudo apt-get install -y php8.4 php8.4-curl php8.4-zip php8.4-xml php8.4-bcmath php8.4-intl php8.4-mysql php8.4-mbstring mariadb-client-core
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
sudo apt-get install -y php8.4 php8.4-curl php8.4-zip php8.4-xml php8.4-bcmath php8.4-intl php8.4-mysql php8.4-redis php8.4-mbstring mariadb-client-core
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

## 8. Generate Application Key

Run the following command inside the `UNIT3D` directory to initialize the application key:

```bash
php artisan key:generate
```

## 9. Start UNIT3D with Laravel Sail

Run the following command inside the `UNIT3D` directory to start the application in detached mode:

```bash
./vendor/bin/sail up -d
```

## 10. Run Database Migrations and Seed Data

After starting UNIT3D, run the following command inside the `UNIT3D` directory to reset and seed the database:

```bash
php artisan migrate:fresh --seed
```

## 11. Set Permissions for `node_modules`

If you need to create the `node_modules` directory and set permissions, run:

```bash
mkdir node_modules
chmod 777 -R node_modules
```

> **Warning:** Setting permissions to `777` allows all users to read, write, and execute files in `node_modules`. Use with caution and only in development environments.

## 12. Install and Build Frontend Assets with Bun

To install frontend dependencies and build assets using Bun, run the following commands inside the `UNIT3D` directory:

```bash
sudo ./vendor/bin/sail bun install
sudo ./vendor/bin/sail bun run build
```

## 13. Create the `public/build/assets` Directory

To manually create the `public/build/assets` directory, run:

```bash
sudo mkdir -p public/build/assets
```

## 14. Set Permissions for the `public` Directory

To set permissions for the `public` directory, run:

```bash
sudo chmod 777 -R public/
```

## 15. Build Frontend Assets

After setting permissions, build the frontend assets:

```bash
sudo ./vendor/bin/sail bun run build
```

## 16. Configure Application Cache

Optimize the application's performance by setting up the cache:

```bash
php artisan set:all_cache
```

## 17. Create the `storage/logs` Directory

To manually create the `storage/logs` directory, run:

```bash
mkdir -p storage/logs
```

## 18. Restart Environment

Apply new configurations or restart the environment by toggling the Docker environment:

```bash
./vendor/bin/sail restart
php artisan queue:restart
```

You need to create the required self-signed SSL certificate and key pair and ensure they are placed in the correct location so the Nginx container can access them.

Stop the running containers if they are not already stopped.

Bash

./vendor/bin/sail stop
Generate a self-signed SSL certificate and private key. You can use the OpenSSL command for this. Run this command from your project's root directory.

Bash

mkdir -p ./certs
openssl req -x509 -newkey rsa:2048 -nodes -keyout ./certs/server.key -out ./certs/server.pem -days 365 -subj "/CN=localhost"
mkdir -p ./certs: Creates a new directory named certs in your project root to store the certificate files.

openssl req ...: This is the command to generate the certificate.

-x509: Creates a self-signed certificate instead of a certificate signing request.

-newkey rsa:2048: Generates a new RSA private key with a size of 2048 bits.

-nodes: Skips encrypting the private key with a passphrase.

-keyout ./certs/server.key: Specifies the output path for the private key.

-out ./certs/server.pem: Specifies the output path for the certificate.

-days 365: Sets the certificate's validity period to one year.

-subj "/CN=localhost": Sets the common name for the certificate to localhost.

Update the docker-compose.yml file. You must now instruct Docker to mount the newly created certs directory from your host machine into the Nginx container. Find the nginx service section in your docker-compose.yml file and add a volume mapping.

Find this line:

YAML

volumes:
    - 'sail-nginx:/etc/nginx/certs'
Add a new mapping for your generated files:

YAML

volumes:
    - './certs:/etc/nginx/certs'
    - 'sail-nginx:/etc/nginx/certs' # You might need to check if this line is still needed or can be removed
The sail-nginx volume is what Laravel Sail uses to manage certificates, but manually mounting your local certs directory is the most direct way to solve this specific error.

Restart your services using sail up to apply the changes.

Bash

./vendor/bin/sail up -d
After these steps, the Nginx container will be able to find and load the certificate and private key, allowing it to start successfully.
