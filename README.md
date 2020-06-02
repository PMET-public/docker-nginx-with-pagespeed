# docker-nginx-with-pagespeed

Using the [offical nginx](https://hub.docker.com/_/nginx) as a baseimage, this image adds a few useful features/optimizations:

* [Pagespeed](https://www.modpagespeed.com/doc/build_ngx_pagespeed_from_source) to act as a CDN / web optimizer
* a self-signed cert and insecure private key for testing @ `/etc/ssl/certs/self.[key,crt]`
* a preconfigured reverse proxy template that interpolates a `PROXY_PASS` env var
* multi-stage build process to keep the image slim

Examples:

1. A secure, optimized reverse proxy using pagespeed to another docker service

```
docker run -e PROXY_PASS=http://host.docker.internal:3000 -p 443:443 pmetpublic/nginx-with-pagespeed
```

2. A simple, optimized web server using pagespeed with your own conf (same options as the offical `nginx`)
```
docker run -v /your-nginx.conf:/etc/nginx/conf.d/your-nginx.conf \
  -v /your-certs:/etc/ssl/certs \
  -p 443:443 \
  pmetpublic/nginx-with-pagespeed
```