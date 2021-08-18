# Ref: https://docs.docker.com/language/golang/build-images/#multi-stage-builds

##
## Build
##

FROM golang:1.16-buster AS build
WORKDIR /app
COPY go.mod ./
RUN go mod download
COPY *.go ./
RUN go build -o /go-web-app

##
## Deploy
##

FROM gcr.io/distroless/base-debian10
WORKDIR /
COPY --from=build /go-web-app /go-web-app
EXPOSE 8080
USER nonroot:nonroot
ENTRYPOINT ["/go-web-app"]