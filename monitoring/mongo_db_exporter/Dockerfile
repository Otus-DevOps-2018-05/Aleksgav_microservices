FROM golang:latest as builder

ENV SRC $GOPATH/src/github.com/percona/mongodb_exporter

WORKDIR $SRC

RUN git clone https://github.com/percona/mongodb_exporter.git $SRC && make build


FROM alpine:latest

COPY --from=builder /go/src/github.com/percona/mongodb_exporter/mongodb_exporter /bin/mongodb_exporter

EXPOSE 9216

ENTRYPOINT ["/bin/mongodb_exporter"]
