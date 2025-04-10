FROM --platform=linux/amd64 golang:1.15.2-alpine3.12
WORKDIR /client
RUN apk add git make build-base
ENV CGO_ENABLED=1 \
    GOOS=linux \
    GOARCH=amd64
RUN go get github.com/lucas-clemente/quic-go/http3
ADD src /client/src

WORKDIR /client/src
RUN go build -o run

FROM --platform=linux/amd64 frankpetrilli/boringtun:latest
WORKDIR /etc/wg_client/
COPY --from=0 /client/src/run .

RUN apt-get update && apt-get install -y wget git curl iputils-ping tcpdump && wget https://go.dev/dl/go1.24.2.linux-amd64.tar.gz && rm -rf /usr/local/go && tar -C /usr/local -xzf go1.24.2.linux-amd64.tar.gz && echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
ENV PATH="/usr/local/go/bin:${PATH}"

WORKDIR /etc/wireguard/
