# Neper News API – Quick Start

Query news by country and optional section. Both GET and POST can return one, several, or all available sections in a single request. Each article includes a headline, source URL, and image and content fields when available.

## Base URL and Authentication

Use the gateway where you subscribed. Send your API key in a request header and keep it out of the URL and JSON body.

**RapidAPI**

```text
Base URL: https://neper-news-api.p.rapidapi.com
X-RapidAPI-Key: <YOUR_KEY>
X-RapidAPI-Host: neper-news-api.p.rapidapi.com
```

**JoJ API**

Use the base URL shown on the API listing and send `X-JoJAPI-Key: <YOUR_KEY>` with each request.

## Endpoint

```text
GET  /queryNews
POST /queryNews
```

Use query parameters for GET and a JSON body for POST.

## Parameters

- **`country`** is required. The API supports 13 country codes: `en`, `ko`, `au`, `nz`, `it`, `ja`, `gb`, `fr`, `de`, `in`, `ca`, `es`, `br`. The server trims whitespace and normalizes the code to lowercase.
- **`section`** is optional. Omit it to query all available sections for the country. Pass one section or a comma-separated string such as `BUSINESS,SPORTS` for both GET and POST. Section codes are case-insensitive, and the server removes duplicates. For a JSON array or repeated query parameter, the server reads the first non-empty value.
- All countries support `top`, `BUSINESS`, `ENTERTAINMENT`, `SPORTS`, `WORLD`, and `HEALTH`.
- `it`, `ko`, `ja`, `fr`, `de`, `es`, and `br` also support `SCITECH`.
- `en`, `au`, `nz`, `gb`, `in`, and `ca` also support `SCIENCE` and `TECHNOLOGY`.

## Request Examples

**GET using curl**

```bash
curl -G 'https://neper-news-api.p.rapidapi.com/queryNews' \
  --data-urlencode 'country=ko' \
  --data-urlencode 'section=top' \
  -H 'X-RapidAPI-Key: <YOUR_KEY>' \
  -H 'X-RapidAPI-Host: neper-news-api.p.rapidapi.com'
```

**POST using Axios**

```javascript
import axios from "axios";

const { data } = await axios.post(
  "https://neper-news-api.p.rapidapi.com/queryNews",
  { country: "en", section: "top,BUSINESS" },
  {
    headers: {
      "X-RapidAPI-Key": "<YOUR_KEY>",
      "X-RapidAPI-Host": "neper-news-api.p.rapidapi.com"
    }
  }
);

console.log(data);
```

## Response

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
            "content": "Article body",
            "faviconUrl": "https://example.com/favicon.ico",
            "img": "https://example.com/image.jpg",
            "section": "top",
            "timeStamp": 1757116800000,
            "title": "Headline",
            "updatedAt": "2025-09-06T00:00:00.000Z",
            "url": "https://example.com/article"
          }
        ]
      }
    ]
  }
}
```

- `data.sections` contains the normalized section filters. An empty array means all available sections were requested.
- `data.count` counts the returned section documents. Each document has its own `news` array.
- The section document's `updatedAt` is a Unix timestamp in milliseconds. News sync is scheduled every five minutes.
- Each article's `title` and `updatedAt` are strings. Missing text fields default to `""`, and a missing article `section` uses the containing document's section ID. Article `timeStamp` is a Unix timestamp in milliseconds or `null`.
- The JSON example shows all eight fields returned for each article.

## Errors

- **400** with `errorCode: "INVALID_REQUEST"` means the country is missing or the country or section code is invalid. For invalid codes, `message` lists the allowed values.
- **401** from the gateway indicates an authentication or subscription problem.
- **403** with `errorCode: "FORBIDDEN"` means the API could not authenticate the gateway request.
- **402** from JoJ API indicates a billing issue, such as an exhausted quota.
- **429** from RapidAPI indicates a rate limit or quota limit.
- **500** with `errorCode: "INTERNAL_SERVER_ERROR"` indicates an unexpected server error.

## Common Recipes

- All available Canadian sections with `GET /queryNews?country=ca`.
- Science and technology in Spain with `GET /queryNews?country=es&section=SCITECH`.
- Sports and business in one POST with `{"country":"gb","section":"SPORTS,BUSINESS"}`.

## Support / Region Requests

Contact **david@hadfamily.com**.
