### STAGE 1 ###
###############

FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,source=./src/checkout/go.sum,target=go.sum \
    --mount=type=bind,source=./src/checkout/go.mod,target=do.mod \
    go mod download 

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=cache,target=/root/.cache/go-build \
    --mount=type=bind,rw,source=./src/checkout,target=. \
    go build -ldflag "-s -w" -o /go/bin/checkout/ ./

### STAGE 2 ###

FROM alpine AS release

WORKDIR /usr/src/app/

COPY --from=builder /go/bin/checkout/ ./

EXPOSE ${CHECKOUT_PORT}

ENTRYPOINT [ "./checkout" ]
