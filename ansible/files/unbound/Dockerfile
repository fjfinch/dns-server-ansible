FROM ubuntu:latest
RUN apt update && apt install -f unbound -y
#COPY unbound.conf /etc/unbound/unbound.conf
#EXPOSE 5334/tcp
#EXPOSE 5334/udp
#RUN unbound-checkconf
CMD unbound -d
