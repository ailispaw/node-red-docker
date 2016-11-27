FROM armhf/alpine:3.5

RUN apk --no-cache add nodejs python py-rpigpio bash sudo

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN adduser -h /usr/src/node-red -D -H node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red \
    && echo "%node-red ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/node-red

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
RUN apk --no-cache add --virtual .build-deps build-base linux-headers && \
    su -l -c "npm install" node-red && \
    apk del .build-deps

USER node-red

# User configuration directory volume
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json

CMD ["npm", "start", "--", "--userDir", "/data"]
