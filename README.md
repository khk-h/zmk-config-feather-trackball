# zmk-config-feather-trackball

19mm トラックボール付き分割キーボード（自作）の ZMK 設定。今は机上試験用で、Adafruit Feather nRF52840 Express ＋ タクトスイッチ4個 ＋ PMW3610 基板の組み合わせで、キーとトラックボールが ZMK で動くかを確かめる。

## 中身

| 場所 | 内容 |
|---|---|
| `boards/arm/feather_nrf52840_zmk/` | Feather nRF52840 Express のボード定義（ZMK v0.3.0 の nice!nano の定義が元。Feather に入っている UF2 ブートローダの配置に合わせた） |
| `boards/shields/trackball_test/` | 机上試験用のシールド（ピンの割り当て・キーマップ） |
| `config/west.yml` | ZMK v0.3.0 と PMW3610 ドライバ（badjeff/zmk-pmw3610-driver の zmk-0.3 ブランチ）の版 |
| `build.yaml` | GitHub Actions でビルドする組み合わせ |

## 配線

| 部品 | 端子 | Feather |
|---|---|---|
| PMW3610 基板 | VIN / GND | 3V / GND |
| | SDIO / SCLK | MO（P0.13）/ SCK（P0.14） |
| | nCS / MOTION | 10（P0.27）/ 9（P0.26） |
| キー（ダイオードなし） | 行0 / 行1 | 5（P1.08）/ 6（P0.07） |
| | 列0 / 列1 | 11（P0.06）/ 12（P0.08） |

キーマップ：SW1（5–11）左クリック、SW2（5–12）右クリック、SW3（6–11）A、SW4（6–12）Bluetooth のペアリング情報を消す。

## 書き込み

1. GitHub の Actions で最新のビルドを開き、Artifacts の `firmware` をダウンロードして展開する
2. Feather を USB でつなぎ、リセットボタンを素早く2回押す（`FTHR840BOOT` ドライブが出る）
3. `trackball_test-feather_nrf52840_zmk-zmk.uf2` をそのドライブにドラッグする

## ライセンス

MIT（`LICENSE`）。[snize/zmk-seiboku-example](https://github.com/snize/zmk-seiboku-example)（MIT）を元にしている。
