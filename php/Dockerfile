FROM php:7.4-fpm

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('sha384', 'composer-setup.php') === '795f976fe0ebd8b75f26a6dd68f78fd3453ce79f32ecb33e7fd087d39bfeb978342fb73ac986cd4f54edd0dc902601dc') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php \
&& php -r "unlink('composer-setup.php');" \
&& mv composer.phar /usr/local/bin/composer \
&& apt-get update && apt-get install -y \
git         \
gnupg2      \
libnss3     \
libzip-dev  \
unzip       \
wget        \
&& docker-php-ext-install \
pdo_mysql \
zip 

# gnupg2        to install chrome
# libnss3       to execute dusk (curl error)
# libzip-dev    to install zip
# unzip         to create laravel project
# wget          to install chrome
# pdo_mysql     to use MySQL on PHP
# zip           to install dusk

# この3行はchromeを別コンテナで立ち上げない場合に必要（Chromeをインストールしている）
# && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
# && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \
# && apt-get update && apt-get install -y google-chrome-stable

# laravelのプロジェクトを作成しても意味がない。ボリュームがマウントされて消える。
# && composer create-project --prefer-dist laravel/laravel new-app

WORKDIR /var/www/html