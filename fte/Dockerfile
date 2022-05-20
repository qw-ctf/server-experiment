FROM debian:stable-slim AS builder

RUN apt-get -qq update && \
    apt-get -qq install --no-install-recommends unzip lhasa curl ca-certificates && \
    curl -L -O https://github.com/Jason2Brownlee/QuakeOfficialArchive/raw/main/bin/quake106.zip && \
    unzip quake106.zip && \
    lha x resource.1 id1/pak0.pak
ADD https://raw.githubusercontent.com/Jason2Brownlee/QuakeOfficialArchive/main/bin/3wctf301.zip /tmp/
ADD https://raw.githubusercontent.com/Jason2Brownlee/QuakeOfficialArchive/main/bin/3wctfc40.zip /tmp/

RUN cd /tmp && \
    unzip 3wctf301.zip pak0.pak && \
    unzip 3wctfc40.zip pak1.pak

FROM debian:stable-slim

COPY --from=builder /id1/pak0.pak /quake/id1/pak0.pak
COPY --from=builder /tmp/pak0.pak /quake/id1/pak2.pak
COPY --from=builder /tmp/pak1.pak /quake/id1/pak3.pak

ADD https://fte.triptohell.info/moodles/linux_amd64/fteqw64 /quake/fteqw

ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/v_coil.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/spawn.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/vwplayer.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_axe.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_shot.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_shot2.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_nail.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_nail2.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_rock.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_rock2.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_light.mdl /quake/id1/progs/
ADD https://github.com/rictorres/quake-ktx-server/raw/master/id1/progs/w_coil.mdl /quake/id1/progs/
ADD https://github.com/nQuake/distfiles/raw/dbe49ad4648c238261020dd2614b0596ae1ce1b4/sv-non-gpl/id1/progs/end2.mdl /quake/id1/progs/
ADD https://github.com/nQuake/distfiles/raw/dbe49ad4648c238261020dd2614b0596ae1ce1b4/sv-non-gpl/id1/progs/end3.mdl /quake/id1/progs/
ADD https://github.com/nQuake/distfiles/raw/dbe49ad4648c238261020dd2614b0596ae1ce1b4/sv-non-gpl/id1/progs/end4.mdl /quake/id1/progs/

RUN chmod 755 /quake/fteqw && \
    echo '#!/bin/bash\nset -x\n./fteqw +snd_device none +vid_renderer headless +name "${NAME:-missing}" +team "${TEAM:-missing}" +connect server $@ &>/dev/null' > /quake/fteqw.sh && \
    chmod 755 /quake/fteqw.sh

WORKDIR /quake

ENTRYPOINT ["./fteqw.sh"]
