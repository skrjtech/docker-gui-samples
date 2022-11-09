FROM debian:latest

RUN apt update && apt -y upgrade
RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y software-properties-common sudo
RUN apt-get install -y x11-apps
RUN apt-get install -y iproute2


ARG USERNAME=venv
ARG UID=1000
ARG GID=1000
RUN groupadd -g $GID $USERNAME
RUN useradd -m -s /bin/bash -u $UID -g $GID -G sudo $USERNAME
RUN echo $USERNAME:$PASSWORD | chpasswd
RUN echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER $USERNAME
WORKDIR /home/$USERNAME

CMD ["/bin/bash"]