# Apache httpd with mod_tora

FROM ubuntu:trusty

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y python-software-properties software-properties-common \
	&& add-apt-repository ppa:haxe/releases -y \
	&& apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
		apache2 \
		neko-dev \
		haxe \
	&& rm -r /var/lib/apt/lists/*

RUN rm -rf /var/www/html \
	&& mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html \
	&& chown -R www-data:www-data /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html 
RUN a2enmod rewrite
RUN mv /etc/apache2/apache2.conf /etc/apache2/apache2.conf.dist && rm /etc/apache2/conf-enabled/* /etc/apache2/sites-enabled/*
COPY apache2.conf /etc/apache2/apache2.conf

RUN { \
		echo 'LoadModule neko_module /usr/lib/neko/mod_neko2.ndll'; \
		echo 'LoadModule tora_module /usr/lib/neko/mod_tora2.ndll'; \
		echo 'AddHandler tora-handler .n'; \
	} > /etc/apache2/mods-enabled/haxelib_test.conf \
	&& apachectl stop

ENV HAXELIB_PATH /haxelib

RUN mkdir "$HAXELIB_PATH" && haxelib setup "$HAXELIB_PATH" \
	&& haxelib install tora 1.8.1

WORKDIR /var/www/html

EXPOSE 80
CMD apachectl restart && haxelib run tora