# Neper News API – Quick Start

## What is it
Real-time, multilingual news feed API. Pull top headlines by country and section with a single GET request. Output is normalized with links, image, favicon, and source meta.

---

## Base URL
```
https://neper-news-api.p.rapidapi.com/
```

---

## Authentication
Pass your RapidAPI key:

```
X-RapidAPI-Key: <YOUR_KEY>
X-RapidAPI-Host: neper-news-api.p.rapidapi.com
```

---

## Endpoint
```
GET /
```

---

## Query Parameters
- **country** – Required. Country/language code.  
  Supported: `en, ko, it, nz, ja, fr, gb, de, au, in`  

- **section** – Optional. One of:  
  `TOP, BUSINESS, ENTERTAINMENT, SCIENCE, SPORTS, TECHNOLOGY, WORLD`  
  *Note:* For `ko/ja/fr/it/de`, use **SCITECH** instead of SCIENCE/TECHNOLOGY  

- **limit** – Optional. Max items per section (default 20, max 50)  

---

## Request Examples

**curl**
```bash
curl -G 'https://neper-news-api.p.rapidapi.com/' \
  --data-urlencode 'country=en' \
  -H 'X-RapidAPI-Key: <YOUR_KEY>' \
  -H 'X-RapidAPI-Host: neper-news-api.p.rapidapi.com'
```

**JavaScript (axios)**
```javascript
import axios from "axios";

const res = await axios.get("https://neper-news-api.p.rapidapi.com/", {
  params: { country: "ko", section: "SCITECH", limit: 20 },
  headers: {
    "X-RapidAPI-Key": "<YOUR_KEY>",
    "X-RapidAPI-Host": "neper-news-api.p.rapidapi.com"
  }
});

console.log(res.data);
```

---

## Response Shape
```json
{
  "status": "success",
  "message": "",
  "data": [
    {
      "id": "BUSINESS",
      "code": "en",
      "updatedAt": 1705820575364,
      "news": [
        {
          "title": { "_text": "Headline..." },
          "link": { "_text": "https://news.google.com/rss/articles/..." },
          "description": { "_text": "<ol>...</ol>" },   // optional HTML
          "childJson": "...",                           // optional JSON string
          "url": "https://source.site/article",
          "baseUrlTitle": "Source Site Title",
          "faviconUrl": "https://source.site/favicon.ico",
          "img": "https://image.cdn/cover.jpg",
          "section": "BUSINESS"
        }
      ]
    }
  ]
}
```

---

## Sections
- TOP  
- BUSINESS  
- ENTERTAINMENT  
- SCIENCE  
- SPORTS  
- TECHNOLOGY  
- WORLD  

**Note:** For Korean/Japanese/French/Italian/German (`ko/ja/fr/it/de`), SCIENCE and TECHNOLOGY are merged as **SCITECH**.

---

## Errors
- **400** – Invalid or missing parameter (e.g., country)  
- **404** – Section not found for given country  
- **429** – Rate limit exceeded  
- **5xx** – Upstream or transient error  

---

## Caching
We set `Cache-Control` headers suitable for CDN use.  
If you need always-fresh data, set a **no-cache policy** on your side.

---

## Common Recipes
1. **Country front page**  
   ```
   GET /?country=en
   ```

2. **Science/Tech for Korean market**  
   ```
   GET /?country=ko&section=SCITECH
   ```

3. **Sports widget with 10 items**  
   ```
   GET /?country=gb&section=SPORTS&limit=10
   ```

---

## Use Cases
- Build a real-time news widget in web/mobile  
- Enrich dashboards with trustworthy headlines  
- Provide multilingual content surfaces for global users  

---

## Support / Region Requests
Want another region?  
📩 Contact: **david@hadfamily.com**
