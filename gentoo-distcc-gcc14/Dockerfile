FROM gentoo/stage3:amd64-openrc

RUN mkdir -p /etc/portage/package.accept_keywords/
RUN echo "=sys-devel/gcc-14.2.0 ~amd64" >> /etc/portage/package.accept_keywords/package.accept_keywords
RUN mkdir -p /etc/portage/package.use/
RUN echo "sys-devel/gcc graphite lto pgo zstd" >> /etc/portage/package.use/gcc

RUN     emerge --sync && \
        emerge -j12 distcc gcc:14 && \
        distcc --version && \
        rm -rf /usr/portage/*

RUN eselect gcc set x86_64-pc-linux-gnu-14
RUN gcc -v
RUN echo '192.168.1.0/24' >> /etc/distcc/clients.allow

# This is the operations port
EXPOSE 3632
# This is the statistics port
EXPOSE 3633

#USER distccd

ENTRYPOINT /usr/bin/distccd --no-detach --daemon --stats --log-level info --log-stderr --allow 192.168.1.0/24 -j 12
