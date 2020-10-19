FROM	ubuntu

RUN	apt-get update && \
	apt-get upgrade -y && \
	apt-get -y install \
	wget \
	openssh-server \
	ca-certificates \
	python3.8 \
	postfix \
	vim

RUN	mkdir /var/run/sshd && \
	echo 'root:123' | chpasswd && \
	sed -i 's|#PermitRootLogin prohibit-password|PermitRootLogin yes|g' /etc/ssh/sshd_config && \
	sed -i 's|required|optional|g' /etc/pam.d/sshd
ENV 	NOTVISIBLE "in users profile"
RUN	echo "export VISIBLE=now" >> /etc/profile

RUN	wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && \
	chmod 755 ./script.deb.sh && \
	bash ./script.deb.sh

RUN	apt-get clean && \
	apt-get update --fix-missing -y && \
	apt-get install -y \
	tzdata \
	gitlab-ce \
	git

RUN	sed -i "s|external_url 'http://gitlab.example.com'|external_url 'https://gitlab.example.com'|g" /etc/gitlab/gitlab.rb && \
	sed -i "s|# nginx['enable'] = true|nginx['enable'] = true| g" /etc/gitlab/gitlab.rb && \
	sed -i "s|# nginx['redirect_http_to_https'] = false|nginx['redirect_http_to_https'] = true| g" /etc/gitlab/gitlab.rb && \
	sed -i "s|# nginx['ssl_certificate'] = \"/etc/gitlab/ssl/#{node['fqdn']}.crt\"|nginx['ssl_certificate'] = \"/etc/gitlab/ssl/server.crt\"| g" /etc/gitlab/gitlab.rb && \
	sed -i "s|# nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/#{node['fqdn']}.key\"|nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/server.key\"| g" /etc/gitlab/gitlab.rb
RUN	git clone https://github.com/Zumwai/self_cert_tempo.git ssl
WORKDIR	/etc/gitlab/ssl/
RUN	chmod 400 server.crt && \
	chmod 400 server.key

EXPOSE 22 80 443
ENTRYPOINT	service ssh start && /bin/bash

#ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && sleep 4 && gitlab-ctl reconfigure && tail -f /dev/null

#docker build -t glab .
#docker run -it -p 8080:80 -p 8022:22 -p 8443:443 --privileged glab

