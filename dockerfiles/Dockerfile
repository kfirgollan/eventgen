FROM redis:7.0.0-alpine

RUN apk --no-cache upgrade && \
	apk add --no-cache --update \
	python3 \
	python3-dev \
	gcc \
	libc-dev \
	libffi-dev \
	openssl-dev \
	libxml2-dev \
	libxslt-dev \
	bash \
	sudo \
	openssh \
	tar \
	acl \
	g++ \
	git \
	curl && \
	curl https://bootstrap.pypa.io/get-pip.py --output /tmp/get-pip.py && \
	python3 /tmp/get-pip.py && \
	rm -rf /tmp/* && \
	rm -rf /var/cache/apk/* && \
	ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa && \
	mkdir -p /var/run/sshd && \
	mkdir -p /root/.ssh && \
	chmod 0700 /root/.ssh && \
	passwd -u root || true && \
	# install dependencies of conductor2 used by perf
	pip3 install filelock twisted requests queuelib ujson psutil crochet msgpack-python unidecode attrdict service_identity

COPY dockerfiles/sshd_config /etc/ssh/sshd_config
COPY dockerfiles/entrypoint.sh /sbin/entrypoint.sh
COPY dist/splunk_eventgen*.tar.gz /root/splunk_eventgen.tgz
RUN pip3 install /root/splunk_eventgen.tgz && \
	rm /root/splunk_eventgen.tgz
COPY pyproject.toml /usr/lib/python3.7/site-packages/splunk_eventgen/pyproject.toml
COPY poetry.lock /usr/lib/python3.7/site-packages/splunk_eventgen/poetry.lock

EXPOSE 2222 6379 9500
RUN chmod a+x /sbin/entrypoint.sh
WORKDIR /usr/lib/python3.7/site-packages/splunk_eventgen
ENTRYPOINT ["/sbin/entrypoint.sh"]
