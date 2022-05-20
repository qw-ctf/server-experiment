FROM debian:stable-slim AS builder

RUN apt-get -qq update && \
    apt-get -qq install --no-install-recommends git ca-certificates build-essential cmake curl unzip

RUN git clone https://github.com/QW-Group/mvdsv
RUN git clone https://github.com/QW-Group/ktx

RUN cd mvdsv && mkdir build && cd build && cmake .. && make -j 4
RUN cd ktx && mkdir build && cd build && cmake .. && make -j 4

ADD https://raw.githubusercontent.com/Jason2Brownlee/QuakeOfficialArchive/main/bin/3wctf301.zip /tmp/
ADD https://raw.githubusercontent.com/Jason2Brownlee/QuakeOfficialArchive/main/bin/3wctfc40.zip /tmp/

RUN cd /tmp && \
    unzip 3wctf301.zip pak0.pak && \
    unzip 3wctfc40.zip pak1.pak

FROM debian:stable-slim

ADD https://raw.githubusercontent.com/nQuake/distfiles/master/qsw106/ID1/PAK0.PAK /quake/id1/pak0.pak
COPY --from=builder /ktx/resources/example-configs/id1/maps/ctf /quake/id1/maps
COPY --from=builder /tmp/pak0.pak /quake/id1/pak2.pak
COPY --from=builder /tmp/pak1.pak /quake/id1/pak3.pak
COPY --from=builder /mvdsv/build/mvdsv /quake/
COPY --from=builder /ktx/build/qwprogs.so /quake/qw/qwprogs.so

RUN ls -lh /quake/id1/

WORKDIR /quake

EXPOSE 27500/tcp

CMD ./mvdsv +exec server.cfg -port 27500
