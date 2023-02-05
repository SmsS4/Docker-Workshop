FROM golang:1.17-alpine
WORKDIR /usr/src/goapp
COPY go.mod ./
COPY *.go ./
RUN go mod tidy
RUN go build -o /docker-go-sha
CMD [ "/docker-go-sha" ]