FROM rockylinux/rockylinux:8
ADD epel-8-ams/files/tmp /tmp
RUN yum clean all
RUN yum -y install openssh
RUN yum -y install \
    epel-release
RUN yum install -y sudo
RUN yum makecache
RUN yum install -y python3 python3-pip python3-devel
RUN yum -y install \
    python3-pip \
    python3-requests \
    python3-devel \
    which \
    perl-devel \
    perl-ExtUtils-MakeMaker
RUN yum install -y rpmdevtools rpmlint rpm-build
RUN yum install -y git
RUN dnf install -y epel-release
RUN yum -y install \
    gcc \
    zlib zlib-devel xz-devel \
    openssl-devel ncurses-devel bzip2-devel \
    readline-devel libffi-devel sqlite-devel
RUN yum -y group install "Development Tools"
RUN groupadd -g 1000 jenkins && \
    useradd -m jenkins -u 1000 -g 1000
ADD epel-8-ams/files/etc/rpm-repo.conf /root/scripts/rpm-repo.conf
ADD epel-8-ams/files/scripts/scp-upload.sh /root/scripts/scp-upload.sh
ADD epel-8-ams/files/scripts/pyenv.sh /home/jenkins/
RUN pip3 install -U pip setuptools
RUN pip3 install mock httmock tox-current-env requests pytest coverage unittest-xml-reporting aiofiles lxml unidecode freezegun pyOpenSSL
RUN dnf -y install python38-devel python38-setuptools python38-requests python39-devel python39-setuptools python39-requests python3.11-devel python3.11-setuptools python3.11-requests
RUN dnf -y install mock bc
RUN useradd mockbuild
COPY utils/build-rpm.sh /home/jenkins/build-rpm.sh
USER jenkins
WORKDIR /home/jenkins
RUN curl https://pyenv.run | bash
RUN mv $HOME/.pyenv $HOME/pyenv
RUN source $HOME/pyenv.sh && pyenv install --list | grep ' 3.7' | tail -n1 | xargs pyenv install
RUN source $HOME/pyenv.sh && pyenv install --list | grep ' 3.8' | tail -n1 | xargs pyenv install
RUN source $HOME/pyenv.sh && pyenv install --list | grep ' 3.9' | tail -n1 | xargs pyenv install
RUN source $HOME/pyenv.sh && pyenv install --list | grep ' 3.10' | tail -n1 | xargs pyenv install
RUN source $HOME/pyenv.sh && pyenv install --list | grep ' 3.11' | tail -n1 | xargs pyenv install
RUN source $HOME/pyenv.sh && pyenv local system
RUN rm $HOME/.python-version
USER root
WORKDIR /
RUN su - jenkins rpmdev-setuptree
