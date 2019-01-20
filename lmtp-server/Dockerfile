from ubuntu:18.04
maintainer Christian Felsing <support@felsing.net>

run apt-get update
run apt-get install -q -y apt-utils
run apt-get install -q -y \
  language-pack-de \
  python3-pymongo \
  curl

run update-locale LANG=de_DE.UTF-8

# Use Mailbox format.
#run DEBIAN_FRONTEND=noninteractive apt-get install -q -y postfix

run groupadd trashmail
run useradd -g trashmail -s /bin/bash -md /home/trashmail trashmail
run mkdir /home/trashmail/lmtp
run chown trashmail:trashmail /home/trashmail/lmtp

run mkdir /tmp/lmtp-server
run chown trashmail:trashmail /tmp/lmtp-server

add src/*.py /home/trashmail/lmtp/
add lmtp-server.ini /home/trashmail/.trashmail/

expose 10025
cmd ["sh", "-c", "service syslog-ng start ; su -c /home/trashmail/lmtp/main.py trashmail"]

