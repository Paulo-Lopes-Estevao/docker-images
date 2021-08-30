# Docker images mini scratch

## Golang

``` # golang

FROM golang as builder

WORKDIR /go/src/app

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

COPY . /go/src/app/

RUN go mod init

RUN go build -o execute

WORKDIR /go/src/app

FROM scratch

COPY --from=builder /go/src/app/execute ./execute

CMD [ "./execute" ]

```

<br>
<br>
<br>
<br>

## Next

``` # next

# Install dependencies only when needed

FROM node:14-alpine AS deps


RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

# Rebuild the source code only when needed
FROM node:14-alpine AS builder
WORKDIR /app
COPY . .
COPY --from=deps /app/node_modules ./node_modules
RUN yarn build

# Production image, copy all the files and run next
FROM node:14-alpine AS runner
WORKDIR /app

ENV NODE_ENV production

RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

# You only need to copy next.config.js if you are NOT using the default configuration
# COPY --from=builder /app/next.config.js ./
COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

USER nextjs

EXPOSE 3000


CMD ["yarn", "start"]

```

<br>
<br>
<br>
<br>

## React

``` # React
FROM node:12.18-alpine AS build

ENV HOME=/usr/srs/app
WORKDIR $HOME

COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --production --silent
RUN npm install react-scripts@^3.4.1 -g --silent

COPY . .

RUN  npm run build

FROM nginx:1.16.0-alpine

COPY --from=build /usr/srs/app/build /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d

EXPOSE 80

CMD ["nginx","-g", "daemon off;"]
```