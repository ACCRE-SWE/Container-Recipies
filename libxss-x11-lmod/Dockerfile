FROM rockylinux:9

# Help instructions
LABEL help="Base container for internal ACCRE use with Lmod support. To access the software stack, make sure to bind mount /accre and /cvmfs."

# Set environment variables
ENV MODULEPATH_ROOT=/accre/arch/easybuild/modules/all \
    MODULEPATH=$MODULEPATH_ROOT/Core:$MODULEPATH_ROOT/BinDist \
    LMOD_SHORT_TIME=86400 \
    LMOD_ADMIN_FILE=/accre/common/lmod/etc/admin.lmod \
    LMOD_RC=/accre/common/lmod/etc/lmodrc.lua

# Source Lmod initialization script in bash shell (needs to be handled at runtime)
# This could be set in an entrypoint script or during container startup

# Install necessary packages
RUN yum -y install wget bzip2 gcc tcl tk tar procps git \
    && yum -y install epel-release \
    && yum -y install lua lua-filesystem lua-posix lua-term make bc

# Install Lmod
RUN export LMOD_VERSION=8.7.47 \
    && wget https://github.com/TACC/Lmod/archive/${LMOD_VERSION}.tar.gz \
    && tar xzf ${LMOD_VERSION}.tar.gz \
    && cd Lmod-${LMOD_VERSION} \
    && ./configure --with-spiderCacheDir=/accre/arch/lmod/cache \
    --with-updateSystemFn=/accre/arch/lmod/cache/timestamp.lmod \
    --with-tcl=no \
    && make install \
    && cd .. \
    && rm -rf ${LMOD_VERSION}.tar.gz Lmod-${LMOD_VERSION}

RUN yum -y install libXScrnSaver

# Install X11 packages for X11 forwarding
RUN yum -y install xorg-x11-server-Xvfb xorg-x11-xauth xterm mesa-dri-drivers mesa-libGL\
    && yum -y install libX11 libXext libXrender libXtst libXrandr libXcursor

# Setup CVMFS binds for ACCRE software stack
RUN mkdir -p /accre /cvmfs

# Copy the ACCRE Lmod bash profile to the appropriate location
# In Docker, you usually need to copy this file from your host system
# Assuming 'accre_lmod_bash_profile' exists in the same directory as the Dockerfile
COPY accre_lmod_bash_profile /etc/profile.d/accre_01_lmod.sh

ENV DISPLAY=:0

# Source the Lmod initialization script during container runtime
CMD ["bash", "-c", "source /usr/local/lmod/lmod/init/bash && exec bash"]
