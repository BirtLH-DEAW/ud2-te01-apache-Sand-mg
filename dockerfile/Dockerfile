# Imagen
FROM ubuntu:latest

# Actualizar e instalar paquetes necesarios
RUN apt-get update && apt-get install -y apache2 openssl apache2-utils

# Crear las carpetas para los sitios web
RUN mkdir -p /var/www/html/smuro/privado

# Copiar archivos de sitios web
COPY html/index.html /var/www/html/
COPY html/css_index /var/www/html
COPY html/virtualhost.html /var/www/html/smuro/
COPY html/css_virtualhost /var/www/html/smuro
COPY html/privado.html /var/www/html/smuro/privado
COPY html/css_privado /var/www/html/smuro/privado

# Configurar el directorio "privado"
RUN htpasswd -cb /etc/apache2/.htpasswd deaw deaw

# Copiar configuración de Apache
COPY conf/smuro.conf /etc/apache2/sites-available/smuro.conf

# Habilitar módulos y sitios
RUN a2ensite smuro.conf && a2enmod ssl rewrite

# Generar certificado autofirmado
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/smuro.key \
    -out /etc/ssl/certs/smuro.crt \
    -subj "/C=ES/ST=Zaragoza/L=Zaragoza/O=BirtLH/OU=BirtLH/CN=www.smuro.eus"

# Exponer los puertos
EXPOSE 80 443

# Comando para iniciar Apache en primer plano
CMD ["apache2ctl", "-D", "FOREGROUND"]
