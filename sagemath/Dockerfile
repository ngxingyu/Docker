FROM texlive/texlive:latest
LABEL maintainer="Xing Yu <ngxingyu@hotmail.com>"

ARG VER=1
USER root
RUN apt-get update
RUN apt-get install -y sagemath
EXPOSE 8888

RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer/.config/ && \
    mkdir -p /etc/sudoers.d && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    echo "alias jupyter='sage -n jupyter --ip 0.0.0.0 --port 8888 --no-browser --allow-root'" >> /home/developer/.bashrc && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer

USER developer
ENV HOME /home/developer
WORKDIR /home/developer

CMD ["sage"]
