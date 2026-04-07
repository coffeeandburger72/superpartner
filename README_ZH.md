<div align="center">

# superpartner

**我終於送了一份她會開心的禮物。**

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

匯入一份聊天紀錄，建立你伴侶的完整檔案 — 性格分析、興趣圖譜、重要日子、禮物推薦、約會企劃、關係洞察。所有建議都來自你們的真實對話，有根有據。

[安裝](#安裝) | [運作方式](#運作方式) | [使用情境](#使用情境) | [指令一覽](#指令一覽) | [隱私保護](#隱私保護) | [**English**](README.md)

</div>

---

你一定經歷過 — 她生日下星期到，腦袋一片空白。想約她出去，但忘了她到底是想試那間新餐廳還是陶藝班。又或者你們異地戀，快要想不到新花樣了。

**superpartner 讀你們的聊天紀錄，建立一個真正了解她的檔案。** 不是敷衍的「送花就好」，而是根據她說過的話、展現過的興趣、讓她眼睛發亮的事物，給出實在的建議。

---

## 安裝

**需要：** [Claude Code](https://docs.anthropic.com/en/docs/claude-code)

打開 Claude Code，貼上這段：

> Install superpartner: run `git clone https://github.com/coffeeandburger72/superpartner.git ~/.claude/skills/superpartner`

或自己執行：

```bash
git clone https://github.com/coffeeandburger72/superpartner.git ~/.claude/skills/superpartner
```

完成。在任何 Claude Code 對話中輸入 `/superpartner` 即可使用。

---

## 運作方式

```
你：     /superpartner
Claude:  你伴侶叫什麼名字？
你：     小雪
Claude:  你們在一起多久，目前什麼階段？
你：     交往半年，異地
Claude:  請提供聊天紀錄檔案。
你：     ~/Downloads/WhatsApp Chat - 小雪.zip

Claude:  找到 3,241 則訊息，日期從一月到七月，平台為 WhatsApp。
         開始分析。

         ... 分析 6 個維度的性格特徵 ...
         ... 從訊息和相片中提取興趣 ...
         ... 擷取日期、里程碑和即將到來的活動 ...
         ... 建立關係動態分析 ...

         完成！以下是重點發現：

         性格：溫暖體貼、喜歡提前計劃、愛用貼圖和語音、
         夜貓子型，晚上 9 點後才打開話匣子

         主要興趣：行山、日本料理、陶藝、獨立電影

         下個重要日子：交往半年紀念日（7月15日）

         首選禮物：雙人陶藝體驗班 — 她上個月提過三次
         想試拉坯

         隨時可用 /superpartner gifts 小雪、/superpartner dates 小雪
         或 /superpartner ask 小雪 <問題> 查詢更多。
```

---

## 使用情境

### 「她生日就快到了，完全唔知買咩好」

```
/superpartner gifts 小雪
```

根據她的興趣、聊天中提過的願望清單、以及她的愛之語言，重新生成個人化禮物建議。每個推薦都有她親口說過或展示過興趣的證據。

---

### 「呢個週末想安排啲特別嘅」

```
/superpartner dates 小雪
```

根據她的喜好、你們的關係階段和所在地生成約會提案。異地戀？自動調整 — 線上電影之夜、寄送驚喜包裹、下次見面的倒數活動。

---

### 「最近覺得她怪怪的，唔知點解」

```
/superpartner guide 小雪
```

你的關係指南 — 溝通模式、衝突處理風格、情緒需求，以及如何根據她的實際溝通方式來處理敏感對話。不是教科書建議，是從你們真實互動中提煉的模式。

---

### 「她到底鍾唔鍾意食壽司？」

```
/superpartner ask 小雪 她鍾唔鍾意食壽司？
```

問任何關於你伴侶的問題。回答基於檔案數據，附上證據 — 原文引用、相片參考、對話中的行為模式。

---

### 「我哋啱啱旅行完，想更新佢嘅檔案」

```
/superpartner update 小雪
```

匯入新的聊天紀錄。檔案會智能合併 — 新數據豐富現有分析，不會覆蓋舊結論。禮物推薦和約會企劃自動根據新資料重新生成。

---

### 「唔想再忘記佢媽咪生日」

建立檔案後，superpartner 會提供即將到來的日子的提醒設定 — 生日、紀念日、旅行、活動。提前兩星期和三日前各收到一次提醒，附上首選禮物建議。

---

## 指令一覽

| 指令 | 功能 |
|------|------|
| `/superpartner` | 從聊天紀錄建立新的伴侶檔案 |
| `/superpartner list` | 查看所有檔案 |
| `/superpartner update <名>` | 用新聊天紀錄更新現有檔案 |
| `/superpartner gifts <名>` | 重新生成禮物建議 |
| `/superpartner dates <名>` | 重新生成約會企劃 |
| `/superpartner guide <名>` | 重新生成關係指南 |
| `/superpartner ask <名> <問題>` | 問任何關於伴侶的問題 |
| `/superpartner delete <名>` | 完全刪除檔案 |

---

## 生成內容

每位伴侶會建立 6 個檔案：

| 檔案 | 內容 |
|------|------|
| **persona.md** | 6 層性格分析 — 溝通風格、情緒模式、價值觀、依附傾向，附原文引用 |
| **interests.md** | 分類整理的興趣、飲食偏好、嗜好、願望清單 — 每項標註發現方式 |
| **occasions.md** | 生日、紀念日、即將到來的活動、90 天禮物行事曆附準備時程 |
| **gifts.md** | 個人化禮物推薦，按契合度排名，根據愛之語言和關係階段調整 |
| **dating_ideas.md** | 按類型分類的約會企劃，異地戀有專屬方案 |
| **relationship_guide.md** | 溝通手冊 — 如何處理困難對話、情緒需求、需要注意的地方 |

每項發現都有證據標記：`(explicit)` 直接陳述、`(inferred)` 行為模式推斷、`(photo)` 相片分析。檔案以聊天語言撰寫。

---

## 支援平台

| 平台 | 匯出格式 | 匯出方式 |
|------|---------|---------|
| **WhatsApp** | `.zip` | 對話 > 更多 > 匯出對話（含媒體）|
| **WeChat** | `.csv` 或文字 | 使用第三方匯出工具 |
| **LINE** | `.txt` | 設定 > 聊天 > 備份聊天紀錄 |
| **任何平台** | 貼上文字 | 直接複製貼上對話內容 |

---

## 隱私保護

你的數據永遠留在你的電腦上。

- 所有伴侶檔案儲存在本地 `partners/` 資料夾，預設已加入 gitignore
- 聊天紀錄絕不會被提交、上傳或傳送到任何地方
- 無遙測、無分析追蹤、無雲端同步
- `delete` 指令徹底清除所有痕跡 — 檔案、排程提醒、記憶條目

---

## 授權

MIT 授權。永久免費。
