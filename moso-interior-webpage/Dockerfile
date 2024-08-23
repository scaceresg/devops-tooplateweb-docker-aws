FROM ubuntu:latest

LABEL "Author"="scaceresg"
LABEL "Project"="moso-interior"

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /var/www/html

RUN apt update && apt install apache2 git -y

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

ADD moso-interior.tar.gz /var/www/html

EXPOSE 80

VOLUME /var/log/apache2
