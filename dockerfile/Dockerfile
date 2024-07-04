# FROM ubuntu:jammy
ARG BASE_IMAGE=debian:stable-slim
FROM ${BASE_IMAGE}

# update_channel
ARG update_channel=release
ENV update_channel=${update_channel}

RUN apt-get update --ignore-missing \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        gpg \
        curl \
        systemd \
        lsb-release \
        # man \
        expect \
        # iproute2 \
        # jq \
        # nftables \
        # iputils-ping \
        # vim \
        # sudo \
        # procps \
    # Set OS_ARCH, OS_VER
    && export OS_ARCH=$(dpkg --print-architecture) \
    && export OS_VER=$(lsb_release -cs) \
    # ===== adguardvpn-cli =====
    && export URL_SH="https://raw.githubusercontent.com/AdguardTeam/AdGuardVPNCLI/master/scripts/${update_channel}/install.sh" \
    && export FILE_SH="install.sh" \
    && curl -fsSL ${URL_SH} -o ${FILE_SH} \
    && expect -c "\
    spawn ${FILE_SH}; \
    expect \"Would you like to link the binary to /usr/local/bin? [y/N]\" { send \"y\\r\" }; \
    expect eof\
    " \
    # && yes "y" | bash ${FILE_SH} \
    # && export version=$(grep "version='" ${FILE_SH} | awk -F"'" '{print $2}') \
    # && echo "version=${version}" \
    # # Determine CPU architecture for cpu variable
    # && export cpu=$(case $(dpkg --print-architecture) in \
    #     amd64) echo "x86_64" ;; \
    #     arm64 | aarch64) echo "aarch64" ;; \
    #     *) echo "unsupported" ;; \
    #     esac) \
    # && export exe_name='adguardvpn-cli' \
    # && export os='linux' \
    # && export pkg_ext='tar.gz' \
    # && export pkg_name="${exe_name}-${version}-${os}-${cpu}.${pkg_ext}" \
    # && echo "pkg_name=${pkg_name}" \
    # && export URL_PKG="https://github.com/AdguardTeam/AdGuardVPNCLI/releases/download/v${version}-${update_channel}/${pkg_name}" \
    # # Install adguardvpn-cli package
    # && curl -fsSL ${URL_PKG} -o ${pkg_name} \
    # && dpkg -i ${pkg_name} \
    # && rm ${pkg_name} \
    # ENTRYPOINT startapp.sh
    && echo "#!/bin/sh\n${exe_name} --help\n" > /startapp.sh \
    && chmod +x /startapp.sh \
    # ===== Cleanup =====
    # ----- Clean pre-install -----
    && apt-get purge --autoremove -y \
        # curl \
        lsb-release \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/log/*.log

ENTRYPOINT ["/startapp.sh"]