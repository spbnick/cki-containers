FROM fedora:29
RUN echo "fastestmirror=true" >> /etc/dnf/dnf.conf
ADD dnf-transaction.txt /tmp/dnf-transaction.txt

# Install OS packages
RUN dnf -y --nodocs shell /tmp/dnf-transaction.txt && \
      dnf builddep -y kernel && \
      dnf clean all && \
      rm -fv /tmp/dnf-transaction.txt

# Install python packages
RUN pip2 install -U pip setuptools virtualenv && \
      pip3 install -U pip setuptools virtualenv

# Add CA certificate
ADD rh-ca.crt /etc/pki/ca-trust/source/anchors/
RUN update-ca-trust

# For supporting arbitrary user IDs
ENV NSS_WRAPPER_PASSWD=/tmp/passwd NSS_WRAPPER_GROUP=/etc/group

RUN touch ${NSS_WRAPPER_PASSWD} && chgrp 0 ${NSS_WRAPPER_PASSWD} && chmod g+rw ${NSS_WRAPPER_PASSWD}

ADD passwd.template /passwd.template
ADD nss_wrapper.sh /nss_wrapper.sh

RUN adduser --gid 0 cki

USER cki

ENTRYPOINT ["/nss_wrapper.sh"]
