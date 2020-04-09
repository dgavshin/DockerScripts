# Build image : docker build -t ft_gitlab .
# Run container : docker run -e GITLAB_HOSTNAME=192.168.99.100 --name ft_gitlab -it -p 80:80 -p 2222:22 -p 443:443 --privileged ft_gitlab

# clone via HTTPS -> GIT_SSL_NO_VERIFY=true git clone ssh://192.168.1.10:2222/admin1337/project
# Clone via SSH -> git clone ssh://git@192.168.1.10:2222/admin1337/project

FROM debian:10

RUN apt update && \
	apt install -y	curl \
					openssh-server \
					ca-certificates \
					postfix
					
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install gitlab-ce

ENV GITLAB_SSL_PATH="/etc/gitlab/ssl"
ENV GITLAB_HOSTNAME="192.168.99.100"
ENV HTTPS_PORT="443"

EXPOSE ${HTTPS_PORT} 22 80

RUN mkdir -p ${GITLAB_SSL_PATH} && \
	chmod 755 ${GITLAB_SSL_PATH} && \
	openssl req	-x509 \
				-days 1461 \
				-nodes \
				-newkey rsa:4096 \
				-out ${GITLAB_SSL_PATH}/${GITLAB_HOSTNAME}.crt \
				-keyout ${GITLAB_SSL_PATH}/${GITLAB_HOSTNAME}.key \
				-subj "/C=RU/ST=SPb/L=SPb/O=Global Security/OU=IT Department/CN=${GITLAB_HOSTNAME}" && \
	chmod 600 ${GITLAB_SSL_PATH}/*

RUN sed -i "s/external_url 'http:\/\/gitlab.example.com'/external_url 'https:\/\/${GITLAB_HOSTNAME}:${HTTPS_PORT}'/" /etc/gitlab/gitlab.rb && \
	echo "letsencrypt['enable'] = false" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['ssl_certificate'] = \"${GITLAB_SSL_PATH}/${GITLAB_HOSTNAME}.crt\"" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['ssl_certificate_key'] = \"${GITLAB_SSL_PATH}/${GITLAB_HOSTNAME}.key\"" >> /etc/gitlab/gitlab.rb && \
	echo "grafana['enable'] = false" >> /etc/gitlab/gitlab.rb
	# echo "gitlab_rails['rake_cache_clear'] = false" >> /etc/gitlab/gitlab.rb

ENTRYPOINT	service ssh start && \
			(/opt/gitlab/embedded/bin/runsvdir-start &) && \
			gitlab-ctl reconfigure && \
			gitlab-ctl tail
