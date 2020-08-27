<p align="center"><img src="https://miro.medium.com/max/792/1*AeB4I13zDQRvqquZHudNiw.png" width="600"></p>

# Docker con Laravel

**Nota:** Este Docker fue creado en base a una imagen de Ubuntu18.04.

Extensiones de PHP 7.2:

* FPM
* MySQL.
* SQLSERVER.
* SOAP.
* XML.
* ZIP.

Este proyecto va acompañado de docker-compose. Se debe crear una carpeta raiz en donde se guardan los siguientes archivos:

### Archivo docker-compose.yml

```.yml
services:
  web:
    container_name: nombre_del_contenedor
    build: .
    expose:
     - 80 #puerto del contenedor
    ports:
     - 8000:80
    volumes:
      - /var/www/html/nombre_del_proyecto:/var/www/html/nombre_del_proyecto
      - $PWD/log/nginx:/var/log/nginx
      - $PWD/nginx:/etc/nginx/sites-available
```
### Archivo Dockerfile

Este archivo debe estar en raiz junto con el archivo docker-compose.

### Carpeta Log

Crear una carpeta nginx y luego un archivo dentro de la carpeta error.log

### Carpeta NGINX

Crear archivo de configuración para nginx, en este caso se llama default el archivo.

```

server {
        listen 80;
        listen [::]:80 ipv6only=on;

        # Log files for Debugging
        access_log /var/log/nginx/laravel-access.log;
        error_log /var/log/nginx/laravel-error.log;

        root /var/www/html/nombre_del_proyecto/public;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;

        location = /50x.html {
                root /usr/share/nginx/html;

        }

        location ~ \.php$ {
                try_files $uri /index.php =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }
}
```
<p align="center"><img src="https://www.portalvallecas.es/wp-content/uploads/2013/09/estamostrabajando-secciones.jpg" width="600"></p>


