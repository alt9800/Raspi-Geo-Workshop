# SD カードを焼く手順 / Flash your SD card

1. **USB メモリから `master.img` を自分の PC にコピー**して、USB は次の人へ回す
   Copy `master.img` from the USB stick to your PC, then pass the stick on
2. **Raspberry Pi Imager** をインストール: https://www.raspberrypi.com/software/
3. Imager で:
   - デバイス選択: スキップで OK / skip
   - OS 選択: 一番下の **Use custom (カスタムイメージを使う)** → `master.img`
   - ストレージ: 配られた **SD カード**。**サイズと名前を必ず確認**
     (内蔵ディスクを選ぶと消えます / picking the wrong disk destroys it)
4. 書き込み前の確認:
   - **「設定をカスタマイズしますか？」→ いいえ / NO (SKIP CUSTOMISATION)**
   - 「Saved (hidden)」と出ても**適用しない**。設定は全てイメージ内にあります
5. 書き込み + 検証で **15〜20 分**。講義に戻って OK。**途中で抜かない**
6. SD を Pi に挿して microUSB で電源 ON (スイッチなし)。1〜2 分で SSH 可能になります

リーダーがない・不安な場合は前方の焼きステーションへ。
No card reader? Come to a flashing station at the front.

注意: 全カードは同一クローンです。SSH 後に講座内で「個体化」
(`personalize-sd.sh`) を行うまで、hostname は全台 `pi-00` のままです (正常)。
