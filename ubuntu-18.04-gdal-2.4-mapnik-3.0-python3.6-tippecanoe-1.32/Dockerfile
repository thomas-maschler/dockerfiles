FROM geographica/gdal2:latest
MAINTAINER Thomas Maschler thomas.maschler@wri.org

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update && apt-get -y install curl git g++ gcc \
    libfreetype6-dev libglib2.0-dev libcairo2-dev \
    libboost-all-dev libicu-dev libxml2 libfreetype6 \
    libfreetype6-dev libjpeg-dev libpng-dev libproj-dev libtiff-dev \
    libcairo2 libcairo2-dev python-cairo python-cairo-dev \
    ttf-unifont ttf-dejavu ttf-dejavu-core ttf-dejavu-extra build-essential \
    libsqlite3-dev python-nose python3-setuptools python3-pip python3-wheel python3-venv \
    postgresql-10 postgresql-server-dev-10 postgresql-contrib-10 postgresql-10-postgis-2.4

# install Harfbuzz
RUN curl https://www.freedesktop.org/software/harfbuzz/release/harfbuzz-2.3.0.tar.bz2 | tar -xj && \
    cd harfbuzz-2.3.0 && ./configure && make && make install && ldconfig

# install mapnik
RUN apt-get install -y wget
ENV MAPNIK_VERSION 3.0.21
RUN wget https://github.com/mapnik/mapnik/releases/download/v${MAPNIK_VERSION}/mapnik-v${MAPNIK_VERSION}.tar.bz2 -P /tmp/
RUN tar xvjf /tmp/mapnik-v${MAPNIK_VERSION}.tar.bz2 -C /tmp/
RUN cd /tmp/mapnik-v${MAPNIK_VERSION} && python scons/scons.py configure
RUN cd /tmp/mapnik-v${MAPNIK_VERSION} && make JOBS=4 && make install JOBS=4

# Mapnik Python Bindings
RUN mkdir -p /opt/python-mapnik && \
    curl -L https://github.com/mapnik/python-mapnik/tarball/v3.0.x | tar xz -C /opt/python-mapnik --strip-components=1
RUN cd /opt/python-mapnik && python3 setup.py install && rm -r /opt/python-mapnik/build

# Tippecanoe
ENV TIPPECANOE_VERSION 1.32.10
RUN wget https://github.com/mapbox/tippecanoe/archive/${TIPPECANOE_VERSION}.tar.gz  -P /tmp/
RUN tar xvzf /tmp/${TIPPECANOE_VERSION}.tar.gz -C /tmp/ && \
    cd /tmp/tippecanoe-${TIPPECANOE_VERSION} && \
    make -j && \
    make install