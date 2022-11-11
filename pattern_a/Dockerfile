FROM debian:latest
RUN apt update && apt -y upgrade
RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y software-properties-common sudo
RUN apt-get install -y x11-apps
RUN apt-get install -y iproute2
CMD ["/bin/bash"]