# SOTOBŌ TIDE（外房タイドグラフ）

いすみ川河口を初期地点にした、スマートフォン対応の釣り向けタイドグラフWebアプリです。

## 主な機能

- いすみ川河口、大原港、太東崎、御宿、勝浦港、鴨川港、銚子漁港、館山港を切り替え
- 任意の緯度経度を指定したマイポイントの追加・削除
- 海面水位モデル予測グラフ、現在の上げ・下げの目安、予想満潮・干潮の時刻
- 日付、3日／1日／±12時間／±6時間の表示切り替え
- 天気、風速、風向、海面気圧、波高、周期、海面水温、日の出・日の入り、概算月齢
- 釣果メモ（魚種、サイズ、ルアー、メモ、予測水位）の端末内保存
- PWA対応（iPhone/Androidでホーム画面に追加可能）

## 開始するには

`index.html` を開くと画面は表示されます。ただし、ブラウザの設定によっては `file://` からの外部通信が制限されるため、Webサーバー上で動かすことを推奨します。

### ローカルで動かす

```bash
cd sotobo-tide
python3 -m http.server 8000
```

ブラウザで `http://localhost:8000` を開いてください（インターネット接続が必要）。

### 無料公開（GitHub Pages）

1. GitHub で新しい **公開リポジトリ**（例：`sotobo-tide`）を作成。
2. このフォルダの `index.html`、`app.js`、`icon.svg`、`manifest.webmanifest`、`sw.js` をリポジトリ直下にアップロード。
3. **Settings → Pages → Build and deployment** で **Deploy from a branch**、`main`、`/(root)` を指定して保存。
4. `https://ユーザー名.github.io/sotobo-tide/` で公開されます。
5. iPhoneのSafariで開いて **共有 → ホーム画面に追加**、AndroidではChromeのメニューからホーム画面に追加できます。

公開リポジトリのコードは誰でも閲覧できます。個人データはログイン不要で端末の `localStorage` に保存され、サーバーには自動アップロードされません。ブラウザのデータを消すと釣果メモも消えるので重要な釣果の唯一の保存先にしないでください。

## データと制約

- 海面水位・波・水温：Open-Meteo Marine API（`sea_level_height_msl`、`wave_height`、`wave_period`、`sea_surface_temperature`）
- 風・気温・海面気圧・日の出/日の入り：Open-Meteo Forecast API
- APIは非商用利用に無料枠があり、利用条件と CC BY 4.0 のクレジット表記が適用されます。商用利用は契約等の確認が必要です。
- 海面水位は**平均海面MSL基準**です。**気象庁の潮位表基準面（DL）や河川水位（TP）ではありません**。グラフや満干潮は海域の数値モデル予測から求めた参考値で、河口内の実測ではありません。
- モデルの空間分解能は約8kmで、隣接する場所の結果は同じ海域格子となる場合があります。日付の利用可能期間はAPIの提供範囲に依存します。
- 観測潮位、潮位偏差、河川実測水位、水質、海流の実況、写真付き釣果、GPS自動選局は今回の版にはありません。
- 海況・水深・出港可否など安全に関わる判断に用いず、海上保安庁・気象庁の公式情報を優先してください。

## 改造例

`app.js` の `BUILTIN_POINTS` にポイントを追加できます。`lat`・`lon` は釣り場の表示位置、`seaLat`・`seaLon` は予測に使う外海側の参照座標です。

## ファイル

- `index.html`: アプリの画面
- `app.js`: 状態管理、データ取得、グラフ描画
- `manifest.webmanifest`, `icon.svg`, `sw.js`: ホーム画面追加とアプリシェルのオフラインキャッシュ

## 出典

- https://open-meteo.com/en/docs/marine-weather-api
- https://open-meteo.com/en/docs
- https://www.data.jma.go.jp/kaiyou/db/tide/suisan/

参考にした機能の構成（オリジナル実装、コードのコピーではありません）：https://kotaro9-design.github.io/my-tidegraph/
