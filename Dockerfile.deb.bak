FROM debian:11.4-slim

# Install software and dependencies
RUN apt-get update && apt-get install -y \
    vim \
    build-essential \
    wget \
    tar \
    libpcre3 \
    libpcre3-dev \
    zlib1g \
    zlib1g-dev \
    libssl-dev

# Download nginx
RUN wget http://nginx.org/download/nginx-1.23.1.tar.gz && tar -zxvf nginx-1.23.1.tar.gz

# Configure and install nginx
WORKDIR /nginx-1.23.1/
RUN ./configure \
    --sbin-path=/usr/bin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --with-pcre \
    --pid-path=/var/run/nginx.pid \
    --with-http_ssl_module \
    --with-http_v2_module \
    --modules-path=/etc/nginx/modules \
    --without-http_autoindex_module
RUN make && make install

WORKDIR /

# Start nginx
CMD [ "nginx", "-g", "daemon off;" ]
