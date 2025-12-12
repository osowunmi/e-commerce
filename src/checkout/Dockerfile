### STAGE 1 ###
###############

FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app

# Use Go build cache for dependencies
RUN  --mount=type=cache,target=/go/pkg/mod \
     --mount=type=cache,target=/root/.cache/go-build \
     mkdir -p /root/.cache/go-build

COPY go.mod go.sum ./

RUN  go mod download

COPY . .
RUN go build -o /go/bin/checkout/ ./

### STAGE 2 ###

FROM alpine AS release

WORKDIR /usr/src/app/

COPY --from=builder /go/bin/checkout/ ./

ENV CHECKOUT_PORT 8081

ENTRYPOINT [ "./checkout" ]

