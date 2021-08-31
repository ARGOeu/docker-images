FROM centos:7
MAINTAINER dvrcic@srce.hr
RUN groupadd jenkins -g 1000 && useradd -u 1000 -g 1000 jenkins -m -d /home/jenkins
RUN passwd -d root
RUN curl -sL https://rpm.nodesource.com/setup_14.x | bash -
RUN yum -y makecache; yum -y update; yum clean all
RUN yum -y install epel-release && \
    yum -y install \
      ack \
      coreutils \
      git \
      htop \
      make \
      nodejs \
      psmisc \
      sudo \
      which \
      wget
RUN yum -y install \
      centos-release-scl \
      scl-utils \
      supervisor
RUN yum -y makecache && yum -y install \
      httpd24-mod_ssl \
      rh-python36-mod_wsgi \
      rh-python36-python-pip
RUN passwd -d apache; usermod --shell /bin/bash apache
RUN scl enable rh-python36 'pip install -U pip' && \
    scl enable rh-python36 'pip3 install virtualenv virtualenvwrapper'
COPY epel-7-poem/venv_poem.sh /etc/profile.d/
COPY epel-7-poem/scl-psql.sh /etc/profile.d/
RUN scl enable rh-python36 'source /etc/profile.d/venv_poem.sh; \
    mkdir -p $WORKON_HOME; \
    mkvirtualenv poem'
WORKDIR /home/jenkins
COPY \
    epel-7-poem/fake-secret \
    epel-7-poem/poem.conf \
    epel-7-poem/entrypoint.sh \
    /home/jenkins/
COPY epel-7-poem/apache/poem-apache.ini /etc/supervisord.d/
COPY epel-7-poem/apache/poem.conf /opt/rh/httpd24/root/etc/httpd/conf.d/
RUN echo "jenkins ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers
USER jenkins
ENTRYPOINT ["/home/jenkins/entrypoint.sh"]

# vim: ft=dockerfile
