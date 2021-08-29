# Docker images mini scratch

## Golang

``` # golang

FROM golang as builder

WORKDIR /go/src/examplo

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

COPY . /go/src/examplo/

RUN go mod init

RUN go build -o execute

WORKDIR /go/src/examplo

FROM scratch

COPY --from=builder /go/src/examplo/execute ./execute

CMD [ "./execute" ]

```