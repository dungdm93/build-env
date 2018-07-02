# Python docker image with native extensions (gcc)
FROM  python:2.7-stretch
LABEL maintainer="Dũng Đặng Minh <dungdm93@gmail.com>"

# NOTE: python-dev => python2.7-dev
RUN set -eux; \
    apt-get update; \
    apt-get install --no-install-recommends -y python2.7-dev; \
    pip install --no-cache-dir pipenv virtualenv virtualenvwrapper; \
    apt-get clean; rm -rf /var/lib/apt/lists/*;

RUN set -eux; \
    apt-get update; \
    # build-essential = gcc + g++ + make
    apt-get install --no-install-recommends -y \
        build-essential unixodbc-dev libgmp3-dev libxml2-dev libxslt1-dev; \
    apt-get clean; rm -rf /var/lib/apt/lists/*;

# NOTE: pre-install ultra-slow packages
RUN pip install --no-cache-dir lxml;

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/python-dev:2.7-debian .
# Run image:
# $ docker run -it --rm --name python-dev dungdm93/python-dev:2.7-debian bash
############################
