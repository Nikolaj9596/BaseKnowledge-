# Exampel build multi stage image

```docker
FROM going:1.11-alpine AS build

RUN apk add --no-cache git
RUN go get github.com/golang/dap/cmd/dep


COPY Gopkg.lock Gopkg.tml /go/src/project/
WORKDIR /go/src/project/

RUN dap ensure -vandor-only

COPY . /go/src/project/
RUN go build -o /bin/project

FROM scratch
COPY --from=build /bin/project /bin project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```
