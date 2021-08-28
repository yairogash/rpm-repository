FROM yairog/centos-nginx:8

RUN yum update -y

ARG REPOSITORY_FOLDER=/repository

RUN yum install -y createrepo rpm-build rpm-sign wget gcc python3 yum-utils && \
  mkdir ${REPOSITORY_FOLDER}

# INSTALL modulemd-tools
RUN wget https://download-ib01.fedoraproject.org/pub/epel/8/Everything/aarch64/Packages/e/epel-release-8-11.el8.noarch.rpm && \
  rpm -i epel-release-8-11.el8.noarch.rpm && \
  'rm' epel-release-8-11.el8.noarch.rpm && \
  dnf copr enable -y frostyx/modulemd-tools-epel && \
  dnf install -y modulemd-tools && \
  dnf copr disable frostyx/modulemd-tools-epel

# CREATE REPO
WORKDIR ${REPOSITORY_FOLDER}
RUN dnf download --resolve nginx && \
  createrepo . && \
  repo2module  -s stable . modules.yaml && \
  modifyrepo_c --mdtype=modules modules.yaml repodata/

COPY default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]