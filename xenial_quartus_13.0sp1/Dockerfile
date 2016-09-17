FROM ubuntu:xenial
MAINTAINER Khaled Hassan <khaled.hassan@gmail.com>

### Install 32-bit libs for Quartus installer

RUN dpkg --add-architecture i386 && apt-get update && apt-get -y install \
    libc6:i386 \
    libncurses5:i386 \
    libstdc++6:i386 \
# Install other dependencies
    ca-certificates \
    libfontconfig1 \
    libfreetype6:i386 \
    libglib2.0-0 \
    libpng12-0 \
    libpng12-0:i386 \
    libsm6 \
    libsm6:i386 \
    libxrender1 \
    locales \
    make \
    xauth \
    xvfb \
    wget
# Set the locale, Quartus expects en_US.UTF-8
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

### Download and run Quartus installer
# Add checksum file
ADD sha1sums.txt /tmp/

# Local files
#ADD QuartusSetupWeb-13.0.1.232.run /tmp/
#ADD max_web-13.0.1.232.qdz /tmp/

# Web download
RUN cd /tmp/ && wget http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_installers/QuartusSetupWeb-13.0.1.232.run -O QuartusSetupWeb-13.0.1.232.run && \
    wget http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_installers/max_web-13.0.1.232.qdz -O max_web-13.0.1.232.qdz && \
# Verify Quartus files
    sha1sum -c sha1sums.txt && \
# Fix file permissions
    chmod +x *.run && \
    chmod +r * && \
# Install Quartus
    ./QuartusSetupWeb-13.0.1.232.run --mode unattended --installdir /opt/altera/13.0sp1 && \ 
# Cleanup to save space
    rm -rf /tmp/* /opt/altera/13.0sp1/uninstall 

# Add Quartus bin directories to the path
ENV PATH $PATH:/opt/altera/13.0sp1/quartus/bin

