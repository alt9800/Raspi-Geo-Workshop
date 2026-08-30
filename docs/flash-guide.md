# Flash your SD card / SD カードを焼く手順

## English

1. **Copy `master.img` from the USB stick to your PC**, then pass the stick
   to the next person.
2. Install **Raspberry Pi Imager** (free, Win/Mac/Linux):
   https://www.raspberrypi.com/software/
3. In Imager:
   - Device: skip.
   - OS: scroll to the bottom → **Use custom** → select `master.img`.
   - Storage: the **SD card** you received. **Double-check the size and
     name** — picking your internal disk will destroy it.
4. When asked "Would you like to apply OS customisation settings?" →
   **NO (SKIP CUSTOMISATION)**. Even if it says "Saved (hidden)", do **not**
   apply — everything is already inside the image.
5. Writing + verify takes **15–20 min**. Go back to the lecture — just
   **don't unplug anything** mid-write.
6. Insert the SD into your Pi and plug in the microUSB power (there is no
   switch). It's SSH-ready in 1–2 min.

No card reader, or not confident? Come to a flashing station at the front.

Note: every card is an identical clone — hostname stays `pi-00` until the
"personalize" step later in the session. That's normal.

## 日本語

1. **USB メモリから `master.img` を自分の PC にコピー**して、USB は次の人へ
   回してください。
2. **Raspberry Pi Imager** をインストール (無料・Win/Mac/Linux):
   https://www.raspberrypi.com/software/
3. Imager で:
   - デバイス選択: スキップで OK。
   - OS 選択: 一番下の **Use custom (カスタムイメージを使う)** →
     `master.img` を選択。
   - ストレージ: 配られた **SD カード**。**サイズと名前を必ず確認** —
     内蔵ディスクを選ぶと消えます。
4. 「設定をカスタマイズしますか？」→ **いいえ (SKIP CUSTOMISATION)**。
   「Saved (hidden)」と出ても**適用しない**でください。設定は全部イメージに
   入っています。
5. 書き込み + 検証で **15〜20 分**。講義に戻って OK。**途中でカードや USB を
   抜かない**。
6. SD を Pi に挿して microUSB で電源 ON (スイッチはありません)。1〜2 分で
   SSH できる状態になります。

リーダーがない人・不安な人は前方の焼きステーションへどうぞ。

注: 全カードは同一クローンです。後の「個体化」の工程まで、hostname は全台
`pi-00` のままです (正常)。
