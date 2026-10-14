# EP.02 熱成像相機模擬：Prompt

## 網站 Prompt（貼進 Claude、Cursor、v0、Bolt 等 vibe coding 工具）

先附上貓和蝴蝶兩張熱像素材，再貼下面這段：

```
做一個只有一個畫面的首頁「02 熱成像相機模擬 Thermal Camera Simulation」，
不要往下捲的區塊。整個畫面就是一台熱像儀的觀景窗，要酷、要有未來科技感。
只用原生 HTML + CSS + JavaScript（Canvas 2D），不要框架。

【素材處理】
我附上貓和蝴蝶的熱像圖。先把圖片顏色反推成 0–1 的熱度值
（以彩虹熱像色盤為參考找最接近的顏色），淺灰背景去背。
畫面上不直接顯示原圖，全部用熱度值 + 色盤即時上色。

【熱場】
- 全螢幕 Canvas，用低解析網格（約 200–300 欄）做熱擴散 + 冷卻回環境溫度，再平滑放大
- 貓和蝴蝶每一幀都依姿勢重新畫進熱場，主體要會動，不能是靜態圖：
  - 蝴蝶：在畫面左側沿平滑曲線飛行，翅膀拍動（水平壓縮再張開），
    一陣拍翅一陣滑翔，往哪邊飛身體就往哪邊傾斜，飛過的地方留下慢慢冷卻的熱殘影
  - 貓：坐在右側，呼吸起伏、尾巴左右擺、頭微微晃動，每隔幾秒眨一次眼（眼睛熱點暫時變暗）
  - 兩者內部的熱紋路用 3D 雜訊慢慢流動，像活體的溫度變化
- 游標/手指移動沿路加熱，點擊投下一團熱
- 每格加一點隨機雜訊，像感測器顆粒

【HUD】
- 四角對焦框、細掃描線、緩慢掃過的亮帶、邊緣暗角
- 左上：準星 icon + THERMAL/SIM、LWIR 8–14 µm、ε 0.95；右上：閃爍 REC + 計時器、感測器解析度
- 右側直式溫度刻度 16–38°C
- 貓和蝴蝶外面加目標鎖定框（四角括號），框跟著主體移動，
  標籤寫 TGT-01 貓 FELIS CATUS + 即時最高溫，載入時有縮放鎖定的動畫
- 游標準星顯示該點溫度；游標不動時，準星自動追蹤蝴蝶
- 左下大標題：小字 THERMAL CAMERA SIMULATION，大字「02 熱成像相機模擬」（02 用描邊字）
- 底部一排 MAX / MIN / AVG / SPOT / FPS，右下五個色盤按鈕：
  鐵虹 Ironbow、彩虹 Rainbow HC、極地 Arctic、白熱 White Hot、等溫 Isotherm
  切換色盤時，強調色也跟著變

【風格】
- 只有深色，底色 #05060c；英文 Chakra Petch、中文 Noto Sans TC 900、數字 IBM Plex Mono
- 不要 emoji、不要圓角卡片、不要紫藍漸層

【要求】
- 手機（390px）也是一個畫面：主體放上半部，標題在下半部，不能互相壓到
- 手機版蝴蝶的飛行範圍要避開貓
- 尊重 prefers-reduced-motion（主體停止動作、關掉掃描動畫）
- 溫度全部標示為模擬值
```

## 素材 Prompt（想換高解析素材時用）

目前的素材是從 EP.02 封面圖裁下來的，貓只有 136 px 寬、蝴蝶 214 px 寬，所以在大螢幕上會偏糊。要換清楚的版本，可以在 Midjourney、Imagen 等工具生成：

- **貓**：`thermal camera image of a sitting cat in side profile, rainbow thermal palette, warm orange-red body, cool blue and purple head and shadows, glowing white eyes, isolated on plain light grey background, clean edges, high detail --ar 3:4`
- **蝴蝶**：`thermal imaging of a butterfly seen from above, wings fully spread and symmetrical, heat map palette of magenta, cyan, yellow and blue, isolated on plain light grey background, high detail --ar 16:10`

換素材後要重新產生 `heatmaps.js`。現在的 `tools/build_heatmaps.py` 是從封面圖用座標裁切，改用獨立圖檔時，要把裁切那段改成直接讀檔。
