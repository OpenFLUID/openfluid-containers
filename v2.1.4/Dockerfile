FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive
ARG OF_VERSION=2.1.4
ARG OF_DISTRO=xenial
ARG ROF_DATE=20170315


# Install dependencies

RUN \
  apt-get update && \
  apt-get install -y build-essential lsb-release g++ gfortran git cmake \
                     texlive-latex-extra texlive-fonts-extra latex2html doxygen \
                     libboost-all-dev libqt4-dev libgdal1-dev libgeos++-dev \
                     p7zip-full gnuplot graphviz \
                     r-base-dev r-cran-runit \
                     python2.7 \
                     mc nano bash-completion sudo


# Install OpenFLUID and ROpenFLUID

ADD https://www.openfluid-project.org/dloadsproxy/final/v${OF_VERSION}/openfluid_${OF_VERSION}_${OF_DISTRO}_amd64.deb /tmp/openfluid.deb
RUN dpkg -i /tmp/openfluid.deb
RUN rm -f /tmp/openfluid.deb
RUN apt-get install -f -y

ADD https://www.openfluid-project.org/dloadsproxy/final/v${OF_VERSION}/ROpenFLUID_${OF_VERSION}-${ROF_DATE}.tar.gz /tmp/ROpenFLUID.tar.gz
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
