FROM opensuse:tumbleweed
MAINTAINER Evgeny Shmarnev "eshmarnev@suse.com"

RUN zypper ar http://download.opensuse.org/repositories/server:/http/openSUSE_Tumbleweed/server:http.repo
RUN zypper -n --gpg-auto-import-keys refresh
RUN zypper -n --gpg-auto-import-keys install nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN touch /srv/www/htdocs/index.html
RUN echo "nginx is working" > /srv/www/htdocs/index.html

EXPOSE 80

CMD [ "/usr/sbin/nginx" ]


