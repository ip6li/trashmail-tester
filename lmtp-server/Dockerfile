from ubuntu:18.04
maintainer Christian Felsing <support@felsing.net>

RUN apt-get update
RUN apt-get install -q -y apt-utils
RUN apt-get install -q -y \
  language-pack-de \
  python3-pymongo \
  git-core \
  curl

RUN mv /etc/locale.gen /etc/locale.gen.ORIG
RUN echo "de_DE.UTF-8 UTF-8" > /etc/locale.gen
RUN locale-gen de_DE.UTF-8  
ENV LANG de_DE.UTF-8  
ENV LANGUAGE de_DE:de
ENV LC_ALL de_DE.UTF-8

RUN groupadd trashmail
RUN useradd -g trashmail -s /bin/bash -md /home/trashmail trashmail

RUN mkdir /tmp/lmtp-server

RUN cd /home/trashmail && git clone https://github.com/ip6li/lmtp-server.git lmtp
ADD lmtp-server.ini /home/trashmail/.trashmail/

RUN chown trashmail:trashmail /home/trashmail
RUN chown trashmail:trashmail /tmp/lmtp-server

cmd ["sh", "-c", "su -c /home/trashmail/lmtp/src/main.py trashmail ; tail -f /var/log/dpkg.log"]

