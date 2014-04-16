Here's a script for building OpenResty locally:
```bash
mkdir openresty
cd openresty
curl -O -L http://downloads.sourceforge.net/sourceforge/pcre/pcre-8.34.tar.gz
curl -O -L http://openresty.org/download/ngx_openresty-1.5.11.1.tar.gz
tar xzf pcre-8.34.tar.gz
tar xzf ngx_openresty-1.5.11.1.tar.gz
cd ngx_openresty-1.5.11.1
export PATH=/sbin:$PATH
./configure --prefix=/usr/local/openresty --with-pcre=`pwd`/../pcre-8.34 --with-luajit --with-http_postgres_module --with-http_realip_module --with-http_xslt_module --with-http_gzip_static_module --with-http_degradation_module --with-http_stub_status_module --with-mail --with-mail_ssl_module --with-pcre-jit --with-http_iconv_module -j2
make
make install
sudo ln -s /usr/local/openresty/nginx/sbin/nginx /usr/bin/nginx
```
