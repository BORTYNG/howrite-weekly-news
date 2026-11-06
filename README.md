# howrite-weekly-news

Howrite 每週時事字卡的資料。App 透過 GitHub Pages 讀取 `weekly_news.json`：

```
https://bortyng.github.io/howrite-weekly-news/weekly_news.json
```

## 檔案

| 檔案 | 用途 |
|---|---|
| `weekly_news.json` | 這週的字卡，App 讀這份。第一次 Merge 之前還不存在：App 拿到 404 時，當成「還沒有字卡」。 |
| `archive/<週別>.json` | 每週存一份，格式跟 `weekly_news.json` 一樣，例如 `archive/2026-W46.json`。 |
| `.nojekyll` | 讓 GitHub Pages 照原樣提供檔案。 |

## 怎麼更新

每週一早上，程式自動開一個 PR（分支 `week-<週別>`），同時更新 `weekly_news.json`、新增 `archive/<週別>.json`。維護者看過字卡、按 Merge 之後才會上線，GitHub Pages 大約一分鐘內更新。這週沒有 Merge 的話，App 繼續顯示上一份。

## JSON 格式（`schema_version: 1`）

```json
{
  "schema_version": 1,
  "week": "2026-W46",
  "week_label": "11/9 這週",
  "published_at": "2026-11-09T07:30:00+08:00",
  "items": [
    {
      "id": "2026-W46-1",
      "title": "原文標題",
      "source": "報導者",
      "url": "https://www.twreporter.org/a/...",
      "tags": ["資訊", "科技"],
      "summary": "我們自己寫的摘要，約 100 字",
      "concept": "讓數據告訴你「你是誰」",
      "question": "數據呈現出來的你，和你自己認識的你，哪一個比較真實？",
      "sides": [
        "數據不會說謊，能讓你看見自己沒發現的習慣",
        "數據只記得你做了什麼，不知道你為什麼這樣做"
      ],
      "practice": { "year": 113, "question_id": "gsat-113" }
    }
  ]
}
```

| 欄位 | 型別 | 說明 |
|---|---|---|
| `schema_version` | 整數 | 目前是 `1`。格式有不相容的改變時才加 1；只新增欄位不加。 |
| `week` | 字串 | ISO 週別，例如 `2026-W46`。 |
| `week_label` | 字串 | 給人看的週名，取那週的週一，例如 `11/9 這週`。 |
| `published_at` | 字串 | 產生這份資料的時間（ISO 8601，含 `+08:00` 時區）。 |
| `items` | 陣列 | 1～3 張字卡。 |
| `items[].id` | 字串 | `<週別>-<第幾張>`，例如 `2026-W46-1`。 |
| `items[].title` | 字串 | 原文標題。 |
| `items[].source` | 字串 | 來源名稱，例如「報導者」。 |
| `items[].url` | 字串 | 原文網址，一定是 `https://`。 |
| `items[].tags` | 字串陣列 | 1～3 個議題，只會是這 14 個：科技、資訊、生命、品德、人權、家庭教育、生涯規劃、多元文化、性別平等、環境、安全、防災、法治、國際教育。 |
| `items[].summary` | 字串 | 自己寫的摘要，最多 130 字。 |
| `items[].concept` | 字串 | 背後的概念，最多 20 字。 |
| `items[].question` | 字串 | 可能怎麼問，最多 40 字。 |
| `items[].sides` | 字串陣列 | 兩邊可以怎麼想，剛好 2 句，每句最多 45 字。 |
| `items[].practice` | 物件或 `null` | 延伸練習。沒有時是 `null`；有的話是 `{ "year": 108～115, "question_id": "gsat-<年份>" }`，`question_id` 對應 App 歷屆題的 id。 |

字數上限把標點也算成一個字。
