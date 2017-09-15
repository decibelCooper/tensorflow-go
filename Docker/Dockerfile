FROM dbcooper/arch:2017-09-08

# Set up basic environment
RUN pacman -S --noconfirm \
	sed \
	sudo

RUN useradd -m -G wheel tfgo && \
    sed -i.bak 's/# \(%wheel ALL=(ALL) NOPASSWD: ALL\)/\1/' /etc/sudoers

USER tfgo
WORKDIR /home/tfgo

CMD /bin/bash

# Tensorflow, jupyter, and python
RUN sudo pacman -S --noconfirm \
	jupyter-notebook \
	python-numpy \
	python-tensorflow \
	tensorflow

RUN sudo printf '#!/bin/bash\njupyter-notebook --ip="*" --no-browser\n' > /usr/bin/jupyter-wrapper && \
	sudo chmod +x /usr/bin/jupyter-wrapper

CMD /usr/bin/jupyter-wrapper

# Go, gophernotes, and tensorflow for go
ENV GOPATH /opt/Go
ENV PATH "${GOPATH}/bin:${PATH}"

RUN sudo pacman -S --noconfirm \
	gcc \
	git \
	go \
	mathjax \
	pkg-config

RUN go get golang.org/x/tools/cmd/goimports && \
	go get -tags zmq_4_x github.com/gopherds/gophernotes && \
	mkdir -p ~/.local/share/jupyter/kernels/gophernotes && \
	cp -r $GOPATH/src/github.com/gopherds/gophernotes/kernel/* ~/.local/share/jupyter/kernels/gophernotes && \
	rm -rf /opt/Go/src

RUN go get github.com/tensorflow/tensorflow/tensorflow/go && \
	rm -rf /opt/Go/src
