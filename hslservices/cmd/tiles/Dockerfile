
ARG BUILDPATH='/build/hsl/cmd'

FROM golang:1.17-alpine as builder

ARG BUILDPATH

WORKDIR $BUILDPATH

# Copy code in && fetch dependencies
COPY . ./
RUN go mod tidy &&\
    go mod download

# Build the MQTT > Redis Connector (produces an image of ~1.02GB, 900GB of it from Go 1.16)
RUN cd ./cmd/tiles &&\
    CGO_ENABLED=1 go build -o tiles

# Create final image -  Just Export Go Binary onto regular alpine w. mqtt -> ~10MB
FROM alpine:latest
    
ARG BUILDPATH
WORKDIR $BUILDPATH

COPY --from=builder $BUILDPATH .

CMD ["./cmd/tiles/tiles"]
