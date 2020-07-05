from node:12-buster-slim
maintainer Christian Felsing <support@felsing.net>

ENV DEBIAN_FRONTEND noninteractive

run apt-get update
run apt-get install -q -y apt-utils
run DEBIAN_FRONTEND=noninteractive apt-get install -q -y \
  locales \
  nullmailer \
  cron \
  curl \
  git-core

RUN mv /etc/locale.gen /etc/locale.gen.ORIG
RUN echo "de_DE.UTF-8 UTF-8" > /etc/locale.gen
RUN locale-gen de_DE.UTF-8  
ENV LANG de_DE.UTF-8  
ENV LANGUAGE de_DE:de
ENV LC_ALL de_DE.UTF-8
ENV NODE_ENV production
#ENV NODE_ENV development
ENV LOGLEVEL debug

run groupadd trashmail
run useradd -g trashmail -s /bin/false -md /home/trashmail trashmail

add cron-trashmail /var/spool/cron/crontabs/trashmail
run chown trashmail:crontab /var/spool/cron/crontabs/trashmail
add clean-tmp /usr/local/bin/clean-tmp
run chmod 755 /usr/local/bin/clean-tmp

run mkdir /home/trashmail/.trashmail

run cd /home/trashmail && git clone https://github.com/ip6li/trashmail2-client.git trashmail
#run cd /home/trashmail/trashmail && git checkout Development && git pull

add trashmail.json  /home/trashmail/.trashmail/
add trashmail.runtime.json /home/trashmail/.trashmail/
add start-trashmail /home/trashmail/start-trashmail
run chmod 755  /home/trashmail/start-trashmail
add install-trashmail /home/trashmail/install-trashmail
run chmod 755  /home/trashmail/install-trashmail

run chown -R trashmail:trashmail /home/trashmail/

USER trashmail
run /home/trashmail/install-trashmail

USER root
cmd ["sh", "-c", "touch /var/log/syslog ; su -s /bin/bash -c /home/trashmail/start-trashmail trashmail ;  tail -F /var/log/syslog"]

