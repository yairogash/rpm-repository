FROM centos:8

RUN yum update -y

RUN yum install -y createrepo rpm-build rpm-sign wget gcc python3 yum-utils && \
  mkdir repository

# INSTALL modulemd-tools
RUN wget https://download-ib01.fedoraproject.org/pub/epel/8/Everything/aarch64/Packages/e/epel-release-8-11.el8.noarch.rpm && \
  rpm -i epel-release-8-11.el8.noarch.rpm && \
  'rm' epel-release-8-11.el8.noarch.rpm && \
  dnf copr enable -y frostyx/modulemd-tools-epel && \
  dnf install -y modulemd-tools && \
  dnf copr disable frostyx/modulemd-tools-epel

# CREATE REPO
WORKDIR /repository
RUN dnf download --resolve nginx && \
  createrepo . && \
  repo2module  -s stable . modules.yaml && \
  modifyrepo_c --mdtype=modules modules.yaml repodata/



EXPOSE 8000
CMD python3 -u -m http.server