# php-fpm7.2_postfix_supervisord
Строка для запуска :
docker build -t php-fpm .
docker run --name php7.2 -d -p 9201:9201  -v /home/bitrix/:/home/bitrix -v /var/docker-php/:/usr/local/etc/php-fpm.d/ -e TIMEZONE=Europe/Kiev php-fpm
