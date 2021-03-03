FROM alpine:3.11

LABEL maintainer="Jose Quintana <git.io/joseluisq>"

ARG USER_NAME
ARG USER_HOME_DIR

ENV BUILD_DEPS="gettext" \
    RUNTIME_DEPS="libintl" \
    \
    USER_NAME=${USER_NAME:-mysql} \
    USER_HOME_DIR=${USER_HOME_DIR:-/home/${USER_NAME}}

RUN adduser -h ${USER_HOME_DIR} -s /sbin/nologin -u 1000 -D ${USER_NAME} && \
    \
    apk --no-cache add ca-certificates tzdata mysql-client nano dumb-init && \
    \
    set -ex; \
    apkArch="$(apk --print-arch)"; \
    case "$apkArch" in \
        armhf) arch='arm' ;; \
        aarch64) arch='arm64' ;; \
        x86_64) arch='amd64' ;; \
        *) echo >&2 "error: unsupported architecture: $apkArch"; exit 1 ;; \
    esac; \
    \
    apk add --update $RUNTIME_DEPS && \
    apk add --virtual build_deps $BUILD_DEPS && \
    \
    cp /usr/bin/envsubst /usr/local/bin/envsubst && \
    apk del build_deps && \
    wget --quiet -O /tmp/enve.tar.gz "https://github.com/joseluisq/enve/releases/download/v1.0.0/enve_v1.0.0_linux_$arch.tar.gz"; \
	tar xzvf /tmp/enve.tar.gz -C /usr/local/bin enve; \
	rm -f /tmp/enve.tar.gz; \
	chmod +x /usr/local/bin/enve



