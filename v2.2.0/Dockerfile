FROM ubuntu:22.04


ENV DEBIAN_FRONTEND noninteractive
ARG OF_VERSION=2.2.0
ARG OF_DISTRO=jammy
ARG ROF_DATE=20250310


# Install.
RUN \
  apt-get update && \
  apt-get install -y build-essential lsb-release g++ gfortran git cmake \
                     texlive-latex-extra texlive-fonts-extra latex2html doxygen pandoc pandoc-citeproc \
                     libboost-all-dev libgdal-dev libgeos++-dev rapidjson-dev \
                     libgl1-mesa-dev \
                     qt6-base-dev qt6-tools-dev libqt6svg6-dev qt6-l10n-tools qt6-base-dev-tools qt6-tools-dev-tools \
                     libqt6uitools6 linguist-qt6 libqt6core5compat6 libqt6gui6 \
                     p7zip-full gnuplot graphviz \
                     r-base-dev r-cran-runit \
                     python3-setuptools \
                     mc nano bash-completion sudo


# Install OpenFLUID and ROpenFLUID

ADD https://www.openfluid-project.org/dloadsproxy/final/v${OF_VERSION}/openfluid_${OF_VERSION}_${OF_DISTRO}_amd64.deb /tmp/openfluid.deb
RUN apt-get install /tmp/openfluid.deb
RUN rm -f /tmp/openfluid.deb

ADD https://www.openfluid-project.org/dloadsproxy/final/v${OF_VERSION}/ROpenFLUID_${OF_VERSION}-${ROF_DATE}.tar.gz /tmp/ROpenFLUID.tar.gz
RUN Rscript --vanilla -e 'install.packages("/tmp/ROpenFLUID.tar.gz",repo=NULL)'


# Qt setup
RUN apt-get install qtchooser
RUN qtchooser -install qt6 $(which qmake6)
ENV QT_SELECT qt6


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
