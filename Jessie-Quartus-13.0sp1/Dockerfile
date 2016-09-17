FROM jessie-quartus-base:latest
MAINTAINER Khaled Hassan <khaled.hassan@gmail.com>

USER root

# Web download
#ADD http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_installers/QuartusSetupWeb-13.0.1.232.run /tmp/
#ADD http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_installers/max_web-13.0.1.232.qdz /tmp/

# Local files
ADD QuartusSetupWeb-13.0.1.232.run /tmp/
ADD max_web-13.0.1.232.qdz /tmp/


# Verify Quartus files
ADD sha1sums.txt /tmp/
RUN cd /tmp/ && sha1sum -c sha1sums.txt

# Fix file permissions
RUN cd /tmp/ && \
    chmod +x *.run && \
    chmod +r *

# Install Quartus
USER builder
RUN cd /tmp/ && \
    ./QuartusSetupWeb-13.0.1.232.run --mode unattended --installdir /opt/altera/13.0sp1/

# Add Quartus bin directory to the path
ENV PATH $PATH:/opt/altera/13.0sp1/quartus/bin

# disabled for now as I probably don't need this # Enable talkback (required to run SoC builds)
#RUN xvfb-run -a tb2_install --enable

# Cleanup to save space
USER root
RUN rm -rf /tmp/* /opt/altera/13.0sp1/uninstall && \
    apt-get clean

USER builder

# !!!-WARNING-!!!
# Flatten the resulting image or it will be GIGANTIC!
#
# It is expected that at some point, Docker will have a native command
# for doing this, but for now the following method is required.
#
# Create a container from the image:
#   docker run <docker-image:big> /bin/true
#
# Find the container ID:
#   docker ps -a
#
# Get the docker-rebase script from here:
#   https://github.com/docbill/docker-scripts
#
# Rebase the big image to make it MUCH smaller:
#   docker-rebase <container-ID> <docker-image:little>
#
# Tag the image as needed:
#   docker tag <docker-image:little> <final-name:version>
#   docker tag <docker-image:little> <final-name:latest>
#
# You may want to limit your interface bandwidth to avoid killing your 'net connection:
#   sudo wondershaper eth0 500000 6000
#
# ...and push to the Docker hub:
#   docker push <final-name:version>
#   docker push <final-name:latest>
#
#   sudo wondershaper clear eth0
