# KR-clearchecker

**かわロボ クリアチェッカー 2026**

川崎ロボット競技大会（かわロボ）の年間活動を記録するWebチェッカーです。  
企画：[下剋上(かわロボ)](https://note.com/jolly_roses919/n/n93ba68eadac8)

👉 **[チェッカーを開く](https://sin1n24.github.io/KR-clearchecker/)**

---

## 概要

戦績・制作・やらかし・経験など100項目の達成を記録できます。  
年末のアドベントカレンダーで結果発表予定。一番埋めた人には粗品進呈！🎁

## 機能

- ✅ 100項目をタップで達成記録（ブラウザに自動保存）
- 🎉 達成時のクラッカーエフェクト・マイルストーン演出
- 🖼 達成数に応じてイラストが徐々に現れる「全体を見る」モード
- 📥 配布Excelからの達成状況取込
- 📂 JSON書き出し・読込（デバイス間の引継ぎ）
- 📢 X(Twitter)へのシェア機能

## 使い方

1. [チェッカーを開く](https://sin1n24.github.io/KR-clearchecker/)
2. 達成した項目をタップ
3. 進捗はブラウザに自動保存されます
4. **デバイスを変えるとき** → 「データ：⬇ JSON書き出し」でファイル保存 → 新しいデバイスで「⬆ JSON読込」
5. **Excelに入力済みの方** → 「📥 Excel取込」で配布ファイルをそのままインポート

## 評価期間

2026年1月1日 〜 12月上旬（立命杯終了が目安）

---

## 開発者向け

### ファイル構成

```
index.html    # チェッカー本体（単一ファイル）
README.md
```

### ローカルで動かす

```bash
git clone https://github.com/sin1n24/KR-clearchecker.git
cd KR-clearchecker
# ブラウザで index.html を直接開くだけ
```

### Excelイラストを更新する

`めくりイラスト` シートを更新した場合、以下のスクリプトでピクセルデータを再生成します。

```python
import openpyxl, json

wb = openpyxl.load_workbook('かわロボクリアチェッカー2026.xlsx')
ws = wb['めくりイラスト']

def is_near_white(rgb):
    r,g,b = int(rgb[2:4],16), int(rgb[4:6],16), int(rgb[6:8],16)
    return r>240 and g>240 and b>240

flat = []
for r in range(2, 52):
    for c in range(1, 51):
        cell = ws.cell(row=r, column=c)
        rgb = cell.fill.fgColor.rgb if cell.fill and cell.fill.fgColor else 'FFFFFFFF'
        if not rgb or rgb == '00000000': rgb = 'FFFFFFFF'
        flat.append('w' if is_near_white(rgb) else '#'+rgb[2:])

print(json.dumps(flat, separators=(',',':')))
```

出力を `index.html` 内の `const PIXELS=` に貼り替えてください。

### 技術スタック

- 単一HTMLファイル（フレームワーク不使用）
- LocalStorage によるデータ永続化
- [SheetJS](https://sheetjs.com/) — Excelファイル読込
- Google Fonts (Noto Sans JP)
- Canvas API — イラスト描画

---

## License

MIT
