FROM node:8-alpine

RUN apk add --update --no-cache git \
                                libzmq \
                                zeromq-dev \
                                python \
                                make \
                                g++

WORKDIR /insight

# Copy lifetioncore-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/cointobanks/lifetioncore-node.git .

# Copy config file
COPY lifetioncore-node.json .

# Install NPM modules
RUN npm ci

ARG VERSION
ARG MAJOR_VERSION

# Install Insight modules
RUN bin/lifetioncore-node install lifetioncore-api@${MAJOR_VERSION}
RUN bin/lifetioncore-node install lifetioncore-ui@${VERSION}

FROM node:8-alpine

LABEL maintainer="Lifetioncoin Developers <support@cointobanks.com>"
LABEL description="Dockerised Insight-Lifetioncoin"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/lifetioncore-node", "start"]
