# Serving Map Tiles from a Raspberry Pi

FOSS4G Hiroshima 2026 workshop / 2026-08-30 14:00–17:00 / Room 610

山小屋・災害現場・極域など通信の乏しい環境で使う「持ち運べるタイルサーバー」を、
Raspberry Pi 3A+ 上の Martin と PMTiles で組み、最後は Pi を Wi-Fi AP 化して
スマホへ完全オフラインで地図を配るワークショップです。

A hands-on workshop: build a portable tile server with Martin and PMTiles on a
Raspberry Pi 3A+, then turn the Pi into a Wi-Fi access point and serve maps to
phones fully offline.

公開ページ (HTML / PDF): https://alt9800.github.io/Raspi-Geo-Workshop/

## Contents

| ファイル / File | 内容 / Description |
|---|---|
| [slide.md](slide.md) | スライド (日本語, Marp) / Slides (Japanese) |
| [slide.en.md](slide.en.md) | スライド英語版 / English slides |
| [glossary.md](glossary.md) | 用語対訳表 / JA–EN glossary |
| [timetable.md](timetable.md) | 当日進行表 / Timetable |
| [instructor-notes.md](instructor-notes.md) | 講師あんちょこ / Instructor notes |
| [docs/setup-notes.md](docs/setup-notes.md) | セットアップ実録 (詰まりどころと対処) / Setup postmortem |

第三部はオフラインで進行するため、参加者は開始時に公開ページから PDF を
ダウンロードしてください。
Part 3 runs offline — participants should download slide.pdf at the start.

## Data attribution

- © OpenMapTiles © OpenStreetMap contributors
- 「指定緊急避難場所データ」(国土地理院) をもとに作成 /
  Derived from "Designated Emergency Evacuation Site Data", GSI Japan
- 「国土数値情報 (洪水浸水想定区域データ)」(国土交通省) をもとに作成 /
  Derived from "National Land Numerical Information (Flood Inundation Zones)", MLIT Japan
- 「国土数値情報 (土砂災害警戒区域データ)」(国土交通省) をもとに作成 /
  Derived from "National Land Numerical Information (Landslide Warning Zones)", MLIT Japan
- 「基盤地図情報 数値標高モデル」(国土地理院) をもとに作成 /
  Derived from "Fundamental Geospatial Data (DEM)", GSI Japan

タイルデータの生成手順・カタログはデータリポジトリ (catalog.html /
data-pipeline.md) を参照してください。
See the data repository (catalog.html / data-pipeline.md) for the tile build
pipeline and catalog.

## Build slides locally

```
npx @marp-team/marp-cli slide.md -o slide.pdf
```
