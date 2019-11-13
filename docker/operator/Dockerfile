FROM golang:1.11-alpine3.8 AS builder
WORKDIR $GOPATH/src/github.com/nats-io/nats-operator/
RUN apk add --update git
RUN go get -u github.com/golang/dep/cmd/dep
COPY . .
RUN dep ensure -v -vendor-only
RUN CGO_ENABLED=0 GOARCH=arm GOARM=7  go build -ldflags "-X github.com/nats-io/nats-operator/version.GitSHA=d9d9559ac9972e466b36118d1fefb15da7ed180a" -installsuffix cgo -o /nats-operator ./cmd/operator/main.go

FROM arm32v7/alpine:3.10.3
COPY --from=builder /nats-operator /usr/local/bin/nats-operator
CMD ["nats-operator"]
