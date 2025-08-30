# Installing Podman and Podman-Compose on Fedora

## 1. Update your system
```sh
sudo dnf update
```

## 2. Install Docker
```sh
sudo dnf install -y docker
```

## 3. Install Docker-Compose
```sh
sudo dnf install -y docker-compose
```

## 4. Verify Installation
```sh
docker --version
docker-compose --version
```


## 5. Add Remi's RPM Repository

```sh
sudo dnf install -y dnf-plugins-core
sudo dnf install -y https://rpms.remirepo.net/fedora/remi-release-$(rpm -E %fedora).rpm
sudo dnf module reset php -y
sudo dnf module enable php:remi-8.4 -y
```

## 6. Install PHP

```sh
sudo dnf install -y php
```


## 7. Install Additional PHP Extensions

```sh
sudo dnf install -y php-cli php-fpm php-json php-common php-mbstring php-xml php-gd php-curl php-zip php-mysqlnd
```

## 8. Start and Enable PHP-FPM

```sh
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

## 9. Check PHP Version

```sh
php -v
```

## 10. Install Additional Packages

```sh
sudo dnf install -y php-bcmath php-intl php-redis mariadb
sudo dnf install -y unzip composer
```

## 11. Enable and Start Docker Service

```sh
sudo systemctl enable docker
sudo systemctl start docker
```

## 12. Add Your User to the Docker Group

```sh
sudo usermod -aG docker $USER
newgrp docker
```