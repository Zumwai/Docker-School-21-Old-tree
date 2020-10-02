FROM    debian

LABEL   maintainer aophion@student.21-school.ru

ENV     TS3SERVER_LICENSE=accept

EXPOSE  9987/udp 10011 30033

RUN     adduser --disabled-login teamspeak && \
        apt update && \
        apt upgrade -y && \
        apt install -y wget bzip2

USER    teamspeak

WORKDIR /home/teamspeak

RUN     wget http://dl.4players.de/ts/releases/3.12.1/teamspeak3-server_linux_amd64-3.12.1.tar.bz2 && \
        tar -xvf teamspeak3-server_linux_amd64-3.12.1.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT /bin/bash /home/teamspeak/teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh
