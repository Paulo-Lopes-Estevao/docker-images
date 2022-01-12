## Node

[# N0](https://github.com)

``` dockerfile

FROM node
WORKDIR /usr/src/app
COPY . /usr/src/app
RUN npm install
CMD "npm" "start"

```

<br>
<br>

[# N1](https://github.com)

``` dockerfile

FROM node
ENV NODE_ENV=production

WORKDIR /app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install --production

COPY . .

CMD [ "npm", "start" ]

```

<br>
<br>

[# N2](https://github.com)

``` dockerfile

FROM node
ENV NODE_ENV production
WORKDIR /usr/src/app
COPY --chown=node:node . /usr/src/app
RUN npm ci --only=production
USER node
CMD ["npm" "start"]

```

<br>
<br>

[# N3](https://github.com)

``` dockerfile

RUN apk add dumb-init
ENV NODE_ENV production

WORKDIR /usr/src/app
COPY --chown=node:node . .
RUN npm ci --only=production

USER node
CMD ["dumb-init", "node", "server.js"]

```

<br>
<br>