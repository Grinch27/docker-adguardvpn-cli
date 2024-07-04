# FROM ubuntu:jammy
ARG BASE_IMAGE=debian:slim
FROM ${BASE_IMAGE}

# update_channel
ARG update_channel=stable
ENV update_channel=${update_channel}

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update --ignore-missing \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        gpg \
        curl \
        lsb-release \
        systemd \
        # man \
        # expect \
        # iproute2 \
        # jq \
        # nftables \
        # iputils-ping \
        # vim \
        # sudo \
        # procps \
    # Set OS_ARCH, OS_VER
    && OS_ARCH=$(dpkg --print-architecture) \
    && OS_VER=$(lsb_release -cs) \
    # Install adguardvpn-cli
    && curl -fsSL https://raw.githubusercontent.com/AdguardTeam/AdGuardVPNCLI/master/scripts/${update_channel}/install.sh | sh -s -- -v \
    # Clean pre-install
    && apt-get purge --autoremove -y \
        # curl \
        lsb-release \
    # Clean apt
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/log/*.log \
    && unset DEBIAN_FRONTEND \
    # ENTRYPOINT startapp.sh
#     && echo "#!/bin/sh\n\n" > /startapp.sh \
#     && chmod +x /startapp.sh
    
# ENTRYPOINT ["/startapp.sh"]
