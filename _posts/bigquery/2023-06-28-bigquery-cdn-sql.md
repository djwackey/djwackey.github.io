---
layout: post
title: BigQuery常用的CDN SQL查询语句
author: djwackey
tags: [CDN, Bigquery]
categories: cdn
excerpt_separator: <!--more-->
---

## 一、概念

参考链接：<a href="https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql">https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql</a>

## 二、目标数据表

my-project.nginx_cache.nginx_access_20210915

目标数据表的组成格式：项目ID.数据集.数据表
<!--more-->

## 三、日志格式（SCHEMA）

参考链接：<a href="https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#HttpRequest">https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#HttpRequest</a>

## 四、常用SQL查询语句

### 4.1、CDN质量和性能分析

#### 4.1.1、健康度

```sql
SELECT
  SUM(flag) * 100.0 / COUNT(*) AS health_ratio
FROM (
  SELECT
    CASE
      WHEN httpRequest.status < 500 THEN 1
    ELSE
    0
  END
    AS flag
  FROM
    `my-project.nginx_cache.nginx_access_20210915` )
```

#### 4.1.2、缓存命中率

```sql
SELECT
  SUM(flag) * 100.0 / COUNT(*) AS hit_ratio
FROM (
  SELECT
    CASE
      WHEN httpRequest.cacheHit THEN 1
    ELSE
    0
  END
    AS flag
  FROM
    `my-project.nginx_cache.nginx_access_20210915` )
```

#### 4.1.3、访问次数Top域名

```sql
SELECT
  NET.REG_DOMAIN(httpRequest.requestUrl) AS domain,
  COUNT(*) AS count
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY count DESC
LIMIT 10
```

#### 4.1.4、下载流量Top域名

```sql
SELECT
  NET.REG_DOMAIN(httpRequest.requestUrl) AS domain,
  SUM(httpRequest.responseSize) AS totalResponseSize
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY totalResponseSize DESC
LIMIT 10
```

#### 4.1.5、请求响应延迟

```sql
SELECT
  CASE
    WHEN httpRequest.latency < 50 THEN '~50ms'
    WHEN httpRequest.latency < 100 THEN '50~100ms'
    WHEN httpRequest.latency < 200 THEN '100~200ms'
    WHEN httpRequest.latency < 500 THEN '200~500ms'
    WHEN httpRequest.latency < 5000 THEN '500~5000ms'
  ELSE
      '5000ms~'
  END
      AS latency,
  COUNT(*) AS count
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY
  latency DESC
```

### 4.2、CDN错误诊断

#### 4.2.1、4xx、5xx错误百分比和分布

```sql
-- CASE 1:
SELECT
  httpRequest.status AS status_code,
  COUNT(*) AS count
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY
  count DESC
  
-- CASE 2:
SELECT
  CASE
    WHEN httpRequest.status <= 200 THEN "1xx"
    WHEN httpRequest.status < 300 THEN "2xx"
    WHEN httpRequest.status < 400 THEN "3xx"
    WHEN httpRequest.status < 500 THEN "4xx"
  ELSE
  "0"
END
  AS status_code,
  COUNT(*) AS count
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY
  count DESC
```

#### 4.2.2、错误域名访问TOP10

```sql
SELECT
  NET.REG_DOMAIN(httpRequest.requestUrl) AS domain,
  COUNT(*) AS count
FROM
  `my-project.nginx_cache.nginx_access_20210915`
WHERE
  httpRequest.status > 400
GROUP BY
  1
ORDER BY count DESC
LIMIT 10
```

#### 4.2.3、客户端分布

```sql
SELECT
  httpRequest.userAgent AS userAgent,
  COUNT(*) AS count
FROM
  `my-project.nginx_cache.nginx_access_20210915`
WHERE
  httpRequest.status > 400
GROUP BY
  1
ORDER BY
  count DESC
LIMIT
  10
```

### 4.3、流量数据统计（以5分钟为单位）

```sql
SELECT
  TIMESTAMP_TRUNC(TIMESTAMP_SUB(timestamp, INTERVAL MOD(EXTRACT(MINUTE
        FROM
          timestamp), 5) MINUTE),MINUTE) AS time,
  SUM(httpRequest.responseSize) AS traffic,
  "byte" AS unit
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY
  time ASC
  
-- 参考文档：https://titanwolf.org/Network/Articles/Article?AID=c64f3be8-9424-40be-9f75-2e4c3a9ce7a9#gsc.tab=0
-- MOD (EXTRACT (MINUTE from my_timestamp), 5) → pull out the minute, number divided by 5
-- TIMESTAMP_SUB(my_timestamp, INTERVAL N MINUTE) ￫ $ {N} minutes ago. Become "the number of too divided by 5 minutes" minutes before the TIMESTAMP In other words, in this
-- TIMESTAMP_TRUNC( TIMESTAMP , MINUTE) ￫ rounded to the minute
```

### 4.4、带宽数据统计（以5分钟为单位）

```sql
SELECT
  TIMESTAMP_TRUNC(TIMESTAMP_SUB(timestamp, INTERVAL MOD(EXTRACT(MINUTE
        FROM
          timestamp), 5) MINUTE),MINUTE) AS time,
  SUM(httpRequest.responseSize) / 300 * 8 / 1024 / 1024 / 1024 AS bandwidth,
  "Gbps" AS unit
FROM
  `my-project.nginx_cache.nginx_access_20210915`
WHERE
    NET.REG_DOMAIN(httpRequest.requestUrl) = 'cdn.mydomain.cn'
GROUP BY
  1
ORDER BY
  time ASC
```

### 4.5、回源带宽统计（以5分钟为单位）

```sql
SELECT
  TIMESTAMP_TRUNC(TIMESTAMP_SUB(timestamp, INTERVAL MOD(EXTRACT(MINUTE
        FROM
          timestamp), 5) MINUTE),MINUTE) AS time,
  SUM(httpRequest.responseSize) / 300 * 8 / 1024 / 1024 / 1024 AS bandwidth,
  "Gbps" AS unit
FROM
  `my-project.nginx_cache.nginx_access_20210915`
WHERE
    NET.REG_DOMAIN(httpRequest.requestUrl) = 'cdn.mydomain.cn'
    AND httpRequest.cacheHit != true
    AND httpRequest.cacheLookup = true  -- or jsonPayload.statusDetails = "response_sent_by_backend"
GROUP BY
  1
ORDER BY
  time ASC

-- 参考文档：https://cloud.google.com/cdn/docs/logging#what_is_logged
```

### 4.6、请求数统计（以5分钟为单位）

```sql
SELECT
  TIMESTAMP_TRUNC(TIMESTAMP_SUB(timestamp, INTERVAL MOD(EXTRACT(MINUTE
        FROM
          timestamp), 5) MINUTE),MINUTE) AS time,
  COUNT(*)
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY
  time ASC
```

### 4.7、平均下载速度（以5分钟为单位）

```sql
SELECT
  time,
  CASE
    WHEN request_time != 0 THEN response_size / request_time
  ELSE
  0
END
  AS download_speed
FROM (
  SELECT
    TIMESTAMP_TRUNC(TIMESTAMP_SUB(timestamp, INTERVAL MOD(EXTRACT(MINUTE
          FROM
            timestamp), 5) MINUTE),MINUTE) AS time,
    SUM(httpRequest.latency) AS request_time,
    SUM(httpRequest.responseSize) AS response_size
  FROM
    `my-project.nginx_cache.nginx_access_20210915`
  GROUP BY
    1
  ORDER BY
    time ASC )
```

### 4.8、访问PV、UV统计（以5分钟为单位）

```sql
SELECT
  TIMESTAMP_TRUNC(TIMESTAMP_SUB(timestamp, INTERVAL MOD(EXTRACT(MINUTE
        FROM
          timestamp), 5) MINUTE),MINUTE) AS time,
  COUNT(*) AS pv,
  COUNT(httpRequest.remoteIp) AS uv
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
ORDER BY
  time ASC
```

### 4.9、缓存状态流量统计

```sql
SELECT
  jsonPayload.httprequest.cachestatus,
  SUM(httpRequest.responseSize) / 1024 / 1024 / 1024
FROM
  `my-project.nginx_cache.nginx_access_20210915`
GROUP BY
  1
```

### 4.10、按国家地区统计

参考链接：<a href="https://cloud.google.com/blog/products/data-analytics/geolocation-with-bigquery-de-identify-76-million-ip-addresses-in-20-seconds">https://cloud.google.com/blog/products/data-analytics/geolocation-with-bigquery-de-identify-76-million-ip-addresses-in-20-seconds</a>

GeoLite2 Country Free Downloadable Databases：<a href="https://github.com/datasets/geoip2-ipv4">https://github.com/datasets/geoip2-ipv4</a>

