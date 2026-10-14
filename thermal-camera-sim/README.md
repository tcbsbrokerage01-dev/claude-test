# 熱成像相機模擬（EP.02）

Vibe Coding 系列 EP.02：只有一個畫面的熱像儀觀景窗。

整個畫面是一個即時運算的熱場，游標和手指就是熱源，熱會擴散、冷卻。蝴蝶在畫面裡飛、拍翅膀，飛過的地方留下熱殘影；貓會呼吸、擺尾巴、晃頭、眨眼。兩者外面有跟著移動的目標鎖定框，顯示即時最高溫。右下角可以切換五種熱像色盤。畫面上的溫度都是模擬值。

直接用瀏覽器打開 `index.html` 就能看。

## 檔案

| 檔案 | 用途 |
| --- | --- |
| `index.html` | 整個網站（HTML、CSS、JS 都在裡面） |
| `heatmaps.js` | 貓和蝴蝶的熱度圖（灰階 = 熱度，alpha = 去背遮罩），由腳本產生 |
| `tools/build_heatmaps.py` | 從封面圖裁切素材、反推熱度、去背，輸出 `heatmaps.js` |
| `PROMPT.md` | 重做這個網站的 prompt，以及生成高解析素材的 prompt |

重新產生熱度圖（需要 Python 3、numpy、Pillow）：

```bash
python3 tools/build_heatmaps.py path/to/ep02-cover.webp
```
