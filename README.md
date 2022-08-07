# for m1 macs running php-fpm images, as iUS/Remi repos do not have RPMs 

docker-compose.yml
```yaml
  php8:
    platform: linux/arm64/v8
    image: madebymode/php8-arm64-alpine
    build: github.com/madebymode/docker-arm64-php80.git
    ports:
      - "9000:9000"
    volumes:
      # real time sync for app php files
      - .:/app
      # cache laravel libraries dir
      - ./vendor:/app/vendor:cached
      # logs and sessions should be authorative inside docker
      - ./storage:/app/storage:delegated
      # cache static assets bc fpm doesn't need to update css or js
      - ./public:/app/public:cached
      # additional php config REQUIRED
      - ./docker-conf/php-ini:/usr/local/etc/php/custom.d
    env_file:
      - .env
    environment:
      # note that apline has dif dir structures: /user/local/etc - conf.d need to be scanned here for all modules from image
      - PHP_INI_SCAN_DIR=/usr/local/etc/php/custom.d:/usr/local/etc/php/conf.d/
      - COMPOSER_AUTH=${COMPOSER_AUTH}
```
