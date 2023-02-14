ARG REPO_NAME="sample"
ARG APP_NAME="sample"

FROM golang:1.20 as builder
ARG REPO_NAME
ARG APP_NAME
COPY . /go/src/${REPO_NAME}
WORKDIR /go/src/${REPO_NAME}
RUN go build -o /go/bin/${APP_NAME} ./

FROM gcr.io/distroless/base
ARG APP_NAME
COPY --from=builder /go/bin/${APP_NAME} /${APP_NAME}
CMD ["/sample"]
