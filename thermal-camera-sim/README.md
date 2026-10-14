# 熱成像相機模擬（EP.02）

Vibe Coding 系列 EP.02 的單頁網站：用 Canvas 即時模擬熱像儀畫面。

- **觀景窗**：可互動的熱場，游標就是熱源，熱會擴散、冷卻。貓和蝴蝶是場裡的固定熱源。
- **樣本庫**：5 張取自封面圖的熱影像，顏色被反推成熱度後，用目前色盤重新顯像。
- **即時鏡頭**：上傳照片或開啟相機轉成熱像，可拖曳分割線比對，有 LEVEL / SPAN 調整。
- **色盤**：鐵虹、彩虹、極地、白熱、等溫，一鍵套用整頁。

所有溫度都是模擬值，不是量測結果。

## 開啟方式

直接用瀏覽器打開 `index.html` 就能看。相機功能需要 https 或 localhost：

```bash
cd thermal-camera-sim
python3 -m http.server 8000
# 開啟 http://localhost:8000
```

## 檔案

| 檔案 | 用途 |
| --- | --- |
| `index.html` | 整個網站（HTML、CSS、JS 都在裡面） |
| `heatmaps.js` | 5 張熱度圖（灰階 = 熱度，alpha = 去背遮罩），由腳本產生 |
| `tools/build_heatmaps.py` | 從封面圖裁切素材、反推熱度、去背，輸出 `heatmaps.js` |
| `PROMPT.md` | 重做這個網站的 prompt，以及生成高解析素材的 prompt |

重新產生熱度圖（需要 Python 3、numpy、Pillow）：

```bash
python3 tools/build_heatmaps.py path/to/ep02-cover.webp
```
