FROM golang:1.19 as builder

WORKDIR /build

RUN apt update && \
    apt install -y protobuf-compiler && \
    go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.27.1 && \
    go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1.0

COPY cmd/emoji cmd/emoji
COPY go.mod .
COPY go.sum .
COPY Makefile .
ADD proto proto
RUN make emoji

FROM golang:1.19 as runner

WORKDIR /app

COPY --from=builder /build/cmd/emoji/bin/emoji /usr/local/bin/emoji

ENTRYPOINT [ "emoji" ]
