
FROM python:3.7-slim
# install the notebook package
RUN pip install --no-cache --upgrade pip && \
    pip install --no-cache notebook

USER root




# create user with a home directory
ARG NB_USER
ARG NB_UID
ENV USER ${NB_USER}
ENV HOME /home/${NB_USER}

RUN apt-get update && \
    apt-get install -y libzmq5 && \
    apt-get install -y wget


RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${NB_UID} \
    ${NB_USER}
WORKDIR ${HOME}

USER ${USER}


# The following snippet is licensed under MIT license
# SEE: https://github.com/jackfirth/racket-docker


ENV RACKET_INSTALLER_URL="http://mirror.racket-lang.org/installers/7.8/racket-7.8-x86_64-linux-natipkg.sh"
ENV RACKET_VERSION=7.8

RUN mkdir ${HOME}/bin

RUN wget --output-document=racket-install.sh -q ${RACKET_INSTALLER_URL} && \
    echo "yes" | sh racket-install.sh --create-dir --unix-style --dest ${HOME} && \
    rm racket-install.sh

ENV PATH="$PATH:${HOME}/bin"
ENV SSL_CERT_FILE="/etc/ssl/certs/ca-certificates.crt"
ENV SSL_CERT_DIR="/etc/ssl/certs"

RUN raco setup
RUN raco pkg config --set catalogs \
    "https://download.racket-lang.org/releases/${RACKET_VERSION}/catalog/" \
    "https://pkg-build.racket-lang.org/server/built/catalog/" \
    "https://pkgs.racket-lang.org" \
    "https://planet-compats.racket-lang.org"

RUN raco pkg install --scope user  --auto iracket

RUN raco iracket install
