FROM php:8.2-apache

# PHP extensions required by the panel
RUN docker-php-ext-install pdo pdo_mysql mysqli \
    && a2enmod rewrite headers

# App code lives at /var/www/html (Apache default docroot)
COPY . /var/www/html/

# storage/ and uploads/ads/ are empty (untracked by git), so they are NOT in the
# build context. Create them explicitly, then keep pristine copies (if any)
# so entrypoint.sh can seed the persistent volume on first boot.
RUN mkdir -p /var/www/html/storage /var/www/html/uploads/ads \
             /opt/defaults/storage /opt/defaults/uploads-ads \
    && cp -a /var/www/html/storage/. /opt/defaults/storage/ \
    && cp -a /var/www/html/uploads/ads/. /opt/defaults/uploads-ads/ \
    && chown -R www-data:www-data /var/www/html

COPY railway-deploy/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

EXPOSE 8080
ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]
