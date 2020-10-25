FROM	ubuntu:20.04

LABEL	maintainer <aophion@student.21-school.ru>

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone


RUN	apt-get update -y && \
	apt-get upgrade -y && \
	apt-get install -y \
	git \
	vim \
	default-jre

ENTRYPOINT /bin/bash
