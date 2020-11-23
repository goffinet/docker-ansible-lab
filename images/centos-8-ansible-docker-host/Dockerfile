FROM centos:8
MAINTAINER Goffinet François

RUN yum install -y openssh openssh-server openssh-clients && \
    yum -y clean all
RUN mkdir ~/.ssh && ssh-keygen -N "" -t rsa -f ~/.ssh/id_rsa && yum install -y initscripts

COPY common/id_rsa.pub /root/.ssh/authorized_keys
COPY ["common/.vimrc", "common/.profile", "./root/"]
COPY common/start-bis.sh /start.sh

WORKDIR /root

ENV ALLOW_EXIT true

CMD /start.sh
