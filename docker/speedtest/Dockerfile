FROM arm32v7/debian:10-slim AS get-speedtest

RUN apt-get update && apt-get install gnupg1 apt-transport-https dirmngr lsb-release -y
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 379CE192D401AB61
RUN echo "deb https://ookla.bintray.com/debian $(lsb_release -sc) main" | tee  /etc/apt/sources.list.d/speedtest.list
RUN apt-get update
RUN apt-get install speedtest

FROM node:12.18.3-buster as install-dependencies
WORKDIR /build
COPY . .
RUN npm ci

FROM arm32v7/node:12.18.3-buster as prod-stage

WORKDIR /usr/src/app

ENV INFLUXDB_DB="speedtest" \
    INFLUXDB_HOST="influxdb" \
    SPEEDTEST_HOST="local"

CMD [ "node", "index.js" ]

COPY --from=get-speedtest /usr/bin/speedtest /usr/bin/speedtest
COPY --from=install-dependencies /build .
