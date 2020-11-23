FROM ubuntu:18.04
MAINTAINER Goffinet François

RUN apt-get update && apt-get install -y rsyslog iputils-ping netbase net-tools openssh-server vim sudo curl python3-pip python3-dev python3-apt \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3 python \
  && pip3 install --upgrade pip
RUN mkdir ~/.ssh && ssh-keygen -N "" -t rsa -f ~/.ssh/id_rsa

COPY common/id_rsa.pub /root/.ssh/authorized_keys
COPY ["common/.vimrc", "common/.profile", "./root/"]
COPY common/start.sh /start.sh

WORKDIR /root

ENV ALLOW_EXIT true

CMD /start.sh
