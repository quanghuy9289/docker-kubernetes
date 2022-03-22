# Example Docker Network

Tạo ra 3 container, chạy 3 dịch vụ khác nhau là `httpd, php-fpm, mysql`.

Tạo một mạng `bridge` để liên kết những container này với nhau (giao tiếp với nhau qua cổng nội bộ httpd:80, php-fpm:9000, mysql:443).

Trong đó thiết lập thêm httpd mở cổng public 8080 để ánh xạ vào 80 của nó.

## Tạo container

### PHP (php-fpm) container

- Tạo một container chạy PHP từ image `php:7.3-fpm`, đặt tên container này là `c-php`, ánh xạ folder `/home/phpcode` trong container với `/mycode/php` trên máy host, đặt tên HOSTNAME của container là php (`-h php`)

> docker run -d --name c-php -h php -v /mycode/php:/home/phpcode php:7.3-fpm

- Nhảy vào container để tạo file php

> docker exec -it c-php bash

- Kiểm tra PHP version

> php -v

- Tạo script php có tên `test.php`

``` !bin/bash
  cd /home/phpcode
  echo "<?php echo 'Hello World';>" > test.php
  php test.php    #run file test.php

  # Output: Hello World
```

- Cấu hình, cài thêm `opcache`, `mysqli` extention - kết nối PHP vs MYSQL, `pdo_mysql` - lib để kết nối PHP vs MYSQL

> docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install opcache
>
> docker-php-ext-install mysqli
>
> docker-php-ext-install pdo_mysql

- Copy file php.ini

> cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

- Restart lại container

> docker restart c-php

- Kiểm tra các extension đã được cài đặt

> php -m

### Apache httpd container

`Apache` là máy chủ `HTTPD` rất phổ biến, nhất là các server Linux

- Pull httpd

> docker pull httpd

- Tạo một container httpd đặt tên là `c-httpd`

> docker run -di -p 8080:80 -p 443:443 --name c-httpd -h httpd -v "/mycode/php":/home/phpcode httpd

- Check httpd đã run

Bật browser trên máy host vào `localhost:8080`

- Thiết lập PHP Handler

Dùng `nano` (apt-get update & apt-get install nano) để update config (`/usr/local/apache2/conf/httpd.conf`)

Thêm dòng
> AddHandler "proxy:fcgi://c-php:9000" .php

Bỏ comment
> LoadModule proxy_module modules/mod_proxy.so
>
> LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so

### Liên kết Apache và PHP-FPM

- Tạo bridge network `www-net`

> docker network create --driver bridge www-net

- Thiết lập container `c-php` và `c-httpd` nối vào mạng này

> docker network connect www-net c-php
>
> docker network connect www-net c-httpd

- Kiểm tra thông tin mạng `www-net`

> docker network inspect www-net

Như vậy 2 container trên đã cùng một network với địa chỉ IP cho từng container. Hai container này đã có thể liên lạc với nhau qua giao tiếp mạng, bạn có thể kiểm tra bằng cách vào container này và dùng lệnh ping đến IP của container kia. Chú ý để có lệnh ping cần cài đặt `apt-get update && apt-get install iputils-ping -y`

- Vào `c-httpd` container mở `httpd.conf` và update thư mục gốc mặc định thành `/home/phpcode/www`

> DocumentRoot "/usr/local/apache2/htdocs" => DocumentRoot "/home/phpcode/www"
>
> <Directory "/usr/local/apache2/htdocs"> => <Directory "/home/phpcode/www">

- Tạo file `index.html` và `test.php` trong /home/phpcode/www để test httpd đã run thành công

- Copy httpd.conf vào thư mục share `/home/phpcode`

> cp /usr/local/apache2/conf/httpd.conf /home/phpcode/httpd.conf

- Remove c-httpd hiện tại vào tạo 1 container mới

> docker run --network www-net --name c-httpd -h httpd -p 9999:80 -p 443:443 -v /mycode/php:/home/phpcode -v /mycode/php/httpd.conf:/usr/local/apache2/conf/httpd.conf httpd

### Cài đặt MYSQL

- Chạy container mysql

> docker run -it --network www-net --name c-mysql -h mysql \
        -v "/mycode/db":/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123  mysql

### Cài đặt WordPress trên Docker

TBC
