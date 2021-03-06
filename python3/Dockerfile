FROM    alpine:3.5

ENV     ZIPLINE_REF master

ENV     ZIPLINE_ROOT /zipline

ENV     ZIPLINE_USER zipline

ENV     ZIPLINE_GROUP zipline

RUN     apk add --no-cache --virtual .build-deps \
            build-base \
            ca-certificates \
            curl \
            gfortran \
            openblas-dev \
            python3-dev && \
        apk add --no-cache --virtual .hdf5-dev \
            --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
            hdf5-dev && \
        ln -v -s /usr/include/locale.h /usr/include/xlocale.h && \
        mkdir -v /src && \
        curl -s -L -o /src/zipline.tar.gz \
            https://github.com/quantopian/zipline/archive/$ZIPLINE_REF.tar.gz && \
        tar -C /src/ -vxzf /src/zipline.tar.gz && \
        rm -v /src/zipline.tar.gz && \
        mv -v /src/zipline-$ZIPLINE_REF /src/zipline && \
        pip3 install \
            --no-cache-dir \
            --no-compile \
            -r /src/zipline/etc/requirements.txt && \
        pip3 install \ 
            --no-cache-dir \
            --no-compile \
            --editable \
            /src/zipline && \
        addgroup $ZIPLINE_GROUP && \
        adduser -D -h $ZIPLINE_ROOT -s /sbin/nologin -G $ZIPLINE_GROUP \
            $ZIPLINE_USER && \
        chown -R $ZIPLINE_USER:$ZIPLINE_GROUP /src && \
        apk del --no-cache .build-deps .hdf5-dev && \
        apk add --no-cache \
            openblas \
            libstdc++ \
            python3 && \
        apk add --no-cache \
            --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
            hdf5

USER    $ZIPLINE_USER

WORKDIR /src/zipline

VOLUME  $ZIPLINE_ROOT

CMD     zipline
