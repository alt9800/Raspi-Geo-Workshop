# Flash your SD card / SD カードを焼く手順

1. **Copy `master.img` from the USB stick to your PC**, then pass the stick to the next person
   **USB メモリから `master.img` を自分の PC にコピー**して、USB は次の人へ
2. Install **Raspberry Pi Imager** (free, Win/Mac/Linux): https://www.raspberrypi.com/software/
   **Raspberry Pi Imager** をインストール (無料・Win/Mac/Linux)
3. In Imager / Imager で:
   - Device: skip / デバイス選択はスキップで OK
   - OS: scroll to the bottom → **Use custom** → select `master.img`
     OS 選択: 一番下の **Use custom (カスタムイメージを使う)** → `master.img`
   - Storage: the **SD card** you received. **Double-check the size and name** —
     picking your internal disk will destroy it
     ストレージ: 配られた **SD カード**。**サイズと名前を必ず確認**
     (内蔵ディスクを選ぶと消えます)
4. When asked "Would you like to apply OS customisation settings?" →
   **NO (SKIP CUSTOMISATION)**. Even if it says "Saved (hidden)", do **not**
   apply — everything is already inside the image
   「設定をカスタマイズしますか？」→ **いいえ**。「Saved (hidden)」と出ても
   **適用しない** (設定は全部イメージ内)
5. Writing + verify takes **15–20 min**. Go back to the lecture — just
   **don't unplug anything** mid-write
   書き込み + 検証で **15〜20 分**。講義に戻って OK。**途中で抜かない**
6. Insert the SD into your Pi and plug in the microUSB power (there is no
   switch). It's SSH-ready in 1–2 min
   SD を Pi に挿して microUSB で電源 ON (スイッチなし)。1〜2 分で SSH 可能

No card reader, or not confident? Come to a flashing station at the front.
リーダーがない・不安な人は前方の焼きステーションへ。

Note: every card is an identical clone — hostname stays `pi-00` until the
"personalize" step later in the session. That's normal.
注: 全カードは同一クローンです。後の「個体化」まで hostname は全台 `pi-00`
のままです (正常)。
