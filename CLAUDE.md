# color-memo 作業メモ

## 現在のブランチ
`claude/fix-save-button-single-image-4wznw`

## 解決済み（main にマージ済み）
- 縦写真が横伸びする問題（html2canvas が object-fit: cover を無視する問題）
  - `<img>` の表示を `<canvas id="photo-canvas">` に置き換えて JS で cover 描画

## 作業中（ブランチに push 済み・未マージ）

### 1. 保存ボタンの見切れ（未解決）
- PC ブラウザでウィンドウ幅が狭いと「画像を保存する」ボタンが見切れる
- 原因：`#scale-wrapper` の `overflow: hidden` が縦方向もクリップし、
  `scaleWrapper.style.height` がロード時に固定されるため、後から表示される
  保存ボタンがクリップされる
- 試みた修正：`overflow-x: hidden` に変更 + `transform-origin: top center` 追加
  → まだ改善されていない

### 2. 色抽出アルゴリズムの刷新（未検証）
- ColorThief の `getPalette()` を廃止し、canvas でピクセルを直接解析する方式に変更
- `getColor()` で支配色相を決定 → ±36° 以内のピクセルを抽出 → 明度帯の平均色を算出
- 暗い2色・明るい3色の構成
- まだ実際の画像で十分に検証できていない

## 参考：理想の出力例
下記のような「支配色相で統一された明暗グラデ5色」を目指している
- 桜写真（青系）→ #204559, #4884A2, #86A1B1, #A7BBC4, #D7DCDC
- 泡写真（青系）→ #3A5D6C, #749EB1, #B6C8CA, #2F7FA2, #FCFCFC
