# zmk-config-feather-trackball

19mm トラックボール付き分割キーボード（自作）の ZMK 設定。今は机上試験用で、Adafruit Feather nRF52840 Express ＋ タクトスイッチ4個 ＋ PMW3610 基板の組み合わせで、キーとトラックボールが ZMK で動くかを確かめる。

## 中身

| 場所 | 内容 |
|---|---|
| `boards/arm/feather_nrf52840_zmk/` | Feather nRF52840 Express のボード定義（ZMK v0.3.0 の nice!nano の定義が元。Feather に入っている UF2 ブートローダの配置に合わせた） |
| `boards/shields/trackball_test/` | 机上試験用のシールド（1台。ピンの割り当て・キーマップ） |
| `boards/shields/trackball_test_split/` | 2台（左右の分割）の机上試験用のシールド。右がセントラル（パソコンとつながる、トラックボール付き）、左がペリフェラル（キーだけ） |
| `boards/shields/trackball_test_paw3222/` | 1台の机上試験用のシールドの PAW3222 版（PMW3610 からの切り替えの検討用）。配線は PMW3610 と同じピン |
| `config/west.yml` | ZMK v0.3.0、PMW3610 ドライバ（badjeff/zmk-pmw3610-driver の zmk-0.3 ブランチ）、PAW3222 ドライバ（sekigon-gonnoc/zmk-driver-paw3222 の torabo-tsuki ブランチ）の版 |
| `build.yaml` | GitHub Actions でビルドする組み合わせ |

## 配線

| 部品 | 端子 | Feather |
|---|---|---|
| PMW3610 基板 | VIN / GND | 3V / GND |
| | SDIO / SCLK | MO（P0.13）/ SCK（P0.14） |
| | nCS / MOTION | 10（P0.27）/ 9（P0.26） |
| キー（ダイオードなし） | 行0 / 行1 | 5（P1.08）/ 6（P0.07） |
| | 列0 / 列1 | 11（P0.06）/ 12（P0.08） |

PAW3222 モジュール（のぎけす屋）を使うときは、モジュールのピン 1＝3.3V、2＝CS、3＝MOTION、4＝SDIO、5＝SCLK、6＝GND を、上の表の VIN、nCS、MOTION、SDIO、SCLK、GND と同じ Feather のピンにつなぐ。書き込むのは `trackball_test_paw3222-feather_nrf52840_zmk-zmk.uf2`。

キーマップ：SW1（5–11）左クリック、SW2（5–12）右クリック、SW3（6–11）A、SW4（6–12）Bluetooth のペアリング情報を消す。

## 書き込み

1. GitHub の Actions で最新のビルドを開き、Artifacts の `firmware` をダウンロードして展開する
2. Feather を USB でつなぎ、リセットボタンを素早く2回押す（`FTHR840BOOT` ドライブが出る）
3. `trackball_test-feather_nrf52840_zmk-zmk.uf2` をそのドライブにドラッグする

## 2台（左右の分割）の試験

- 右（セントラル）：上の配線のまま（キー4個＋PMW3610）。`trackball_test_split_right-feather_nrf52840_zmk-zmk.uf2`
- 左（ペリフェラル）：キー4個だけ。キーの配線は右と同じピン（行 5・6、列 11・12）。`trackball_test_split_left-feather_nrf52840_zmk-zmk.uf2`
- キーマップ：右は1台のときと同じ。左は SW1 B、SW2 C、SW3 D、SW4 E（どちらの手の入力か分かるように）
- 書き込みの順番：
  1. 左右とも `settings_reset-feather_nrf52840_zmk-zmk.uf2` を書き込む（前のペアリング情報を消す）
  2. パソコンの Bluetooth の一覧から、前の「TB Test」を削除する
  3. 左に `…split_left…`、右に `…split_right…` を書き込む
  4. 左右の電源を入れると、左と右が自動でつながる。パソコンからは「TB Split Test」（右）とペアリングする

## ライセンス

MIT（`LICENSE`）。[snize/zmk-seiboku-example](https://github.com/snize/zmk-seiboku-example)（MIT）を元にしている。
