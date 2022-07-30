FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential

COPY . /repo
WORKDIR /repo/resources
RUN gcc -o bin2c bin2c.c

FROM ubuntu:22.04 as package
COPY --from=builder /repo/resources/bin2c /
