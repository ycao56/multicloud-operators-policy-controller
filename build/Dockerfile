FROM registry.access.redhat.com/ubi7/ubi-minimal:7.7-98

ENV OPERATOR=/usr/local/bin/trusted-container-policy-controller \
    USER_UID=1001 \
    USER_NAME=trusted-container-policy-controller

# install operator binary
COPY build/_output/bin/trusted-container-policy-controller ${OPERATOR}

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}
