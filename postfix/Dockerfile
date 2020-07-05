from debian:buster-slim
maintainer Christian Felsing <support@felsing.net>

ARG hostname=mail
ARG domain=example.com
ARG fqname=${hostname}.${domain}

run DEBIAN_FRONTEND=noninteractive apt-get update
run DEBIAN_FRONTEND=noninteractive apt-get install -q -y apt-utils
run DEBIAN_FRONTEND=noninteractive apt-get install -q -y \
  locales

RUN sed -i -e 's/# de_DE.UTF-8 UTF-8/de_DE.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG de_DE.UTF-8
ENV LANGUAGE de_DE:de
ENV LC_ALL de_DE.UTF-8

run DEBIAN_FRONTEND=noninteractive apt update && apt -q -y full-upgrade

run echo ${hostname} > /etc/hostname
add etc/hosts /etc/hosts
add etc/aliases /etc/aliases
run chown root:root /etc/hosts /etc/aliases

# Install Postfix.
run echo "postfix postfix/main_mailer_type string Internet site" > preseed.txt
run echo "postfix postfix/mailname string ${fqname}" >> preseed.txt
# Use Mailbox format.
run debconf-set-selections preseed.txt
run DEBIAN_FRONTEND=noninteractive apt-get install -q -y \
  postfix \
  postfix-pcre \
  s-nail \
  rsyslog \
  python3-pymongo \
  git-core \
  curl \
  netcat \
  rsyslog

RUN groupadd trashmail
RUN useradd -g trashmail -s /bin/bash -md /home/trashmail trashmail

RUN mkdir /tmp/lmtp-server

RUN chown trashmail:trashmail /home/trashmail
RUN chown trashmail:trashmail /tmp/lmtp-server

ADD ./mailname /etc/mailname
ADD ./rsyslog.conf /etc/rsyslog.conf

ADD ./make-cert /tmp/make-cert
RUN chmod 755 /tmp/make-cert
ADD ./postfix /etc/postfix
RUN \
  cd /etc/postfix && \
  postmap relay_domains && \
  postmap transport && \
  /tmp/make-cert smtp.example.com
ADD ./trashmail /home/trashmail
RUN ln -sf /dev/stdout /var/log/syslog

cmd ["sh", "-c", "/usr/sbin/rsyslogd; newaliases; postfix start-fg"]

