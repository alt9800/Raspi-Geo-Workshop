# 用語対訳表 / Glossary (JA–EN)

本ワークショップで頻出する用語の対訳。講師の説明・翻訳表示の補助用。
Terms frequently used in this workshop, for reference alongside the live translation.

## 地図・タイル / Maps and tiles

| 日本語 | English | 補足 / Note |
|---|---|---|
| タイル配信 | tile serving | |
| タイルサーバー | tile server | |
| ベクトルタイル | vector tiles | 中身は MVT (Mapbox Vector Tile) |
| ラスタータイル | raster tiles | PNG/JPEG/WebP |
| ベースマップ | basemap | |
| ズームレベル | zoom level | z0–z14 など |
| 地物 | feature | 地図上のオブジェクト |
| 属性 | attribute / property | クリックで引ける情報 |
| スタイル | style | 描画ルール (MapLibre style JSON) |
| 陰影図 | hillshade | DEM から作る |
| 数値標高モデル | DEM (Digital Elevation Model) | |
| 出典表記・帰属 | attribution | |

## データ / Datasets

| 日本語 | English | 補足 / Note |
|---|---|---|
| 指定緊急避難場所 | designated emergency evacuation site | 地理院データ |
| 洪水浸水想定区域 | flood inundation zone | 国土数値情報 A31 |
| 土砂災害警戒区域 | landslide (sediment disaster) warning zone | 国土数値情報 A33 |
| ハザードマップ | hazard map | |
| 基盤地図情報 | Fundamental Geospatial Data | 地理院 |
| 国土数値情報 | National Land Numerical Information | 国交省 |
| 国土地理院 | GSI (Geospatial Information Authority of Japan) | |
| 国土交通省 | MLIT (Ministry of Land, Infrastructure, Transport and Tourism) | |

## サーバー・CLI / Server and CLI

| 日本語 | English | 補足 / Note |
|---|---|---|
| 常駐サービス | (background) service / daemon | systemd 管理 |
| 死活監視 | health check / liveness monitoring | `/health` |
| ログを追尾する | follow the log | `journalctl -f` |
| 再起動 | restart | サービスの restart と OS の reboot を区別 |
| 差し替え | swap / replace | 本日の演習: ファイル 1 つの mv |
| 設定ファイル | config file | config.yaml |
| 単一バイナリ | single binary | Martin の利点 |
| 権限 / sudo | privileges / sudo | |

## ネットワーク / Networking

| 日本語 | English | 補足 / Note |
|---|---|---|
| アクセスポイント (AP) 化 | turning the Pi into a Wi-Fi access point | 本日は片道 |
| 片道 (戻らない切り替え) | one-way switch (no automatic way back) | |
| 自動接続プロファイル | autoconnect profile | NetworkManager |
| 固定リース | static (DHCP) lease | 席番号=IP 規約の実体 |
| 上流ルーター | upstream router | 本日は Opal |
| 電源断 | power loss / pulling the plug | |
| 無人復帰 | unattended recovery | 本日の山場 |
| オフライン | offline | |
| 席番号 | seat number | NN = IP 末尾 |
| チャンネル分散 | channel distribution | 5GHz W52: ch 36/40/44/48 |
| トンネル | tunnel | ngrok / Cloudflare Tunnel |

## ハードウェア / Hardware

| 日本語 | English | 補足 / Note |
|---|---|---|
| 消費電力 | power consumption | Pi 4 + ffmpeg + HLS ≈ 3W |
| 電圧降下 | voltage drop | 不調の主因 |
| 書き込み耐性 | write endurance | SD カード |
| 現物回覧 | passing hardware around | |
| 無償譲渡 | free giveaway / transfer at no cost | 学生向け |
