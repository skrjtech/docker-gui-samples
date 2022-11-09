FROM debian:latest

RUN apt update && apt -y upgrade
RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y software-properties-common sudo
RUN apt-get install -y x11-apps
RUN apt-get install -y iproute2


ARG USERNAME=venv
RUN groupadd -g 1000 $USERNAME
RUN useradd -m -s /bin/bash -u 1000 -g 1000 -G sudo $USERNAME
RUN echo $USERNAME:$USERNAME | chpasswd
RUN echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER $USERNAME
WORKDIR /home/$USERNAME

CMD ["/bin/bash"]