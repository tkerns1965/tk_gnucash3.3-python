FROM ubuntu/1804tz

ARG BASE_DIR=/docker
ARG SOURCE_DIR=${BASE_DIR}/gnucash-3.3
ARG BUILD_DIR=${BASE_DIR}/gnucash-3.3-BUILD
ARG INSTALL_DIR=/usr/local

ADD gnucash-3.3.tar.bz2 ${BASE_DIR}/.
COPY CMakeLists.txt ${SOURCE_DIR}/
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
        swig \
        python3-pip \
        python3-setuptools \
        python3-wheel && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir ${BUILD_DIR} && \
    cd ${BUILD_DIR} && \
    cmake -DCMAKE_INSTALL_PREFIX=${INSTALL_DIR} \
        -DWITH_GNUCASH=ON -DWITH_PYTHON=ON \
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

ARG GROUP_GID
ARG GROUP_NAME
ARG USER_UID
ARG USER_NAME

RUN groupadd --gid ${GROUP_GID} ${GROUP_NAME} && \
    useradd --gid ${GROUP_GID} --uid ${USER_UID} --create-home --shell /bin/bash ${USER_NAME}

ARG HOME_DIR=/home/${USER_NAME}

WORKDIR ${HOME_DIR}
