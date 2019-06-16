FROM alpine:${ALPINE_VERSION:-3.9}

ARG ANSIBLE_VERSION

ENV ANSIBLE_VERSION=${ANSIBLE_VERSION:-2.8.1}

RUN set -eux && \
    addgroup ansible && adduser -D -G ansible ansible && \
    apk --no-cache add \
    ca-certificates openssh-client curl sudo python3 py3-pip && \
    apk --no-cache add --virtual .build-deps \
    python3-dev libffi-dev openssl-dev build-base && \
    pip3 install --upgrade pip && \
    su ansible -c 'pip3 install --user ansible==${ANSIBLE_VERSION}' && \
    ln -s /home/ansible/.local/bin/ansible /usr/bin/ && \
    apk del --purge .build-deps && \
    mkdir -p /etc/ansible && \
    echo 'localhost ansible_connection=local ansible_python_interpreter=/usr/bin/python3' >> /etc/ansible/hosts && \
    chown -R ansible:ansible /etc/ansible

VOLUME ["/etc/ansible/"]

USER ansible

WORKDIR /home/ansible/

ENTRYPOINT ["/bin/ash"]

#ENTRYPOINT ["/usr/bin/ansible"]
#CMD ["--version"]
