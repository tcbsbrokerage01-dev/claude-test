# EP.02 熱成像相機模擬：Prompt

## 1. 網站 Prompt（貼進 Claude、Cursor、v0、Bolt 等 vibe coding 工具）

先附上 5 張素材圖，再貼下面這段：

```
你是資深創意前端工程師。請做一個單頁網站「熱成像相機模擬 Thermal Camera Simulation」，
是 Vibe Coding 系列 EP.02。只用原生 HTML + CSS + JavaScript（Canvas 2D），不要框架。

【素材】
我附上 5 張熱成像風格圖片：貓、蝴蝶、夜間的鹿（斜切一半熱像、一半可見光）、
動態殘影人像、仿《創造亞當》的雙手。
請先把每張圖的顏色反推成 0–1 的「熱度值」（以彩虹熱像色盤為參考，找最接近的顏色），
淺灰背景去背變成透明。之後所有畫面都用熱度值 + 色盤即時上色，不直接顯示原圖。

【視覺風格：未來科技感的熱像儀觀景窗】
- 只有深色主題，底色接近黑的深藍 #05060c，細線 HUD 框線，四角對焦框
- 字體：英文標題 Chakra Petch，中文 Noto Sans TC 900，數據用 IBM Plex Mono（等寬數字）
- 細掃描線疊層、緩慢掃過的亮帶、閃爍的 REC 紅點與計時器
- 強調色跟著目前色盤改變（鐵虹是橘色、彩虹是青色……）
- 不要用 emoji、不要圓角卡片堆疊、不要紫藍漸層

【頁面結構】
1. Hero 觀景窗（約 78vh）
   - 全幅 Canvas 熱場模擬：網格做熱擴散（拉普拉斯）+ 往環境溫度冷卻
   - 貓和蝴蝶的熱度圖當成固定熱源放在熱場右側，周圍會有熱暈
   - 滑鼠/手指移動會沿路徑加熱，點擊投下一團熱；閒置時有自動巡航的熱點
   - HUD：準星跟著游標並顯示該點溫度、右側直式溫度刻度（16–38°C）、
     上方「LWIR 8–14 µm · ε 0.95 · 模擬」、下方 MAX / MIN / AVG / SPOT / FPS
   - 標題：「Thermal Camera Simulation」+ 大字「02 熱成像相機模擬」（02 用描邊字）
2. 樣本庫：5 張樣本卡（SPC-01～05），12 欄不對稱網格（貓直式跨兩列、雙手橫跨整列）
   每張卡有自己的溫度範圍、迷你刻度、MAX/MIN，滑鼠移上去顯示該點溫度
3. 即時鏡頭：上傳照片 / 開啟相機 / 封面樣本三種來源
   - 斜線分割：左邊原圖、右邊熱像，可拖曳
   - LEVEL（中心溫度）與 SPAN（溫度跨距）滑桿，跟真的熱像儀一樣
   - 照片用亮度 + 膚色估算熱度（臉和手會最亮），可反轉；相機畫面鏡像
   - 相機被拒絕時要顯示清楚的替代說明，不要直接壞掉
4. 色盤：鐵虹 Ironbow、彩虹 Rainbow HC、極地 Arctic、白熱 White Hot、等溫 Isotherm
   每列有漸層條與一句用途說明，點一下就套用到整頁；頂部導覽列也有同樣的色盤切換
5. 頁尾：註明所有溫度都是模擬值

【技術要求】
- 熱場用低解析網格（約 200–300 欄）計算，再放大繪製（平滑插值）＝熱像儀的柔和感
- 色盤用 256 色查找表（LUT），切色盤時所有 canvas 同步重繪
- 熱場離開畫面時暫停運算；尊重 prefers-reduced-motion
- 手機版（390px）不能水平捲動，Hero 標題不能壓在主體上
- 照片與相機畫面只在瀏覽器處理，不上傳
```

## 2. 素材 Prompt（想換高解析素材時用）

目前網站用的素材是從 EP.02 封面圖裁下來的，每張只有 136–248 px 寬，放大看會糊。要換高解析版本，可以用下面的 prompt 在 Midjourney、Imagen、GPT-image 等工具生成：

| 素材 | Prompt |
| --- | --- |
| 貓 | `thermal camera image of a sitting cat in side profile, rainbow thermal palette, warm orange-red body, cool blue and purple head and shadows, glowing white eyes, isolated on plain light grey background, clean edges, high detail --ar 3:4` |
| 蝴蝶 | `thermal imaging of a butterfly seen from above, wings fully spread and symmetrical, heat map palette of magenta, cyan, yellow and blue, isolated on plain light grey background, high detail --ar 16:10` |
| 鹿 | `night thermal scope view of a deer standing in tall grass, yellow-orange deer on deep blue background, frame split diagonally, top-right half is a normal visible-light photo of dry grass --ar 1:1` |
| 殘影人像 | `abstract thermal camera portrait with heavy horizontal motion blur, ghostly glitch silhouette, red orange yellow core fading into blue, black background --ar 1:1` |
| 雙手 | `The Creation of Adam hands reinterpreted as a thermal camera image, two reaching hands, rainbow heat map palette red yellow green, isolated on plain light grey background --ar 8:3` |

換素材後要重新產生 `heatmaps.js`。現在的 `tools/build_heatmaps.py` 是從封面圖用座標裁切，換成獨立圖檔時需要把裁切那段改成直接讀檔。
