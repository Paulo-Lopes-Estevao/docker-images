## Golang

[# N1](https://github.com)

``` dockerfile

FROM golang as builder

WORKDIR /go/src/app

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

COPY . /go/src/app/

RUN go mod init app

RUN go build -o execute

WORKDIR /go/src/app

FROM scratch

COPY --from=builder /go/src/app/execute ./execute

CMD [ "./execute" ]

```
