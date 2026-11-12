# PHP-Version wie auf dem SOM-PC (XAMPP: PHP 7.4.33)
FROM php:7.4-apache
# Debian 11 (bullseye) ist ins Archiv gewandert
RUN printf "deb http://archive.debian.org/debian bullseye main\ndeb http://archive.debian.org/debian-security bullseye-security main\n" > /etc/apt/sources.list \
 && apt-get -o Acquire::Check-Valid-Until=false update && apt-get install -y --no-install-recommends libpng-dev libjpeg-dev libfreetype6-dev libzip-dev libbz2-dev libssl-dev msmtp \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install mysqli pdo_mysql gd exif gettext bz2 zip ftp \
 && rm -rf /var/lib/apt/lists/*
# Mails gehen nur an Mailpit im abgeschotteten Netz
RUN printf 'account default\nhost mailpit\nport 1025\nfrom som-spiegel@localhost\n' > /etc/msmtprc \
 && printf 'sendmail_path = "/usr/bin/msmtp -t"\ndate.timezone = Europe/Berlin\ndisplay_errors = On\nerror_reporting = E_ALL & ~E_NOTICE & ~E_DEPRECATED\nmemory_limit = 512M\nupload_max_filesize = 200M\npost_max_size = 200M\n' > /usr/local/etc/php/conf.d/som.ini
