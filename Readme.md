# Docker images mini scratch

## Golang
``` # golang

FROM golang as builder

WORKDIR /go/src/ussd

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

COPY . /go/src/ussd/

RUN go mod init

RUN go build -o execute

WORKDIR /go/src/ussd

FROM scratch

COPY --from=builder /go/src/ussd/execute ./execute

CMD [ "./execute" ]

```