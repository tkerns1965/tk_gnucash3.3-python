FROM ubuntu/1804tz

ARG ROOT=/root
ARG SOURCE_DIR=${ROOT}/gnucash-3.3
ARG BUILD_DIR=${ROOT}/gnucash-3.3-BUILD
ARG INSTALL_DIR=/usr/local

ADD gnucash-3.3.tar.bz2 ${ROOT}/.
COPY gnucash_core.* ${SOURCE_DIR}/bindings/python/
COPY gnc-budget.* ${SOURCE_DIR}/libgnucash/engine/
COPY qofbook.* ${SOURCE_DIR}/libgnucash/engine/

RUN apt update && \
    DEBIAN_FRONTEND=noninteractive \
    apt install -yq --no-install-recommends \
        build-essential \
        cmake \
        pkg-config \
        libglib2.0-dev \
        libxml2-dev \
        libxslt1-dev \
        libwebkit2gtk-4.0-dev \
        gettext \
        guile-2.2-dev \
        xsltproc \
        libpython3-dev \
        libsecret-1-dev \
        libboost-dev \
        libboost-date-time-dev \
        libboost-filesystem-dev \
        libboost-locale-dev \
        libboost-regex-dev \
        libboost-system-dev \
        libgtest-dev \
        google-mock \
        libxml2-utils \
        python3-pip \
        python3-setuptools \
        python3-wheel && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir ${BUILD_DIR} && \
    cd ${BUILD_DIR} && \
    cmake -DCMAKE_INSTALL_PREFIX=${INSTALL_DIR} \
        -DWITH_GNUCASH=OFF -DWITH_PYTHON=ON \
        -DWITH_AQBANKING=OFF -DWITH_OFX=OFF -DWITH_SQL=OFF \
        ${SOURCE_DIR} && \
    make && \
    make install && \
    apt remove -yq --autoremove \
        build-essential \
        cmake && \
    python3 -m pip install \
        fpdf && \
    rm -rf ${SOURCE_DIR}

ENV PYTHONPATH=/usr/local/lib/python3/dist-packages
