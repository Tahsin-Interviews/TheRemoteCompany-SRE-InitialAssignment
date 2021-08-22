### To run with `go` 

```shell
go run .
```

### Build Application and running the binary 

```shell
go build
```

It will generate a binary named `Go-Web-App-Container`. You can now run the app with

```shell
./Go-Web-App-Container 
```

### Build and run the app with `docker`

```shell
docker build -t "rafaftahsin/go-web-app:master" .
docker run -it -p 8080:8080 "rafaftahsin/go-web-app:master"
```

### Run with dockerhub image

```shell
docker rmi rafaftahsin/go-web-app:master
docker pull rafaftahsin/go-web-app:master
docker run -it -p 8080:8080 "rafaftahsin/go-web-app:master"
```

### CI is configured with Github Action

Any changes inside Go-Web-App-Container directory triggers a CI build and an updated image is pushed to docker hub with a branch tag.

