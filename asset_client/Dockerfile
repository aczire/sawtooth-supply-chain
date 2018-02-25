# Copyright 2018 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# -----------------------------------------------------------------------------

# NOTE: Use `volumes` to make: asset_client/public/
# available at: /usr/local/apache2/htdocs/

FROM httpd:2.4

RUN echo "\
\n\
ServerName asset_client\n\
AddDefaultCharset utf-8\n\
LoadModule proxy_module modules/mod_proxy.so\n\
LoadModule proxy_http_module modules/mod_proxy_http.so\n\
ProxyPass /api http://server:3000\n\
ProxyPassReverse /api http://server:3000\n\
\n\
<filesMatch \"\.(html|htm|js|css)$\">\n\
  FileETag None\n\
  <ifModule mod_headers.c>\n\
     Header unset ETag\n\
     Header set Cache-Control \"max-age=0, no-cache, no-store, must-revalidate\"\n\
     Header set Pragma \"no-cache\"\n\
     Header set Expires \"Wed, 11 Jan 1989 05:00:00 GMT\"\n\
  </ifModule>\n\
</filesMatch>\n\
\n\
\n\
LoadModule deflate_module modules/mod_deflate.so\n\
\n\
# Deflate configuration\n\
Include /usr/local/apache2/conf/httpd.deflate.conf\n\
\n\
\n\
" >>/usr/local/apache2/conf/httpd.conf

RUN echo "\
\n\
#Set to gzip all output\n\
SetOutputFilter DEFLATE\n\
\n\
#exclude the following file types\n\
SetEnvIfNoCase Request_URI \.(?:exe|t?gz|zip|iso|tar|bz2|sit|rar|png|jpg|gif|jpeg|flv|swf|mp3)$ no-gzip dont-vary\n\
\n\
#set compression level\n\
DeflateCompressionLevel 9\n\
\n\
#Handle browser specific compression requirements\n\
BrowserMatch ^Mozilla/4 gzip-only-text/html\n\
BrowserMatch ^Mozilla/4.0[678] no-gzip\n\
BrowserMatch bMSIE !no-gzip !gzip-only-text/html\n\
SetEnvIf User-Agent \".*MSIE.*\" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0\n\
\n\
\n\
" >>/usr/local/apache2/conf/httpd.deflate.conf

RUN cat /usr/local/apache2/conf/httpd.deflate.conf
RUN cat /usr/local/apache2/conf/httpd.conf

ENV PATH $PATH:/sawtooth-supply-chain/bin

EXPOSE 80/tcp
