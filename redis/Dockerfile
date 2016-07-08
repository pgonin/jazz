FROM opensuse:latest
MAINTAINER Evgeny Shmarnev "eshmarnev@suse.com"

RUN zypper -n --gpg-auto-import-keys refresh
RUN zypper -n --gpg-auto-import-keys update
RUN zypper -n --gpg-auto-import-keys install redis

EXPOSE 6379

ENTRYPOINT [ "/usr/sbin/redis-server" ]

CMD []
