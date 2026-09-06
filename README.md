# Neper News API – Quick Start

## What is it

Query current news by country code and optional section. GET or POST. One request can return a single section or every section for that country. Articles include headlines, links, images, and AI summaries for supported locales.

---

## Spotlights

- Ten countries: `en`, `ko`, `au`, `nz`, `it`, `ja`, `gb`, `fr`, `de`, `in`
- GET or POST in one call, one section or every section
- Headlines, links, images, and AI summaries for `de`, `en`, and `ko`

---

## Base URL

```
https://neper-news-api.p.rapidapi.com
```

---

## Authentication

Subscribe to a plan, then send your RapidAPI key on every request. Do not put the key in the URL or JSON body.

```
X-RapidAPI-Key: <YOUR_KEY>
X-RapidAPI-Host: neper-news-api.p.rapidapi.com
```

---

## Endpoint

```
GET  /queryNews
POST /queryNews
```

---

## Parameters

- **country** – Required. Lowercase country/language code.  
  Allowed: `en`, `ko`, `au`, `nz`, `it`, `ja`, `gb`, `fr`, `de`, `in`

- **section** – Optional. Omit to return every section for that country.  
  Common: `top`, `BUSINESS`, `ENTERTAINMENT`, `SPORTS`, `WORLD`, `HEALTH`  
  `it`, `ko`, `ja`, `fr`, `de` use `SCITECH`.  
  `en`, `au`, `nz`, `gb`, `in` use `SCIENCE` and `TECHNOLOGY`.  
  `TOP` is treated as `top`.  
  GET: comma-separated list, for example `BUSINESS,SPORTS`.  
  POST: string, comma-separated string, or JSON array.

---

## Request Examples

**GET**

```
GET /queryNews?country=ko
GET /queryNews?country=ko&section=top
```

**curl**

```bash
curl -G 'https://neper-news-api.p.rapidapi.com/queryNews' \
  --data-urlencode 'country=ko' \
  --data-urlencode 'section=top' \
  -H 'X-RapidAPI-Key: <YOUR_KEY>' \
  -H 'X-RapidAPI-Host: neper-news-api.p.rapidapi.com'
```

**JavaScript (axios)**

```javascript
import axios from "axios";

const res = await axios.get("https://neper-news-api.p.rapidapi.com/queryNews", {
  params: { country: "ko", section: "top" },
  headers: {
    "X-RapidAPI-Key": "<YOUR_KEY>",
    "X-RapidAPI-Host": "neper-news-api.p.rapidapi.com"
  }
});

console.log(res.data);
```

**POST**

```javascript
const res = await axios.post(
  "https://neper-news-api.p.rapidapi.com/queryNews",
  { country: "en", section: ["top", "BUSINESS"] },
  {
    headers: {
      "X-RapidAPI-Key": "<YOUR_KEY>",
      "X-RapidAPI-Host": "neper-news-api.p.rapidapi.com"
    }
  }
);
```

---

## Response Shape

```json
{
  "status": "success",
  "message": "Request successful",
  "data": {
    "country": "ko",
    "sections": ["top"],
    "count": 1,
    "items": [
      {
        "docId": "kotop",
        "code": "ko",
        "id": "top",
        "updatedAt": 1757116800000,
        "news": [
          {
            "title": { "_text": "Headline" },
            "link": { "_text": "https://news.google.com/rss" },
            "url": "https://example.com/article",
            "img": "https://example.com/image.jpg",
            "content": "Article body",
            "summary": "One-sentence summary",
            "section": "top"
          }
        ]
      }
    ]
  }
}
```

- `data.sections` is empty when every section was returned.
- `updatedAt` is a Unix timestamp in milliseconds.
- AI `summary` is filled for `de`, `en`, and `ko`. Other countries return an empty summary.
- News is synced about every 5 minutes.

---

## Errors

- **400** – Invalid or missing `country` / `section`. `errorCode` is `INVALID_REQUEST`. Allowed values are in `message`.
- **401** – RapidAPI key missing, invalid, or no active subscription.
- **429** – Rate limit or quota exceeded.
- **5xx** – Upstream or transient error.

---

## Common Recipes

1. **Country front page**

   ```
   GET /queryNews?country=en
   ```

2. **Science/Tech for Korean market**

   ```
   GET /queryNews?country=ko&section=SCITECH
   ```

3. **Sports and business in one POST**

   ```json
   { "country": "gb", "section": ["SPORTS", "BUSINESS"] }
   ```

---

## Support / Region Requests

Want another region?  
Contact: **david@hadfamily.com**
