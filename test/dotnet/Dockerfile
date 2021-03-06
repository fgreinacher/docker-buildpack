ARG IMAGE=renovate/buildpack
FROM ${IMAGE} as base

RUN touch /.dummy

WORKDIR /tmp

COPY --chown=1000:0 test test


#--------------------------------------
# net3: dotnet 3.1 base image
#--------------------------------------
FROM base as net3

# renovate: datasource=docker lookupName=mcr.microsoft.com/dotnet/sdk versioning=docker
RUN install-tool dotnet 3.1.407

USER 1000

RUN set -ex; \
    dotnet --info


#--------------------------------------
# test: dotnet 3.1
#--------------------------------------
FROM net3 as testa

RUN set -ex; \
    cd test; \
    dotnet restore --use-lock-file

RUN set -ex;  \
    cd test; \
    dotnet add package Newtonsoft.Json --version 12.0.3; \
    dotnet restore --force-evaluate


#--------------------------------------
# test: dotnet 5.0
#--------------------------------------
FROM base as testb

# renovate: datasource=docker lookupName=mcr.microsoft.com/dotnet/sdk versioning=docker
RUN install-tool dotnet 5.0.201

USER 1000

RUN set -ex; \
    cd test; \
    dotnet restore --use-lock-file

RUN set -ex;  \
    cd test; \
    dotnet add package Newtonsoft.Json --version 12.0.3; \
    dotnet restore --force-evaluate


#--------------------------------------
# final
#--------------------------------------
FROM base

COPY --from=testa /.dummy /.dummy
COPY --from=testb /.dummy /.dummy
