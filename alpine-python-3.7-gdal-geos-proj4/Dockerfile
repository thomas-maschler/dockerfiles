FROM python:3.7-alpine
MAINTAINER Thomas Maschler thomas.maschler@wri.org

ENV GEOS http://download.osgeo.org/geos/geos-3.6.3.tar.bz2
ENV PROJ4 http://download.osgeo.org/proj/proj-5.1.0.tar.gz
ENV GDAL http://download.osgeo.org/gdal/2.3.2/gdal-2.3.2.tar.gz

RUN apk update && apk upgrade && \
   apk add --no-cache --update bash git libressl-dev build-base alpine-sdk \
   libffi-dev gcc python3-dev postgresql-dev musl-dev linux-headers\
    && mkdir -p /usr/src \
    && curl -SL $GEOS | tar -xjC /usr/src \
    && curl -SL $PROJ4 | tar -xzC /usr/src \
    && curl -SL $GDAL | tar -xzC /usr/src

WORKDIR /usr/src/geos-3.6.3
RUN ./configure --enable-python \
    && make \
    && make install
RUN ldconfig /usr/src/geos-3.6.3
RUN geos-config --cflags

WORKDIR /usr/src/proj-5.1.0
RUN ./configure --enable-python \
    && make \
    && make install \
    && export PROJ_DIR=/usr/local/lib/

WORKDIR /usr/src/gdal-2.3.2
RUN ./configure \
    --with-geos \
    --with-geotiff=internal \
    --with-hide-internal-symbols \
    --with-libtiff=internal \
    --with-libz=internal \
    --with-threads \
    --without-bsb \
    --without-cfitsio \
    --without-cryptopp \
    --without-curl \
    --without-dwgdirect \
    --without-ecw \
    --without-expat \
    --without-fme \
    --without-freexl \
    --without-gif \
    --without-gif \
    --without-gnm \
    --without-grass \
    --without-grib \
    --without-hdf4 \
    --without-hdf5 \
    --without-idb \
    --without-ingres \
    --without-jasper \
    --without-jp2mrsid \
    --without-jpeg \
    --without-kakadu \
    --without-libgrass \
    --without-libkml \
    --without-libtool \
    --without-mrf \
    --without-mrsid \
    --without-mysql \
    --without-netcdf \
    --without-odbc \
    --without-ogdi \
    --without-openjpeg \
    --without-pcidsk \
    --without-pcraster \
    --without-pcre \
    --without-perl \
    --without-php \
    --without-png \
    --without-qhull \
    --without-sde \
    --without-sqlite3 \
    --without-webp \
    --without-xerces \
    --without-xml2 \
    && make \
    && make install
RUN ldconfig /usr/src/gdal-2.3.2
RUN export GDAL_LIBRARY_PATH=/usr/local/lib/libgdal.so

RUN easy_install pip \
    && pip install --upgrade pip \
    && pip install -U pip setuptools \
    && pip install cython gevent numpy gdal==2.3.2\
    # pulling pyproj directly from github because of
    # https://github.com/jswhit/pyproj/issues/136
    && pip install git+https://github.com/jswhit/pyproj.git