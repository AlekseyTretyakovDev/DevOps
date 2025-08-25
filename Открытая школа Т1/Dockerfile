FROM golang:1.23-alpine AS base
RUN apk add --no-cache git upx

ENV CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -ldflags="-s -w -extldflags=-static" -trimpath -o pig .
RUN upx --best --lzma pig

FROM scratch
WORKDIR /app
COPY --from=base /app/pig /app
COPY --from=base /app/resources /app/resources
EXPOSE 8000
CMD [ "/app/pig" ]