FROM debian:bullseye

ARG UID_TEST=1000
ARG GID_TEST=1000

RUN apt-get update && apt-get -y install \
        apache2 apache2-suexec-pristine libapache2-mpm-itk
RUN groupadd -g $GID_TEST test
RUN useradd -u $UID_TEST -g test -s /bin/bash -m test
RUN a2dismod mpm_event
RUN a2enmod cgid mpm_prefork suexec
ADD 000-default.conf /etc/apache2/sites-available
ADD 001-userid.conf /etc/apache2/sites-available
ADD html /var/www/html
RUN chown -R test:test /var/www/html
RUN find /var/www/html -name \*.cgi -exec chmod 755 {} \;
RUN a2ensite 001-userid
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
