FROM golang:bookworm AS build

ARG GIT_CI_REF=v2.4.*
ARG GIT_BRANCH=release-2.4

RUN git clone -b "$GIT_BRANCH" --single-branch https://github.com/zgoat/goatcounter.git
RUN cd goatcounter \
  && git checkout $(git describe --match "$GIT_CI_REF" --abbrev=0 --tags HEAD) \
  && go build ./cmd/goatcounter

FROM debian:bookworm-slim

WORKDIR /goatcounter

ENV GOATCOUNTER_LISTEN '0.0.0.0:8080'
ENV GOATCOUNTER_DB 'sqlite+/goatcounter/db/goatcounter.sqlite3'
ENV GOATCOUNTER_SMTP ''

RUN apt-get update \
  && apt-get install -y ca-certificates \
  && update-ca-certificates --fresh

COPY --from=build /go/goatcounter/goatcounter /usr/bin/goatcounter
COPY goatcounter.sh ./
COPY entrypoint.sh /entrypoint.sh

VOLUME ["/goatcounter/db"]
EXPOSE 8080

ENTRYPOINT ["/entrypoint.sh"]
CMD ["/goatcounter/goatcounter.sh"]
