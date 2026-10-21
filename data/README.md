# 題庫資料（data/）

網頁 `index.html` 只放版面與程式；開啟時讀取 `manifest.json`，再依 `files` 的順序載入各 JSON。

| 檔案 | 內容 |
|---|---|
| tags.json | 所有考點標籤（錯題分析依此順序顯示） |
| toeic_p5_mock.json | Part 5 綜合模擬、早期專項特訓 |
| toeic_p5_s1.json ／ s2 ／ s3 | Part 5 第一～三階段 |
| toeic_p5_adv.json | Part 5 進階考點 |
| toeic_p6.json | Part 6 |
| toeic_part7.json | Part 7 |
| bigdata_past.json ／ bigdata_mock.json | 巨量資料概論 考古題／模擬考 |

## 格式

```json
{"about":"說明","tags":{…可省略…},"exams":[
  {"id":"p5_pos","kind":"toeic","part":5,"stage":1,"title":"詞性題","sub":"…",
   "qs":[ {"g":"t_p5pos","q":"題目","o":["A","B","C","D"],"a":1,"h":"提示","trans":"翻譯","e":"詳解","w":"其他選項"} ]}
]}
```

- `kind`：`toeic`、`past`（考古題）、`mock`（巨量資料模擬考）。
- `acc`（可省略）：官方公告多個答案都給分時列出所有正確選項索引，例如 `"acc":[0,2]`；一律給分用 `[0,1,2,3]`。`a` 仍要填其中一個。
- 一題一行，方便 git diff 與審閱。

## 修改規則（重要）

1. 作答紀錄以「題組 id－題目順序」記錄，所以**新增題目一律加在該題組 `qs` 的最後面**，不要插入中間、不要刪題、不要改題組 `id`。
2. 改完任何 JSON 後執行 `python tools/update_manifest.py`：檢查 JSON 格式、答案索引、題組 ID 重複，並更新版本雜湊 `v`（讓瀏覽器抓到新版）。
3. 新增資料檔：在 `manifest.json` 的 `files` 加入 `{"path":"新檔.json"}`，再執行上面的程式。
4. 本機預覽不能直接雙擊 index.html，請在專案資料夾執行 `python -m http.server 8000`，再開 http://localhost:8000 。
