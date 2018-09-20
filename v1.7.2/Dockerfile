FROM ubuntu:12.04

ENV DEBIAN_FRONTEND noninteractive
ARG OF_VERSION=1.7.2
ARG OF_BUILD_JOBS=1
ARG ROF_DATE=20141215


# Install dependencies

RUN \
  apt-get update && \
  apt-get install -y build-essential lsb-release g++ gfortran git cmake \
                     texlive-latex-extra texlive-fonts-extra latex2html doxygen \
                     libboost-all-dev libgdal-dev libxml2-dev libcurl4-gnutls-dev \
                     libglibmm-2.4-dev libgtkmm-2.4-dev \
                     p7zip-full gnuplot graphviz \
                     r-base-dev r-cran-runit \
                     python2.7 \
                     mc nano bash-completion sudo


# Install OpenFLUID and ROpenFLUID

ADD https://github.com/OpenFLUID/openfluid/archive/v${OF_VERSION}.tar.gz /tmp/

RUN \
  cd /tmp && \
  tar xvf v${OF_VERSION}.tar.gz
  
RUN \
  cd /tmp/openfluid-${OF_VERSION} && \
  mkdir _build && \
  cd _build && \
  cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release && \
  make -j ${OF_BUILD_JOBS} && \
  make install

RUN \
  rm -rf /tmp/openfluid-${OF_VERSION}


ADD https://www.openfluid-project.org/dloadsproxy/final/v${OF_VERSION}/ROpenFLUID_${OF_VERSION}.tar.gz /tmp/ROpenFLUID.tar.gz

RUN Rscript --vanilla -e 'install.packages("/tmp/ROpenFLUID.tar.gz",repo=NULL)'


# Set default root password

RUN echo 'root:root' | chpasswd


# Create user openfluid

RUN useradd -b /home -m -s /bin/bash openfluid
RUN echo 'openfluid:openfluid' | chpasswd
RUN adduser openfluid sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


# Finalize environment

ENV HOME /home/openfluid
WORKDIR /home/openfluid
USER openfluid


# Define default command.

CMD ["/bin/bash"]
