FROM rockylinux:9

RUN useradd -ms /bin/bash jenkins \
    && echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


RUN yum update -y && \
    yum install -y wget tar gcc-c++ make git sudo rpm-build rpmdevtools  && \
    yum clean all

ENV GO_SRC=go1.25.0.linux-amd64.tar.gz
ENV GO_URL=https://golang.org/dl/$GO_SRC


RUN mkdir -p /home/jenkins/go && chown -R jenkins:jenkins /home/jenkins/go
RUN mkdir -p /data/db && chown -R jenkins:jenkins /data/db


RUN wget $GO_URL && \
    tar -C /usr/local -xzf $GO_SRC && \
    rm $GO_SRC

COPY rocky9-go1.25-mongo6/files/mongodb-org-6.0.repo /etc/yum.repos.d/monodb-org-6.0.repo

RUN yum install -y mongodb-org && \
    yum clean all

COPY --chown=jenkins:jenkins utils/build-rpm.sh /home/jenkins/build-rpm.sh
RUN chmod u+x /home/jenkins/build-rpm.sh

ENV GOPATH=/home/jenkins/go
ENV PATH=$GOPATH/bin:/usr/local/go/bin:$PATH

RUN echo GOPATH=/home/jenkins/go >> /home/jenkins/.bashrc
RUN echo PATH=$GOPATH/bin:/usr/local/go/bin:$PATH >> /home/jenkins/.bashrc
RUN chown -R jenkins:jenkins /home/jenkins/go
RUN go version

RUN go install golang.org/x/tools/cmd/goimports@latest
RUN go install gotest.tools/gotestsum@latest
RUN go install github.com/t-yuki/gocover-cobertura@latest

RUN su - jenkins rpmdev-setuptree

CMD ["/usr/bin/mongod", "--bind_ip_all"]
