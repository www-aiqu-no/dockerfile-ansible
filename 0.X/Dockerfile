ARG ALPINE_RELEASE

FROM alpine:${ALPINE_RELEASE:-3.9}

LABEL maintainer="rune.ronneseth@aiqu.no" \
      description="This image is for running ansible from within a container"

ARG ANSIBLE_VERSION

ENV ANSIBLE_VERSION=${ANSIBLE_VERSION:-2.8.1}

RUN set -eux \
    && \
      addgroup ansible && \
      adduser -D -G ansible ansible \
    && \
      apk --no-cache add \
        sudo \
        ca-certificates \
        openssh-client \
        python3 \
        py3-yaml \
        py3-pip \
    && \
      apk --no-cache add --virtual .build-deps \
        python3-dev \
        libffi-dev \
        openssl-dev \
        g++ \
        make \
    && \
      pip3 install --no-cache-dir --upgrade pip && \
      pip3 install --no-cache-dir --upgrade setuptools && \
      pip3 install --no-cache-dir --upgrade docker && \
      su ansible -c 'pip3 install --no-cache-dir --user ansible==${ANSIBLE_VERSION}' \
    && \
      apk del --purge .build-deps \
    && \
      ln -s /home/ansible/.local/bin/ansible /usr/bin/ \
    && \
      echo '%ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/050-NOPASSWD

RUN mkdir -p /etc/ansible \
    && \
      printf '%s\n' \
      '---' \
      'all:' \
      '  hosts:' \
      '    master:' \
      '      ansible_connection: local' \
      '      ansible_python_interpreter: /usr/bin/python3' >> /etc/ansible/hosts \
    && \
      chown -R ansible:ansible /etc/ansible

USER ansible

WORKDIR /home/ansible/

VOLUME ["/home/ansible/"]

ENTRYPOINT ["/bin/ash"]

#ENTRYPOINT ["/usr/bin/ansible"]
#CMD ["--version"]
