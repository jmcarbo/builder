FROM alpine:3.13
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG TARGETARCH

ARG VERSION=0.1.7

LABEL name="Boundary" \
      maintainer="HashiCorp Boundary Team <boundary@hashicorp.com>, jmcarbo@gmail.com" \
      vendor="HashiCorp" \
      version=$VERSION \
      release=$VERSION \
      summary="Boundary provides simple and secure access to hosts and services" \
      description="The Boundary Docker image is designed to enable practitioners to run Boundary in server mode on a container scheduler"

RUN addgroup boundary && \
    adduser -s /bin/sh -S -G boundary boundary

RUN echo $TARGETPLATFORM >/build.txt
RUN echo $BUILDPLATFORM >>/build.txt
RUN echo $TARGETARCH >>/build.txt

RUN apk update && apk add unzip ca-certificates gnupg openssl libcap su-exec dumb-init tzdata
ADD https://releases.hashicorp.com/boundary/${VERSION}/boundary_${VERSION}_linux_${TARGETARCH}.zip /tmp/
RUN cd /tmp && unzip -d /bin boundary_${VERSION}_linux_${TARGETARCH}.zip

RUN mkdir /boundary/
ADD config.hcl /boundary/config.hcl
RUN chown -R boundary:boundary /boundary/ 

EXPOSE 9200 9201 9202
VOLUME /boundary/

USER boundary
ENTRYPOINT ["/bin/boundary"]
CMD ["server", "-config", "/boundary/config.hcl"]
