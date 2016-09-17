FROM ubuntu:xenial
MAINTAINER Khaled Hassan <khaled.hassan@gmail.com>

### Install wget to download Quartus installer
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

RUN apt-get update && apt-get -y install \
    libglib2.0-0 \
    locales \
    wget

# Set the locale, Quartus expects en_US.UTF-8
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

### Download and run Quartus installer
# Add checksum file
ADD md5sums.txt /tmp/

# Local files
#ADD QuartusSetupWeb-13.0.1.232.run /tmp/
#ADD max_web-13.0.1.232.qdz /tmp/

# Web download
RUN cd /tmp/ && \
    wget http://download.altera.com/akdlm/software/acdsinst/16.0/211/ib_installers/QuartusLiteSetup-16.0.0.211-linux.run -O QuartusLiteSetup-16.0.0.211-linux.run && \
    wget http://download.altera.com/akdlm/software/acdsinst/16.0/211/ib_installers/max-16.0.0.211.qdz -O max-16.0.0.211.qdz && \
# Verify Quartus files
    md5sum -c md5sums.txt && \
# Fix file permissions
    chmod +x *.run && \
    chmod +r * && \
# Install Quartus
    ./QuartusLiteSetup-16.0.0.211-linux.run --mode unattended --installdir /opt/altera/16.0 && \
# Cleanup to save space
    rm -rf /tmp/* /opt/altera/16.0/uninstall

# Add Quartus bin directories to the path
ENV PATH $PATH:/opt/altera/16.0/quartus/bin

