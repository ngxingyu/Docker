FROM texlive/texlive:latest
LABEL maintainer="Xing Yu <ngxingyu@hotmail.com>"

ARG VER=1
RUN apt-get update
RUN apt-get install -y openssl curl wget unzip fuse git gcc g++ ripgrep procps
RUN apt-get install -y pip zathura
RUN pip install pynvim

RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer/.config/ && \
    mkdir -p /etc/sudoers.d && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer

USER developer
ENV HOME /home/developer

WORKDIR /tmp
RUN wget https://github.com/neovim/neovim/releases/download/nightly/nvim.appimage
RUN chmod +x nvim.appimage
RUN ./nvim.appimage --appimage-extract

ENV PATH="/tmp/squashfs-root/usr/bin/:${PATH}"

RUN mkdir -p /home/developer/.config

WORKDIR /home/developer/.config/

RUN git clone https://github.com/ngxingyu/nvim.git
WORKDIR /home/developer/.config/nvim
RUN git checkout latex
RUN nvim --headless +'autocmd User PackerComplete sleep 100m | write ~/.packer.sync.result | qall' +PackerInstall
WORKDIR /home/developer

CMD ["nvim", "-v"]
