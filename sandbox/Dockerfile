FROM phusion/baseimage:latest
RUN rm -f /etc/service/sshd/down && /etc/my_init.d/00_regen_ssh_host_keys.sh
RUN apt-get update -y && apt-get install -y iproute2
RUN ln -s /usr/bin/python3 /usr/bin/python
CMD ["/sbin/my_init", "--enable-insecure-key"]