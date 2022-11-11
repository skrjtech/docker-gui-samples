FROM pattern-gui
ARG USERNAME
ARG GROUPNAME
ARG PASSWORD
ARG UID
ARG GID
RUN set -x \
    && groupadd -g $GID $GROUPNAME \
    && useradd -m -s /bin/bash -u $UID -g $GID -G sudo $USERNAME \
    && echo $USERNAME:$PASSWORD | chpasswd \
    && echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER $USERNAME
WORKDIR /home/$USERNAME/ws
CMD [ "/bin/bash" ]