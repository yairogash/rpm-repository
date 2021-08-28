ARG FROM_IMAGE='centos:8'
FROM $FROM_IMAGE

LABEL dockerfile_version="1.0.0"

RUN yum install -y nginx && \
  yum clean all && \
  mkdir -p /startup-hooks

COPY ./docker-entrypoint.sh /docker-entrypoint.sh
COPY ./healthcheck.sh /healthcheck.sh

# Create a self-signed certificate so the build doesn't blow up by default.
# If actually using SSL, a user will likely want to provide actual certificate
# files for the target deployment.
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
  ln -sf /dev/stderr /var/log/nginx/error.log

HEALTHCHECK \
  --interval=15s \
  --timeout=5s \
  --start-period=1m \
  --retries=2 \
  CMD /healthcheck.sh

EXPOSE 80
EXPOSE 443
STOPSIGNAL SIGQUIT

CMD [ "/docker-entrypoint.sh" ]