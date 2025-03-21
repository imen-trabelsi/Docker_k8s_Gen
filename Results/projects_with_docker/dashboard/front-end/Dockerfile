FROM node:16-alpine3.13 as base-node-modules

WORKDIR /usr/src/build

RUN apk update \
    && apk add openssl

COPY ./ ./
COPY ./${BUILD_ENV}.env ./.env

RUN yarn cache clean
RUN yarn add @coda-19/coda19-fhir-templates@1.0.6-main-7573265b.0
RUN yarn install
RUN rm -f .yarnrc
RUN rm -f .npmrc

RUN yarn build

FROM node:16-alpine3.13

RUN npm install -g http-server

RUN apk update \
    && apk add openssl

ARG BUILD_ENV=prod

WORKDIR /usr/src/app
COPY --from=base-node-modules /usr/src/build ./
COPY ./${BUILD_ENV}.env ./.env

# Make build footprint version for easier debugging.
RUN rm ./version.txt
RUN openssl rand -hex 12 > version.txt

EXPOSE 8080
CMD [ "http-server", "--port", "8080",  "dist" ]