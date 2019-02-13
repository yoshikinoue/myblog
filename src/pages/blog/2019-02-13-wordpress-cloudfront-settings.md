---
templateKey: blog-post
title: WordPress cloudfront settings
date: 2019-02-13T18:47:08.095Z
description: WordPress cloudfront のセッティングで苦労した(Gutenbergエディタが動かないとかあった)のでメモ
tags:
  - WordPress
  - AWS
  - cloudfront
---

## Behavior

### Default*

- Viewer Protocol Policy
  - Redirect HTTP to HTTPS
- Allowed HTTP Methods
  - GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- Cache Based on Selected Request Headers
  - Whitelist
    - Authorization
    - CloudFront-Forwarded-Proto
    - Host
- Object Caching
  - Use Origin Cache Headers
- Forward Cookies
  - 
- Whitelist Cookies
  - wordpress_logged_in*
  - wp-settings*
- Query String Forwarding and Caching
  - Forward all, cache based on all

### /wp-json/*

- Viewer Protocol Policy
  - HTTP and HTTPS
- Allowed HTTP Methods
  - GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- Cache Based on Selected Request Headers
  - Whitelist
    - Authorization
    - CloudFront-Forwarded-Proto
    - Host
    - X-WP-Nonce
- Object Caching
  - Customize
    - Minimum TTL 0
    - Maximum TTL 0
    - Default TTL 0
- Forward Cookies
  - Whitelist
- Whitelist Cookies
  - wordpress_logged_in*
  - wp-settings*
- Query String Forwarding and Caching
  - Forward all, cache based on all

### .php

- Viewer Protocol Policy
  - HTTP and HTTPS
- Allowed HTTP Methods
  - GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- Cache Based on Selected Request Headers
  - None
- Object Caching
  - Customize
    - Minimum TTL 0
    - Maximum TTL 0
    - Default TTL 0
- Forward Cookies
  - ALL
- Query String Forwarding and Caching
  - Forward all, cache based on all

### /wp-admin/*

- Viewer Protocol Policy
  - HTTP and HTTPS
- Allowed HTTP Methods
  - GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- Cache Based on Selected Request Headers
  - Whitelist
    - Authorization
    - CloudFront-Forwarded-Proto
    - Host
    - User-Agent
- Object Caching
  - Customize
    - Minimum TTL 0
    - Maximum TTL 0
    - Default TTL 0
- Forward Cookies
  - ALL
- Query String Forwarding and Caching
  - Forward all, cache based on all


### WordPress

wp-config.phpの設定もしておく


```wp-config.php
define('WP_HOME','https://example.com');
define('WP_SITEURL','https://example.com');
$_SERVER['HTTPS'] = 'on';
$_ENV['HTTPS'] = 'on';
$_SERVER['HTTP_HOST'] = 'example.com';
$_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_X_FORWARDED_FOR'];
```
