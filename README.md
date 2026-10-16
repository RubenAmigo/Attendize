*Attendize* is an open-source ticketing and event management application built using the Laravel PHP framework. Attendize allows event organisers to sell tickets to their events and manage attendees without paying service fees to third party ticketing companies.

<p align="center">
  <img src="/public/assets/images/logo-dark.png" alt="Attendize"/>
  <img style='border: 1px solid #444;' src="https://www.attendize.com/images/screenshots/screen1.PNG" alt="Attendize"/>
</p>

# Attendize
Open-source ticket selling and event management platform

> Please report bugs here: https://github.com/Attendize/Attendize/issues. Detailed bug reports are more likely to be looked at. Simple creating an issue and saying "it doesn't work" is not useful. Providing some steps to reproduce your problem as well as details about your operating system, PHP version etc can help.

> Take a look https://www.attendize.com/troubleshooting.html and follow the https://www.attendize.com/getting_started.html guide to make sure you have configured attendize correctly.  

Documentation Website: https://www.attendize.com<br />

## Current Features (v2.X.X)
 - Beautiful mobile friendly event pages
 - Easy attendee management - Refunds, Messaging etc.
 - Data export - attendees list to XLS, CSV etc.
 - Generate print friendly attendee list
 - Ability to manage unlimited organisers / events
 - Manage multiple organisers 
 - Real-time event statistics
 - Customizable event pages
 - Multiple currency support
 - Quick and easy checkout process
 - Customizable tickets - with QR codes, organiser logos etc.
 - Fully brandable - Have your own logos on tickets etc.
 - Affiliate tracking
    - track sales volume / number of visits generated etc.
 - Widget support - embed ticket selling widget into existing websites / WordPress blogs
 - Social sharing 
 - Support multiple payment gateways - Stripe, PayPal & Coinbase so far, with more being added
 - Support for offline payments
 - Refund payments - partial refund & full refunds
 - Ability to add service charge to tickets
 - Messaging - eg. Email all attendees with X ticket
 - Public event listings page for organisers
 - Ability to ask custom questions during checkout
 - Browser based QR code scanner for door management
 - Elegant dashboard for easy management.

## Minimum Requirements

Attendize should run on most pre-configured LAMP or LEMP environments as long as certain requirements are adhered to. Attendize is based on the [Laravel Framework](https://laravel.com/)

**PHP Requirements**
1. PHP >= 7.1.3
2. OpenSSL PHP Extension 
3. PDO PHP Extension 
4. Mbstring PHP Extension 
5. Tokenizer PHP Extension 
6. Fileinfo PHP Extension 
7. GD PHP Extension

**MySQL Requirements**
1. MySQL version 5.7 or higher required

## Contributing
Feel free to fork and contribute. If you are unsure about adding a feature, create a Github issue to ask for Feedback. Read the [contribution guidelines](CONTRIBUTING.md)

## Submitting an issue
If you encounter a bug in Attendize, please first search the list of current open [Issues on the GitHub repository](https://github.com/Attendize/Attendize/issues). You may add additional feedback on an existing bug report. If the issue you're having has not yet been reported, please open a new issue. There is a template available for new issues. Please fill out all information requested in the template so we can help you more easily.

Please note: support is not offered from the project maintainers through GitHub. Paid support is available by [purchasing a license](https://www.attendize.com/license.html).

## Installation
To get developing straight away use the [Pre-configured Docker Environment](https://www.attendize.com/getting_started.html#running-attendize-in-docker-for-development)<br />
To do a manual installation use the [Manual Installation Steps](https://www.attendize.com/getting_started.html#manual-installation)

## Upgrade
To upgrade to a new version:

- we recommend putting your site into [Maintenance Mode](https://laravel.com/docs/6.x/configuration#maintenance-mode) by running `php artisan down` from the root of your installation. This isn't required but can help prevent problems if users are on your site during the upgrade.
- download the latest version of the code from GitHub and copy the code to your server however you normally do so - via FTP, git pull, continuous integration deployment, etc.
- navigate to https://<your-site.com>/upgrade and follow the instructions on screen
- the upgrade page will run database migrations, update configuration settings, etc as needed for the new code version.

## Testing
To run the application tests, you can run the following from your project root:

```sh
# If the testing db does not exist yet, please create it
touch database/database.sqlite
# Run the test suite
./vendor/bin/phpunit
```

This will run the feature tests that hits the database using the `sqlite` database connection.

## Troubleshooting
If you are having problems please read the [troubleshooting guide](https://www.attendize.com/troubleshooting.html) 

## License
Attendize is open-sourced software licensed under the Attribution Assurance License. See [https://www.attendize.com/license.html](https://www.attendize.com/license.html) for further details. We also have white-label license options available.

## Code of Conduct
The Attendize community operates a [Code of Conduct](CODE_OF_CONDUCT.md) to ensure everyone is able to participate comfortably, equally and safely.



# Tarea 01: Despliegue local reproducible de Attendize

Proyecto Intermodular II · 2.º DAW · IES Juan de Garay

## Punto de partida

- Repositorio: fork de [Attendize/Attendize](https://github.com/Attendize/Attendize)
- Versión base: etiqueta `v2.8.0` (sin cambiar de versión ni usar soluciones externas)
- Rama de trabajo: `tarea-01-despliegue`
- Etiqueta de inicio: `tarea-01-inicio`
- Etiqueta de entrega: `tarea-01-entrega`

## Requisitos

- Git
- Docker Desktop (con `docker compose`)
- Terminal (Git Bash en Windows)

## Pasos realizados

1. Fork del repositorio oficial y clonado local, con `upstream` apuntando al repositorio original.
2. Creación de la rama `tarea-01-despliegue` desde `v2.8.0` y de la etiqueta `tarea-01-inicio`.
3. Copia de `.env.example` a `.env` y revisión de las variables de aplicación, base de datos y correo.
   `DB_HOST` es `db`, el nombre del servicio de base de datos en `docker-compose.yml`.
   El archivo `.env` no se sube al repositorio.
4. Construcción de las imágenes, con las órdenes declaradas en el Makefile:

   ```
   docker build --tag attendize_base --target base .
   docker build --tag attendize_worker --target worker --cache-from attendize_base:latest .
   docker build --tag attendize_web --target web --cache-from attendize_worker:latest .
   ```

5. Inicio de servicios, preparación de Laravel y MySQL, instalación y comprobación de persistencia
   (ver sección "Estado de los pasos posteriores").

## Incidencias y adaptaciones

Al ejecutar `docker build --target base` aparecieron tres problemas encadenados.
Se resolvieron con cambios mínimos en el `Dockerfile`, sin cambiar de versión de la aplicación
ni de la imagen base. El registro del primer error se conserva en `error-build-base.log`.

### Incidencia 1: los repositorios de Debian devuelven 404

**Error**

```
RUN apt-get update && apt-get install -y wait-for-it libxrender1
...
E: The repository 'http://deb.debian.org/debian buster Release' does not have a Release file.
ERROR: ... did not complete successfully: exit code: 100
```

**Causa**

La imagen base `wyveo/nginx-php-fpm:php74` está construida sobre Debian 10 "buster",
que ha llegado al final de su vida útil (EOL). Sus repositorios ya no están en
`deb.debian.org` ni en `security.debian.org` (responden 404) y se encuentran en
`archive.debian.org`. Al fallar `apt-get update`, `apt-get install` no puede continuar.
El repositorio de PHP (`packages.sury.org`) también devolvía 404.

**Adaptación**

Se apuntan las fuentes de apt a `archive.debian.org`, se elimina la línea `buster-updates`
(que no existe en el archivo) y se desactiva la comprobación de caducidad de los índices
antiguos con `Acquire::Check-Valid-Until=false`.

### Incidencia 2: clave de firma caducada del repositorio de nginx

**Error**

```
W: GPG error: http://nginx.org/packages/mainline/debian buster InRelease:
The following signatures were invalid: EXPKEYSIG ABF5BD827BD9BF62 nginx signing key
E: The repository 'http://nginx.org/packages/mainline/debian buster InRelease' is not signed.
```

**Causa**

La clave con la que está firmado el repositorio de nginx incluido en la imagen ha caducado,
por lo que apt lo rechaza como no firmado y `apt-get update` termina con error.
Se localizó la línea con:

```
docker run --rm wyveo/nginx-php-fpm:php74 sh -c "grep -rn nginx.org /etc/apt/"
```

Resultado: `/etc/apt/sources.list:7:deb http://nginx.org/packages/mainline/debian/ buster nginx`.

**Adaptación**

Se elimina esa línea con `sed`. Para instalar `wait-for-it` y `libxrender1` no hace falta
el repositorio de nginx, y nginx ya viene instalado en la imagen base.

**Cambio resultante en el `Dockerfile`** (incidencias 1 y 2):

```dockerfile
# Antes
RUN apt-get update && apt-get install -y wait-for-it libxrender1

# Después
RUN sed -i 's|deb.debian.org|archive.debian.org|g; s|security.debian.org|archive.debian.org|g; /buster-updates/d; /nginx.org/d' /etc/apt/sources.list \
    && rm -f /etc/apt/sources.list.d/*.list \
    && apt-get -o Acquire::Check-Valid-Until=false update \
    && apt-get install -y wait-for-it libxrender1
```

### Incidencia 3: `./scripts/setup: not found` (exit code 127)

**Error**

```
RUN ./scripts/setup
/bin/sh: 1: ./scripts/setup: not found
ERROR: ... did not complete successfully: exit code: 127
```

**Causa**

El script existe, pero tiene finales de línea de Windows (CRLF). Esto se debe a que Git for
Windows convierte los saltos de línea al clonar (`core.autocrlf=true`). La primera línea
queda como `#!/bin/bash\r` y Linux busca un intérprete llamado `/bin/bash\r`, que no existe.

Verificado con:

```
$ file scripts/setup
scripts/setup: Bourne-Again shell script, ASCII text executable, with CRLF line terminators
```

**Adaptación**

Se normaliza el script a LF dentro del propio `Dockerfile` antes de ejecutarlo. Así el
build funciona en cualquier máquina, con independencia de la configuración de Git.

```dockerfile
# Antes
RUN ./scripts/setup

# Después
RUN sed -i 's/\r$//' ./scripts/setup && chmod +x ./scripts/setup && ./scripts/setup
```

### Incidencia 4: falta `vendor/autoload.php` al ejecutar `php artisan`

**Error**

```
PHP Fatal error: require(): Failed opening required '/usr/share/nginx/html/vendor/autoload.php'
sh: 2: artisan: not found
```

**Causa**

`docker-compose.yml` monta el directorio del proyecto sobre el del contenedor
(`.:/usr/share/nginx/html`). Ese montaje oculta el `vendor/` que se instaló en la imagen
durante el build, y en el equipo local no existe `vendor/` (verificado con `ls vendor`).

**Adaptación**

Instalar las dependencias desde el contenedor, de modo que `vendor/` quede en el
directorio compartido:

```
docker compose exec web composer install
```

`vendor/` no se sube a Git.

### Incidencia 5: `APP_KEY` con varias claves y el instalador pide la clave en bucle

**Error**

El instalador volvía a pedir generar la clave de Laravel. En `.env`, la línea `APP_KEY`
contenía cinco claves `base64:...` concatenadas, y `printenv APP_KEY` dentro del contenedor
no devolvía nada.

**Causa probable**

El contenedor se creó con `APP_KEY` vacía y no relee el `.env` hasta que se recrea. Cada
generación de clave añadía una nueva en lugar de reemplazar la anterior, con lo que la
clave resultante no era válida.

**Adaptación**

Se generó una clave con `echo "base64:$(openssl rand -base64 32)"`, se dejó una única
clave en `.env` y se recrearon los contenedores y la caché de configuración:

```
docker compose up -d --force-recreate
docker compose exec web php artisan config:clear
```

### Nota: aviso al importar la caché

Al construir `worker` y `web` aparece `ERROR importing cache manifest from attendize_base:latest`.
Es un aviso no fatal: `--cache-from` busca esa referencia como imagen de un registro remoto y,
al no encontrarla, Docker continúa sin esa caché. El build termina correctamente y no
requirió ninguna modificación.

## Resultado

- [x] Imágenes `attendize_base`, `attendize_worker` y `attendize_web` construidas.
- [x] Servicios iniciados con `docker compose up -d`.
- [x] Migraciones de Laravel ejecutadas.
- [x] Instalación completada en `https://localhost:8081/install` con datos ficticios.
- [x] Persistencia comprobada: tras `docker compose stop` y `docker compose start`, el organizador
      y el evento de prueba siguen disponibles.

## Seguridad

- `.env` está ignorado por Git y no se sube al repositorio.
- `.env.example` solo contiene nombres de variables y valores de ejemplo, sin contraseñas reales.
- Todos los datos de la instalación son ficticios y no se han configurado pagos ni servicios externos.