ARG IMAGE=renovate/buildpack
FROM ${IMAGE} as build

# renovate: datasource=docker versioning=docker
RUN install-tool swift 5.3.3

RUN touch /.dummy

WORKDIR /tmp


#--------------------------------------
# testa: swift 5.3
#--------------------------------------
FROM build as testa

USER 1000

RUN swift --version

COPY --chown=1000:0 test test

RUN set -ex; \
  cd test/a; \
  swift package resolve

RUN set -ex; \
  cd test/b; \
  swift package resolve

RUN set -ex; \
  cd test/c; \
  swift package resolve


#--------------------------------------
# testb: swift 5.2.5
#--------------------------------------
FROM build as testb

# renovate: datasource=docker versioning=docker
RUN install-tool swift 5.3.3

USER 1000

RUN swift --version

COPY --chown=1000:0 test test


RUN set -ex; \
  cd test/c; \
  swift package resolve

#--------------------------------------
# final
#--------------------------------------
FROM build

COPY --from=testa /.dummy /.dummy
COPY --from=testb /.dummy /.dummy
