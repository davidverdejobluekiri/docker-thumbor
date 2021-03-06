FROM python:2-alpine3.6

MAINTAINER David Verdejo <david.verdejo@bluekiri.com>

RUN echo -e '@edgunity http://nl.alpinelinux.org/alpine/edge/community\n\
@edge http://nl.alpinelinux.org/alpine/edge/main\n\
@testing http://nl.alpinelinux.org/alpine/edge/testing\n\
@community http://dl-cdn.alpinelinux.org/alpine/edge/community'\
  >> /etc/apk/repositories

RUN apk update && \
      apk upgrade && \
      apk add --no-cache \

      # --virtual .build-deps \
      build-base \
      openblas-dev \
      unzip \
      wget \
      cmake \

      #Intel® TBB, a widely used C++ template library for task parallelism'
      libtbb@testing \
      libtbb-dev@testing \

      # Wrapper for libjpeg-turbo
      libjpeg \

      # accelerated baseline JPEG compression and decompression library
      libjpeg-turbo-dev \

      # Portable Network Graphics library
      libpng-dev \

      # A software-based implementation of the codec specified in the emerging JPEG-2000 Part-1 standard (development files)
      jasper-dev \

      # Provides support for the Tag Image File Format or TIFF (development files)
      tiff-dev \

      # Libraries for working with WebP images (development files)
      libwebp-dev \

      # A C language family front-end for LLVM (development files)
      clang-dev \

      linux-headers && \
      
    # Install numpy
    pip install --use-wheel --no-cache-dir numpy && \

    #Remove cache apk
    rm -rf /var/cache/apk/* && \
    rm -rf /tmp/*

ENV CC /usr/bin/clang
ENV CXX /usr/bin/clang++
ENV OPENCV_VERSION 3.3.0

RUN mkdir /opt && cd /opt && \
    wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
    unzip ${OPENCV_VERSION}.zip && \
    rm -rf ${OPENCV_VERSION}.zip && \
    cd /opt/opencv-${OPENCV_VERSION} && \
    mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_FFMPEG=NO \
      -D WITH_IPP=NO -D WITH_OPENEXR=NO .. && \
    make VERBOSE=1 && \
    make && \
    make install && \
    rm -rf /opt/opencv-${OPENCV_VERSION}

RUN apk update && \
    apk del \
      build-base \
      unzip \
      wget \
      cmake \
      clang-dev \
      linux-headers
