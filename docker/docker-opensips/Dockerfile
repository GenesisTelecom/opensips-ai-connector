FROM debian:bookworm

USER root
ENV DEBIAN_FRONTEND=noninteractive

ARG OPENSIPS_VERSION=3.5

# Instala dependências do sistema e de build
RUN apt-get update && apt-get install -y \
    git build-essential flex bison libxml2-dev libssl-dev \
    libcurl4-openssl-dev libpcre3-dev libunistring-dev libexpat1-dev \
    libsqlite3-dev libncurses5-dev libtool autoconf pkg-config \
    ca-certificates curl wget nano \
    procps iproute2 net-tools socat && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


# Clona o repositório oficial do OpenSIPS
WORKDIR /usr/src
#RUN git clone --branch ${OPENSIPS_VERSION} https://github.com/OpenSIPS/opensips.git
RUN git clone --recurse-submodules https://github.com/OpenSIPS/opensips.git -b 3.5 opensips

# Compila o OpenSIPS com os módulos desejados
WORKDIR /usr/src/opensips
RUN make include_modules="tm rr sipmsgops signaling cfgutils mi_fifo proto_tcp proto_udp sl event_datagram mi_datagram uac_auth b2b_entities" prefix=/usr/local/opensips all && \
    make prefix=/usr/local/opensips install

# Configuração básica
COPY ./cfg/opensips.cfg /usr/local/opensips/etc/opensips/opensips.cfg

# Porta padrão SIP
EXPOSE 5060/udp

# Entrypoint OpenSIPS
ENTRYPOINT ["/usr/local/opensips/sbin/opensips", "-f", "/usr/local/opensips/etc/opensips/opensips.cfg", "-F"]