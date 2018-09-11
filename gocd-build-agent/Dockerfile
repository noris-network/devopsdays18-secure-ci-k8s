FROM alpine:3.7 AS buildstage
MAINTAINER Jonas Roeckl "jonas.roeckl@noris.de"
RUN apk --no-cache update && apk --no-cache upgrade
WORKDIR /workdir
ADD https://download.docker.com/linux/static/stable/x86_64/docker-17.12.1-ce.tgz .
RUN tar xvfo docker-17.12.1-ce.tgz


FROM gocd/gocd-agent-alpine-3.7:v18.5.0
MAINTAINER Jonas Roeckl "jonas.roeckl@noris.de"
RUN apk --no-cache update && apk --no-cache upgrade

# install needed packaged software
RUN apk --no-cache add python3 sudo

# install docker CLI from stage 0
COPY --chown=root:root --from=buildstage /workdir/docker/docker /usr/local/bin/docker

# install dockerw script
ADD --chown=root:root dockerw /usr/local/bin
ADD --chown=root:root dockerw.json /etc

# configure sudo
ADD --chown=root:root sudoers /etc
