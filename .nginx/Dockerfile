# Debian ベース（記事の選択）
FROM buildpack-deps:bullseye

# バージョン指定
ENV NGINX_VERSION=nginx-1.23.2
ENV NGINX_RTMP_MODULE_VERSION=1.2.2

# 必要な依存パッケージをインストール
RUN apt-get update && \
    apt-get install -y \
        ca-certificates \
        openssl \
        libssl-dev \
        build-essential \
        wget \
        ffmpeg \
        libpcre3-dev \
        zlib1g-dev \
        libxml2-dev \
        libxslt1-dev \
        git \
    && rm -rf /var/lib/apt/lists/*

# nginx ダウンロード
RUN mkdir -p /tmp/build/nginx && \
    cd /tmp/build/nginx && \
    wget https://nginx.org/download/${NGINX_VERSION}.tar.gz && \
    tar -zxf ${NGINX_VERSION}.tar.gz

# nginx-rtmp-module ダウンロード
RUN mkdir -p /tmp/build/nginx-rtmp-module && \
    cd /tmp/build/nginx-rtmp-module && \
    wget https://github.com/arut/nginx-rtmp-module/archive/refs/tags/v${NGINX_RTMP_MODULE_VERSION}.tar.gz && \
    tar -zxf v${NGINX_RTMP_MODULE_VERSION}.tar.gz

# nginx ビルド & インストール
RUN cd /tmp/build/nginx/${NGINX_VERSION} && \
    ./configure \
        --sbin-path=/usr/local/sbin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --pid-path=/var/run/nginx/nginx.pid \
        --with-http_ssl_module \
        --with-http_v2_module \
        --with-threads \
        --with-stream \
        --with-stream_ssl_module \
        --add-module=/tmp/build/nginx-rtmp-module/nginx-rtmp-module-${NGINX_RTMP_MODULE_VERSION} && \
    make -j $(nproc) && \
    make install && \
    rm -rf /tmp/build

# HLS 配信用ディレクトリ作成
RUN mkdir -p /mnt/hls

# nginx 設定ファイルをコピー
COPY nginx.conf /etc/nginx/nginx.conf

# ポートを公開
EXPOSE 1935 8080

# nginx 起動
CMD ["nginx", "-g", "daemon off;"]
