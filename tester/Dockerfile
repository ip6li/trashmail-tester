from ubuntu:bionic
maintainer Christian Felsing <support@felsing.net>

run apt-get update
run apt-get install -q -y apt-utils
run apt-get install -q -y \
  locales \
  rsyslog

RUN mv /etc/locale.gen /etc/locale.gen.ORIG
RUN echo "de_DE.UTF-8 UTF-8" > /etc/locale.gen
RUN locale-gen de_DE.UTF-8  
ENV LANG de_DE.UTF-8  
ENV LANGUAGE de_DE:de
ENV LC_ALL de_DE.UTF-8

run groupadd trashmail
run useradd -g trashmail -s /bin/false -md /home/trashmail trashmail

run apt-get update
RUN apt-get install -y \
  gnupg2 \
  python3-selenium \
  curl

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs

run mkdir /home/trashmail/trashmail

add trashmail2-client/test /home/trashmail/trashmail
add start-tests /home/trashmail/start-tests
run chmod 755  /home/trashmail/start-tests
add install-tests /home/trashmail/install-tests
run chmod 755  /home/trashmail/install-tests

add test-remote.py /home/trashmail/test-remote.py
run chmod 755 /home/trashmail/test-remote.py

run chown -R trashmail:trashmail /home/trashmail/

USER trashmail
run /home/trashmail/install-tests

USER root
cmd ["sh", "-c", "service rsyslog start ; tail -F /var/log/syslog"]
#cmd ["sh", "-c", "service rsyslogd start ; su -s /bin/bash -c /home/trashmail/start-trashmail trashmail; tail -F /var/log/syslog"]

