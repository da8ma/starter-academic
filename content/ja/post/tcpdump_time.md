---
Title: "tcpdumpのキャプチャ時間を指定する"
date: 2019-11-28T21:35:19+09:00
summary: "tcpdumpの実行時間をローテートオプションで指定する方法"
tags: ["tcpdump", "Linux"]
categories: ["Network"]
profile: false
---

### はじめに

ネットワークのトラブルシューティングやパケットの流れを追うときに使用するtcpdumpですが、
通常実行時はscript等で制御していない限りキャプチャを実行し続けます。

どうせならコマンドオプションで実行時間を指定できないかと思い調べました。  
(終了するのを忘れてpcapファイルが肥大化してたとか避けたい。。)

### 結論

`-W` オプションと `-G`オプションというtcpdumpのログローテートオプションを使用することによって実現可能。

`-W` はログローテートの実行回数、`-G` はログローテートの実行間隔を指定が可能です。  
例えばeth0に流れるすべてのパケットを60秒間キャプチャして、pcapファイルに保存したい場合は以下のようにします。

```
# tcpdump -i eth0  -w ./test.pcap -W1 -G60
```

ローテート回数を1回、間隔を60秒に指定しています。。よって実行時間が60秒に達した時点でローテートが実施されコマンドが終了することを意味しています。
(基本的にファイルを分割して保存したいとかなければ、ローテート回数は1回で良いと思います)

### あとがき

さらに調べたところcronでtcpdumpを実行する際に `timeout` コマンドで実行時間を指定している方がいました。

秒数以外の値を何も指定しない場合は `TERM` シグナルで引数で指定しているコマンドを終了させるので、この方法が一番シンプルな印象です。
~~(この方法のほうが楽のような気がしてきた。)~~

https://qiita.com/gzock/items/bbe2c8798ccc4c355fa6


#### 参考文献

https://moznion.hatenadiary.com/entry/2017/11/09/094634

https://hydrocul.github.io/wiki/commands/timeout.html







