## Golang

[# N0](https://github.com)

``` dockerfile

FROM golang

WORKDIR /app

COPY go.mod ./
COPY go.sum ./
RUN go mod tidy

COPY . ./

RUN go build -o /main

EXPOSE 8080

CMD [ "/main" ]

```


<br>
<br>

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


<br>
<br>

[# N2](https://github.com)

``` dockerfile

FROM golang as builder

WORKDIR /go/src/app

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

COPY . /go/src/app/


RUN go build -o main

WORKDIR /go/src/app

FROM scratch

COPY --from=builder /go/src/app/ ./main

CMD [ "./main"]

```
<br>
<br>

[# N3](https://github.com)

``` dockerfile

FROM golang as builder

WORKDIR /go/src/app

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

RUN go mod init app

COPY . /go/src/app/

RUN go build -o main

EXPOSE 9000

RUN chmod +x ./main

CMD ["./main"]

```

<br>
<br>

[# N4](https://github.com)

``` dockerfile

FROM golang as builder

WORKDIR /go/src/app

ENV GOOS=linux
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOARCH=amd64

RUN go mod init app

COPY . /go/src/app/

RUN go build -o main

EXPOSE 9000

RUN chmod +x ./main

CMD ["./main"]

```