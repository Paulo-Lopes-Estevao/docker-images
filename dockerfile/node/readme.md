## Node

[# N0](https://github.com)

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