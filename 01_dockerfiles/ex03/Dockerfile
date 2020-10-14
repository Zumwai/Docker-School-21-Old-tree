FROM	ubuntu

RUN	apt-get update && \
	apt-get upgrade -y && \
	apt-get -y install \
	wget \
	openssh-server \
	ca-certificates \
	postfix

RUN	mkdir /var/run/sshd && \
	echo 'root:123' | chpasswd && \
	sed -i 's/^.*?PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
	sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
ENV 	NOTVISIBLE "in users profile"
RUN	echo "export VISIBLE=now" >> /etc/profile

RUN	wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && \
	chmod 755 ./script.deb.sh && \
	bash ./script.deb.sh


RUN	apt-get install -y tzdata gitlab-ce

EXPOSE 22 80 443

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && sleep 4 && gitlab-ctl reconfigure && tail -f /dev/null

#docker build -t glab .
#docker run -it -p 8080:80 -p 8022:22 -p 8443:443 --privileged glab

