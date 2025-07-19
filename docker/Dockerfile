FROM ubuntu:22.04 AS base

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

RUN apt-get update \
 && apt-get install -y software-properties-common

RUN add-apt-repository ppa:flatpak/stable && apt-get update

RUN apt-get install -y \
        python-is-python3 \
        python3-venv \
        flatpak \
        flatpak-builder \
        ostree \
        python3-pip \
        diffoscope &&\
    apt-get clean autoclean &&\
    apt-get autoremove --yes &&\
    rm -rf /var/lib/{apt,dpkg,cache,log}/ &&\
    rm -fr /tmp/* /var/tmp/*

RUN chown root:root /usr/bin/bwrap && chmod u+s /usr/bin/bwrap
RUN useradd -m -d /home/reprotester -s /bin/bash reprotester
RUN chown -R reprotester:reprotester /home/reprotester

FROM base AS image

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN pip install --no-cache-dir git+https://github.com/flathub-infra/flathub-repro-checker.git@main
RUN ln -s /opt/venv/bin/flathub-repro-checker /usr/bin/flathub-repro-checker

USER reprotester
WORKDIR /home/reprotester
