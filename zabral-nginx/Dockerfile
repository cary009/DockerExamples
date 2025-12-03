# Before starting, download Nginx, PCRE2, headers-more-nginx-module and zlib
# Nginx - https://nginx.org/en/download.html
# PCRE - https://www.pcre.org/
# headers-more-nginx-module - https://github.com/openresty/headers-more-nginx-module?tab=readme-ov-file
# Zlib project - https://zlib.net/

FROM nginx:1.29.3 AS build

RUN mkdir -p /sources
RUN apt update && apt install -y gcc make

# Change 1.29.3 for your nginx version
ADD nginx-1.29.3.tar.gz /sources

# Change 0.38 for your headers-more-nginx-module version 
ADD v0.38.tar.gz /sources

# Change 10.47 for your Perl Compatible Regular Expression (PCRE) version
ADD pcre2-10.47.tar.gz /sources

ADD zlib.tar.gz /sources

# Change 1.29.3 for your nginx version
WORKDIR /sources/nginx-1.29.3


# Change 0.38 for your headers-more-nginx-module version 
# Change 10.47 for your Perl Compatible Regular Expression (PCRE) version
# Change 1.3.1 for your zlib version
RUN ./configure --with-compat --add-dynamic-module=/sources/headers-more-nginx-module-0.38 --with-pcre=/sources/pcre2-10.47 --with-zlib=/sources/zlib-1.3.1 && \
    make modules && \
    cp objs/ngx_http_headers_more_filter_module.so /etc/nginx/modules

# Change 1.29.3 for your nginx version
FROM nginx:1.29.3
COPY --from=build /sources/nginx-1.29.3/objs/ngx_http_headers_more_filter_module.so /etc/nginx/modules
RUN sed -i 's:events {:load_module modules/ngx_http_headers_more_filter_module.so; \n\nevents {:g' /etc/nginx/nginx.conf
