FROM node:19-slim as builder

USER node
ENV HOME=/docker/build
WORKDIR $HOME
RUN apt-get -y update && \
  apt-get -y install curl && \
  rm -rf /var/lib/apt/lists/* && \
  mkdir -p $HOME && \
  chown -R node:node $HOME
COPY package* $HOME/
COPY tsconfig.json $HOME/
COPY --chown=node:node src $HOME/src
COPY .eslint* $HOME/
RUN npm ci && npm run build

FROM node:19-slim
ENV HOME=/docker/sample
WORKDIR $HOME
RUN apt-get -y update && \
  apt-get -y install curl && \
  rm -rf /var/lib/apt/lists/* && \
  mkdir -p $HOME && \
  chown -R node:node $HOME
COPY --chown=node:node package* $HOME/
COPY --chown=node:node --from=builder /docker/build/.npmrc $HOME/
COPY --chown=node:node --from=builder /docker/build/dist $HOME/dist

RUN npm ci
ENTRYPOINT ["npm"]
CMD ["start"]