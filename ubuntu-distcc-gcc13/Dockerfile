FROM ubuntu:22.04

# Prerequisites for adding the repositories
# Remove the apt cache to keep layer-size small.
RUN apt-get update && \
  apt install -y gpg wget software-properties-common && \
  rm -rf /var/lib/apt/lists/* && \
  apt-add-repository ppa:ubuntu-toolchain-r/test -y

RUN apt update && \
  apt install -y \
    distcc \
    gcc-13 \
    g++-13 \
    clang && \
  rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 100 --slave /usr/bin/g++ g++ /usr/bin/g++-13 --slave /usr/bin/gcov gcov /usr/bin/gcov-13

#RUN echo '192.168.1.0/24' >> /etc/distcc/clients.allow

# whitelist
RUN ln -s /usr/bin/distcc /usr/lib/distcc/x86_64-pc-linux-gnu-gcc
RUN ln -s /usr/bin/distcc /usr/lib/distcc/x86_64-pc-linux-gnu-g++
RUN ln -s /usr/bin/distcc /usr/lib/distcc/gcc
RUN ln -s /usr/bin/distcc /usr/lib/distcc/g++

# symlinks
RUN ln -s /usr/bin/x86_64-linux-gnu-gcc /usr/bin/x86_64-pc-linux-gnu-gcc
RUN ln -s /usr/bin/x86_64-linux-gnu-g++ /usr/bin/x86_64-pc-linux-gnu-g++
RUN ln -s /usr/bin/gcc /usr/bin/x86_64-linux-gnu-gcc
RUN ln -s /usr/bin/g++ /usr/bin/x86_64-linux-gnu-g++

# This is the operations port
EXPOSE 3632
# This is the statistics port
EXPOSE 3633

USER distccd

ENTRYPOINT /usr/bin/distccd --no-detach --daemon --stats --log-level info --log-stderr --allow 192.168.1.0/24 -j 12

# Spawn four instances on different ports
# docker create -p3632:3632 -p3633:3633 --name distcc my-distcc
# docker run -d --restart unless-stopped distcc
