FROM centos:6
MAINTAINER Khaled Hassan <khaled.hassan@gmail.com>

### Install 32-bit libs for Quartus installer
RUN yum install -y \
    redhat-lsb.i686

# Set the locale, Quartus expects en_US.UTF-8
RUN localedef -v -c -i en_US -f UTF-8 en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

### Download and run Quartus installer

# Web download
ADD http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_installers/QuartusSetupWeb-13.0.1.232.run /tmp/
ADD http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_installers/max_web-13.0.1.232.qdz /tmp/

# Local files
#ADD QuartusSetupWeb-13.0.1.232.run /tmp/
#ADD max_web-13.0.1.232.qdz /tmp/


# Verify Quartus files
ADD sha1sums.txt /tmp/
RUN cd /tmp/ && sha1sum -c sha1sums.txt

# Fix file permissions
RUN cd /tmp/ && \
    chmod +x *.run && \
    chmod +r *

# Install Quartus
RUN cd /tmp/ && \
    ./QuartusSetupWeb-13.0.1.232.run --mode unattended --installdir /opt/altera/13.0sp1

# Add Quartus bin directories to the path
ENV PATH $PATH:/opt/altera/13.0sp1/quartus/bin

# Enable talkback (required to run SoC builds)
RUN xvfb-run -a tb2_install --enable

# Cleanup to save space
USER root
RUN rm -rf /tmp/* /opt/altera/13.0sp1/uninstall && \
    yum clean all

