# Exploring Laravel Sail on Docker Desktop for Windows without WSL 2
An experimental guide on running (part of) Laravel Sail on Docker Desktop for Windows without WSL 2

(under construction)

**Spoiler alert:** you will set up Laravel Sail's Docker containers, but will not be able to use the `sail` command as `sail` is implemented as a bash script. In stead this guide will set up an interactive session with one of the containers, where you can use the original commands like `php artisan` and `composer` seamlessly.

## Quick reference

1. Open a Command Prompt and navigate to the directory (e.g. `cd my-projects`) where you want to create a subdirectory where a new Laravel project is to be installed. Assuming the name of your project directory is `strawberry`, enter the following command:

```
docker run --rm -v %cd%:/opt -w /opt laravelsail/php80-composer:latest bash -c "laravel new strawberry && cd strawberry && php ./artisan sail:install"
```

2. Before you ever think of building and starting the multi-container environment described in `strawberry/docker-compose.yml` you need to review the `strawberry/.env` file first, because `strawberry/docker-compose.yml` contains a set of environment variables (notation: `${}`), which will be looked up in `strawberry/.env` !
  * The .env variables **DB_DATABASE**, **DB_USERNAME** and **DB_PASSWORD** will be used during MySQL DB creation (database name and credentials)
  * Don't leave **DB_PASSWORD** empty !!
  * Add **WWWGROUP=www-data**
  * If some other application is already listening on TCP/IP port 80, add **APP_PORT=8080**
  * If some other application is already listening on TCP/IP port 3306, add **FORWARD_DB_PORT=3307**

3. Start the the multi-container environment by

```
cd strawberry
docker-compose up
```

4. In a separate command prompt, start an interactive session in the laravel.test container by

```
docker-compose exec laravel.test bash
```
Here you can use the original commands like `php artisan` and `composer` seamlessly
