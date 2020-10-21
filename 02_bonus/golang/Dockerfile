FROM	debian

LABEL	maintainer aophion@student.21-school.ru

RUN	apt-get update -y
RUN	apt-get upgrade -y
RUN	apt-get install -y software-properties-common
RUN	add-apt-repository ppa:longsleep/golang-backports
RUN	apt-get install -y golang-go vim

ENTRYPOINT	/bin/bash

#docker build -t golocal .
#docker run -ti golocal
