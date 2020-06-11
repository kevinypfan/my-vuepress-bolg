---
category: memo
tags:
  - nginx
date: 2019-11-01
title: Nginx Configuration
lang: zh-Hant-TW
vssue-id: 2
---

關於 Nginx 相關設定，目前只有 Location 相關，未來會持續更新

![](https://img.shields.io/github/license/meteorlxy/vuepress-theme-meteorlxy.svg?style=flat)

<!-- more -->

### Location

::: tip
解析順序

1.  Exact match `= url`
2.  Preferential Prefix match `^~ url`
3.  REGEX match `~ regex_url` or `~* regex_url`
4.  Prefix match `url`

:::

Prefix match

```conf
location /Greet2 {
    return 200 'Hello from NGINX "/greet" location.';
}
```

Preferential Prefix match

```conf
location ^~ /Greet2 {
    return 200 'Hello from NGINX "/greet" location.';
}
```

Exact match

```conf
location = /greet {
    return 200 'Hello from NGINX "/greet" location - EXACT MATCH.';
}
```

REGEX match - case sensitive

```conf
location ~ /greet[0-9] {
    return 200 'Hello from NGINX "/greet" location - REGEX MATCH.';
}
```

REGEX match - case insensitive

```conf
location ~* /greet[0-9] {
    return 200 'Hello from NGINX "/greet" location - REGEX MATCH INSENSITIVE.';
}
```
