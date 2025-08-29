
- **Rerun Composer after updating `.env`:**
    After making changes to your `.env` file, run the following commands to update dependencies and regenerate autoload files:
    ```bash
    ./vendor/bin/sail composer update
    ./vendor/bin/sail composer install
    ```
    This ensures your application uses the latest configuration and dependencies.

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


    