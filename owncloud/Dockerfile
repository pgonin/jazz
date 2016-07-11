FROM opensuse:42.1

RUN zypper ar http://download.opensuse.org/repositories/isv:/ownCloud:/community/openSUSE_Leap_42.1/isv:ownCloud:community.repo
RUN zypper -n --gpg-auto-import-keys ref
RUN zypper -n --gpg-auto-import-keys install glibc-locale

ENV LANG en_US.UTF-8

RUN zypper -n --gpg-auto-import-keys install owncloud php5-mysql

EXPOSE 80

ENTRYPOINT ["/usr/sbin/apache2ctl"]

CMD ["-D", "FOREGROUND"]
