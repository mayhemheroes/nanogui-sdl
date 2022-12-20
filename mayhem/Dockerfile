FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential clang cmake libsdl2-dev libsdl2-image-dev libsdl2-ttf-dev

WORKDIR /repo/build
COPY . /repo
WORKDIR /repo/resources
RUN gcc -o bin2c bin2c.c

RUN clang++ /repo/mayhem/fuzz_utf8.cpp -fsanitize=fuzzer,address /repo/sdlgui/*.cpp -I /repo/sdlgui/ /repo/sdlgui/*.c -lSDL2main -lSDL2 -lSDL2_image -lSDL2_ttf -I /repo -o /fuzz

FROM ubuntu:22.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libsdl2-dev libsdl2-image-dev libsdl2-ttf-dev
COPY --from=builder /repo/resources/bin2c /
COPY --from=builder /fuzz /fuzz_utf8
