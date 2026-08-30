# セットアップ実録: 詰まりどころと対処

マスター SD の構築から複製までを実際に行った際に、手が止まった箇所と
その対処をまとめたものです。同じ構成 (Raspberry Pi OS Lite 64bit +
Martin + nginx + NetworkManager) を再現する人の時間節約のために公開します。

## 1. Raspberry Pi Imager の設定持ち越し

症状: パスワード欄が「Saved (hidden) — leave blank to keep」と表示される。

Imager は前回セッションのカスタマイズ内容 (パスワードや SSH 鍵のパス) を
保持します。空欄のまま進めると「前回の値」が焼かれ、意図したパスワードで
入れないカードができます。

対処: 書き込みのたびにカスタマイズ画面を目視確認し、パスワードは
毎回明示的に入力し直す。SSH 公開鍵の設定は Imager に任せず、初回は
パスワード認証で入り `ssh-copy-id` で鍵を入れる方が確実です。

## 2. .local (mDNS) で見つからない — ネットワーク不一致

症状: `ping pi-00.local` が解決しない、または以前の IP を返す。

mDNS は同一セグメント内のマルチキャストで解決されます。Mac と Pi が
別の Wi-Fi (例: Mac は持ち運びルーター、Pi は会場 AP) に居ると絶対に
届きません。今回も宿→会場の移動でこれを踏みました。

対処: まず「Mac と Pi が同じ SSID に居るか」を確認する。
どのネットワークに Pi が居るか不明なときは、LanScan 等でセグメントを
スキャンし、MAC アドレスの OUI で Raspberry Pi を探す
(`b8:27:eb` / `dc:a6:32` / `e4:5f:01` / `d8:3a:dd` / `2c:cf:67`)。
Vendor 欄に "Raspberry Pi Foundation" と出るツールなら一目で分かります。

## 3. SSH Connection refused — 初回起動は遅い

症状: ping は通るのに `ssh` が Connection refused。

初回ブートはファイルシステム拡張→再起動→ユーザー作成→sshd 起動という
段取りで、電源投入から 3–5 分かかります。ping 応答は sshd より先に
返り始めます。

対処: 2–3 分待って再試行。5 分以上待っても拒否される場合のみ、
カードを Mac に挿し boot パーティションに `ssh` という空ファイルを置く
(`touch /Volumes/bootfs/ssh`)。

## 4. host key 警告 / known_hosts の衝突

症状: `REMOTE HOST IDENTIFICATION HAS CHANGED` 警告、または
以前と違う IP で繋がらない。

同名ホストの IP が変わった場合 (ネットワーク移動) や、同じ名前で
別カード・再フラッシュ後のカードに繋ぐ場合に必ず出ます。

対処: `ssh-keygen -R pi-00.local` (または該当 IP) で該当行を消して
から接続し直す。複製カードを次々検証する作業では
`ssh-keygen -R pi-00.local; ssh workshop@pi-00.local` を 1 行にして
使い回すと速い。

## 5. /tmp が tmpfs で溢れる (RAM 512MB 機)

症状: `/tmp` でアーカイブを展開中に `No space left on device`。
壊れたバイナリが配置され、実行時に
`error while loading shared libraries` などの意味不明なエラーになる。

ログの tmpfs 化などの構成により `/tmp` が RAM 上 (今回 208MB) に
なっている場合、数百 MB の展開で簡単に溢れます。しかも tar は途中まで
書いてから失敗するため、後続の `mv` が壊れたファイルを運びます。

対処: ダウンロード・展開はホームディレクトリで行う。溢れた後は
展開先と配置先の両方を掃除してからやり直す。`df -h /tmp` で事前確認。

## 6. Martin gnu ビルドの依存不足 → musl 版を使う

症状: `martin: error while loading shared libraries: libuv.so.1`。

リリースの `aarch64-unknown-linux-gnu` ビルドは共有ライブラリに
依存します。素の Raspberry Pi OS Lite には libuv が入っていません。

対処: `aarch64-unknown-linux-musl` ビルド (静的リンク) を使う。
依存ゼロで確実に動き、配布イメージの頑健性も上がります。

## 7. nginx の sites-enabled シンボリックリンク

症状: 設定を書いて `nginx -t` も通るのに、期待した location が
404 を返す。

`ln -sf 新設定 /etc/nginx/sites-enabled/default` は、`default` が
既存のシンボリックリンクだった場合に意図と違う結果になり得ます。
実際、リンクは元の `sites-available/default` を指したままでした。

対処: `ls -la /etc/nginx/sites-enabled/` でリンク先を必ず目視する。
確実な手順は「全部消してから張る」:

```
sudo rm -f /etc/nginx/sites-enabled/*
sudo ln -s /etc/nginx/sites-available/workshop /etc/nginx/sites-enabled/workshop
sudo nginx -t && sudo systemctl restart nginx
```

## 8. オフラインで死ぬ外部依存 (CDN と glyphs)

症状: オンラインでは完璧に動くビューワが、AP モード (オフライン) に
すると真っ白になる、または地名ラベルだけ消える。

MapLibre GL JS / pmtiles.js を CDN (unpkg 等) から読んでいると、
オフライン化した瞬間に本体ごと動かなくなります。ライブラリを
ローカルに置いても、style の `glyphs` が外部 URL (フォントグリフ配信)
を指しているとラベルだけ静かに消えます。後者は気づきにくい。

対処:

- JS/CSS はサーバー自身に置いて相対パスで読む
- グリフ (PBF) 一式もサーバーに置き、`glyphs` を相対パスにする
- 検証は DevTools の Network タブで「全リクエストのドメインが
  サーバー自身だけか」を見る。これが最も確実なオフライン耐性チェック

## 9. Martin の --font は TTF が対象 (PBF グリフではない)

症状: 変換済み PBF グリフのディレクトリを `--font` に渡すと
`No font files found`。

Martin の `--font` は TTF/OTF を受け取り実行時に PBF へ変換する機能です。
既に PBF 化されたグリフ一式はフォントファイルとして認識されません。

対処: PBF グリフ一式は Web サーバー (nginx) で静的配信するのが
最短。Martin に配信させたい場合は TTF を渡す。

## 10. 長いヒアドキュメントのペースト事故

症状: `tee <<'EOF'` で複数行スクリプトを SSH 越しに貼り付けたら、
行が欠落・混線した壊れたファイルができた。

端末とネットワークの状態によっては長文ペーストは化けます。壊れた
スクリプトは一見それらしく置かれるので、実行して初めて気づきます。

対処: 長いスクリプトは scp でファイル転送する。ペーストするなら
貼った直後に `cat` で全文を目視してから実行する。今回のように
時間がないときは、1 コマンドずつ実行する方式に切り替えるのも有効。

## 11. dd の基本 3 点 (macOS)

- `of=~/master.img` は sudo 下で `~` が展開されず失敗する。
  `/Users/名前/master.img` とフルパスで書く
- 対象ディスクは `diskutil list` で必ず確認。`external, physical` かつ
  サイズが一致するものだけ。iOS シミュレータ等の disk image が
  多数並ぶ環境では特に注意
- `rdiskN` (raw デバイス) を使うと `diskN` より大幅に速い

## 12. 複製前に「初期状態へ戻す」チェック

イメージ吸い出しは、検証で動かした状態を元に戻してから行う必要が
あります。今回の構成でいうと:

- AP モードのまま吸うと、複製カード全部が起動直後から AP になる
- hazard を差し替え演習で `.bak` から戻し忘れると、種明かし済みの
  状態が配布される
- autoconnect の on/off、hostname が初期値かも含めて、
  「電源を入れたら何が起きるカードか」を吸い出し直前に一度通しで確認する

## 13. Imager での複製は SKIP CUSTOMISATION

マスターイメージからの複製書き込みでは、Imager のカスタマイズ画面で
必ず「SKIP CUSTOMISATION」を選ぶ。設定は全てイメージ内に焼き込み済み
であり、ここで Imager の設定 (それも前回の持ち越し値かもしれない) を
当てると上書きで壊れます。verify は有効のままにする。
