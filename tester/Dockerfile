from debian:buster-slim
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
run useradd -g trashmail -s /bin/bash -md /home/trashmail trashmail

run apt-get update
RUN apt-get install -y \
  gnupg2 \
  python3-selenium \
  curl

cmd ["sh", "-c", "su -s /bin/bash -c /home/trashmail/test-remote.py trashmail"]

