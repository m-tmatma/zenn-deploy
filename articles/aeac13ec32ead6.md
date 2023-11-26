---
title: "時刻を扱うシステムコールまとめ"
emoji: "👋"
type: "tech"
topics:
  - "time"
published: true
published_at: "2022-01-23 15:45"
---

# 時刻を扱うシステムコールまとめ

現在時刻取得などで Epoch Time(=1970-01-01 00:00:00 +0000, UTC) からの秒数として取得できるが、人間がわかりやすいようにカレンダー時間に変換する。いろんな関数があり、使うたびに調べているので、まとめておく。

※ システムコールと書いていますが、所謂 スーパバイザコールという意味ではなく、単にAPIとして利用可能な関数という意味で書いています。

## 構造体、変数定義など

### clockid_t

現在時刻の取得では `CLOCK_REALTIME` を使う。

### time_t

Epoch time からの秒数を表す。
32bit で表現することによって、UTC 2038年1月19日3時14分8秒以降正しく時間を扱えない問題、通称2038年問題が発生する。(time_t を 64bit にしてこの問題の修正が進みつつある模様)

### struct timespec (Epoch time からの秒数。nanosec 精度)

```
struct timespec {
    time_t   tv_sec;        /* seconds */
    long     tv_nsec;       /* nanoseconds */
};
```

以下関数などで使用

* clock_gettime
* clock_getres

### struct timeval (Epoch time からの秒数。マイクロ秒精度)

```
struct timeval {
    time_t      tv_sec;     /* 秒 */
    suseconds_t tv_usec;    /* マイクロ秒 */
};
```

以下関数などで使用

* gettimeofday

### struct tm (カレンダー時間)

```
struct tm {
    int tm_sec;        /* 秒 (0-60) */
    int tm_min;        /* 分 (0-59) */
    int tm_hour;       /* 時間 (0-23) */
    int tm_mday;       /* 月内の日付 (1-31) */
    int tm_mon;        /* 月 (0-11) */
    int tm_year;       /* 年 - 1900 */
    int tm_wday;       /* 曜日 (0-6, 日曜 = 0) */
    int tm_yday;       /* 年内通算日 (0-365, 1 月 1 日 = 0) */
    int tm_isdst;      /* 夏時間 */
};
```

* 2022 年の場合 `tm_year` には `2022 - 1900 = 122` を設定する。
* 2月の場合、`tm_mon` には `2 - 1 = 1` を設定する。

以下関数などで使用

* localtime_r
* localtime
* gmtime_r
* gmtime
* mktime
* timelocal
* timegm


### 参考サイト

https://linuxjm.osdn.jp/html/LDP_man-pages/man2/clock_gettime.2.html

https://linuxjm.osdn.jp/html/LDP_man-pages/man2/gettimeofday.2.html

https://linuxjm.osdn.jp/html/LDP_man-pages/man3/localtime.3.html


## Epoch Timeからの秒数 ⇒ 現在時刻取得

`入力データ` は引数として与えるものという意味ではなく、システムコール呼び出し時に情報として与える必要があるものを意味している。

||入力データ|出力データ|取得単位|備考||
|--|--|--|--|--|--|
|time_t time(time_t *tloc);|なし|現在時間|秒|秒精度しか取れない|[TIME](https://linuxjm.osdn.jp/html/LDP_man-pages/man2/time.2.html)|
|int gettimeofday(struct timeval *tv, struct timezone *tz);|なし|現在時間|秒+マイクロ秒|POSIX.1-2008 では gettimeofday() は廃止予定|[GETTIMEOFDAY](https://linuxjm.osdn.jp/html/LDP_man-pages/man2/gettimeofday.2.html)|
|int clock_gettime(clockid_t clk_id, struct timespec *tp);|取得する clock のタイプ|現在時間|秒+ナノ秒|**おすすめ**|[CLOCK_GETRES](https://linuxjm.osdn.jp/html/LDP_man-pages/man2/clock_gettime.2.html)|

## クロックの分解能

||入力データ|出力データ|取得単位|備考||
|--|--|--|--|--|--|
|int clock_getres(clockid_t clk_id, struct timespec *res);|取得する clock のタイプ|クロックの分解能|秒, ナノ秒||[CLOCK_GETRES](https://linuxjm.osdn.jp/html/LDP_man-pages/man2/clock_gettime.2.html)|

## Epoch Timeからの秒数  ⇒ カレンダー時間 (ローカルタイム)


||入力データ|出力データ|取得単位|備考||
|--|--|--|--|--|--|
|struct tm *localtime_r(const time_t *timep, struct tm *result)|Epoch Timeからの秒数|カレンダー時間 (ローカルタイム)|秒|**おすすめ**|[CTIME](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/ctime.3.html)|
|struct tm *localtime(const time_t *timep);|Epoch Timeからの秒数|カレンダー時間 (ローカルタイム)|秒|スレッドセーフでないので localtime_r を使うべし|[CTIME](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/ctime.3.html)|

## Epoch Timeからの秒数 ⇒ カレンダー時間 (UTC)

||入力データ|出力データ|取得単位|備考||
|--|--|--|--|--|--|
|struct tm *gmtime_r(const time_t *timep, struct tm *result);|Epoch Timeからの秒数|カレンダー時間 (UTC)|秒|**おすすめ**|[CTIME](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/ctime.3.html)|
|struct tm *gmtime(const time_t *timep);|Epoch Timeからの秒数|カレンダー時間 (UTC)|秒|スレッドセーフでないので gmtime_r を使うべし|[CTIME](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/ctime.3.html)|

## カレンダー時間 (ローカルタイム) ⇒  Epoch Timeからの秒数

||入力データ|出力データ|取得単位|備考||
|--|--|--|--|--|--|
|time_t mktime(struct tm *tm);|カレンダー時間 (ローカルタイム)|Epoch Time|秒|**おすすめ**|[CTIME](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/ctime.3.html)|
|time_t timelocal(struct tm *tm);|カレンダー時間 (ローカルタイム)|Epoch Time|秒|mktime を使えばよいので使う必要ない。<br>glic の場合 time.h のインクルード前に _DEFAULT_SOURCE を定義すれば利用可能|[TIMEGM](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/timegm.3.html)|

## カレンダー時間 (UTC) ⇒  Epoch Timeからの秒数

||入力データ|出力データ|取得単位|備考||
|--|--|--|--|--|--|
|time_t timegm(struct tm *tm);|カレンダー時間 (UTC)|Epoch Time|秒|glic の場合 time.h のインクルード前に _DEFAULT_SOURCE を定義すれば利用可能|[TIMEGM](https://linuxjm.osdn.jp/html/LDP_man-pages/man3/timegm.3.html)|

