# laravel-sqlserver
Use ODBC to connect with your SQL Server instances from Laravel 5+.

This package allows you to use the OFFICIAL Microsoft SQL Server ODBC driver to 
connect to a Microsoft SQL Server database.

Disclaimer: all Bash commands you will see here are examples for RHEL/CentOS Yum package manager. 
If you would like to help make these docs more friendly with other distros, please make sure to either 
open an issue with additional commands specifying which distro you are using. I'll then update the README
docs.

## Dependencies
### UnixODBC

```bash
sudo yum install -y unixodbc
```

### PHP's ODBC PDO driver

```bash
sudo yum install -y php-odbc
```

### Microsofts SQL Server Driver
You can cand specific instructions for your distro here: https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server

## Package Installation

Install with composer
```bash
composer require techscope/laravel-sqlserver:dev-master
```

Add the Service Provider to config/app.php
```php
TechScope\SqlServer\SqlServerServiceProvider::class
```

Make sure to update your config/database.php file

```php
'domdb' => [
            'driver'        => 'sqlsrv',
            'odbc_driver'   => '{ODBC Driver 13 for SQL Server}',
            'host'          => env('DB_HOST', 'localhost'),
            'database'      => env('DB_DATABASE', 'forge'),
            'username'      => env('DB_USERNAME', 'forge'),
            'password'      => env('DB_PASSWORD', ''),
            'port'          => env('DB_PORT', '1433'),
            'TrustServerCertificate' => 'yes'
        ],
```

**IMPORTANT NOTES:** 
- `driver` should be set to `sqlsrv`. This uses the preexisting SQL Server grammar that ships with Laravel.
- `odbc_driver` should be the name of the ODBC Driver as it appears in `/etc/odbcinst.ini`. Example:
```vim
[ODBC Driver 13 for SQL Server] <--- USE THIS LINE OR SIMILAR DEPENDENDING ON WHICH VERSION YOU INSTALL
Description=Microsoft ODBC Driver 13 for SQL Server
Driver=/opt/microsoft/msodbcsql/lib64/libmsodbcsql-13.1.so.8.0
UsageCount=1
```
- For the ODBC Driver **MAKE SURE TO REPLACE THE SQUARE BRACKETS ([]) WITH CURLY BRACES ({}) IN** `config/database.php`
- You must use the `host`, `database`, `username`, `password`, `port` properties to properly setup the DSN string for the ODBC Connection
- any additional parameters you'd like to use for the connection can be found here: https://docs.microsoft.com/en-us/sql/relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client
    - **NOTE:** the `Database`, `Server`, `UID`, `PWD`, `Network`, `Net`, `DSN` and `Database` properties cannot be used here as they are already specified the Laravel way 
