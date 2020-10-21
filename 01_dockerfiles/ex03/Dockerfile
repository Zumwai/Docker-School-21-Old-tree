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
	git \
	openssl

RUN	git clone https://github.com/Zumwai/self_cert_tempo.git ssl && \
	chmod 777 /ssl/server.crt && \
	chmod 777 /ssl/server.key && \
	chmod 777 /ssl/server.csr

#RUN	sed -i "s|external_url 'http://gitlab.example.com'|external_url 'https://gitlab.example.com'|g" /etc/gitlab/gitlab.rb && \
#	sed -i "s|# nginx['enable'] = true|nginx['enable'] = true| g" /etc/gitlab/gitlab.rb && \
#	sed -i "s|# nginx['redirect_http_to_https'] = false|nginx['redirect_http_to_https'] = true| g" /etc/gitlab/gitlab.rb && \
#	sed -i "s|# nginx['ssl_certificate'] = \"/etc/gitlab/ssl/#{node['fqdn']}.crt\"|nginx['ssl_certificate'] = \"/ssl/server.crt\"| g" /etc/gitlab/gitlab.rb && \
#	sed -i "s|# nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/#{node['fqdn']}.key\"|nginx['ssl_certificate_key'] = \"/ssl/server.key\"| g" /etc/gitlab/gitlab.rb

#RUN	mkdir /ssl
#WORKDIR	/ssl
#RUN	openssl req -x509 -out localhost.crt -keyout localhost.key \
# 	-newkey rsa:2048 -nodes -sha256 \
# 	-subj '/CN=localhost' -extensions EXT -config <( \
#  	printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")

RUN	sed -i "s|external_url 'http://gitlab.example.com'|external_url 'https://gitlab.example.com'| g" /etc/gitlab/gitlab.rb && \
	sed -i "1221s|.*|nginx['enable'] = true| g" /etc/gitlab/gitlab.rb && \
	sed -i "1222s|.*|nginx['client_max_body_size'] = '250m'| g" /etc/gitlab/gitlab.rb && \
	sed -i "1223s|.*|nginx['redirect_http_to_https'] = true| g" /etc/gitlab/gitlab.rb && \
	sed -i "1235s|.*|nginx['ssl_certificate'] = \"/ssl/server.crt\"| g" /etc/gitlab/gitlab.rb && \
	sed -i "1236s|.*|nginx['ssl_certificate_key'] = \"/ssl/server.key\"| g" /etc/gitlab/gitlab.rb && \
	sed -i "1242s|.*|nginx['ssl_protocols'] = \"TLSv1.2 TLSv1.3\"|g" /etc/gitlab/gitlab.rb

EXPOSE 22 80 443

#ENTRYPOINT	service ssh start && /bin/bash

ENTRYPOINT service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && sleep 4 && gitlab-ctl reconfigure && tail -f /dev/null

#docker build -t glab .
#docker run -it -p 8080:80 -p 8022:22 -p 8443:443 --privileged glab

