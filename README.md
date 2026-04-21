# Interview Book API (pseudocode spec)

Base URL: `http://127.0.0.1:8000` (default). JSON everywhere unless noted.

Shared type:

```
Book = { id: number, title: string, author: string, year: number, available: boolean }
```

---

## `GET /api/books/:id`

**Params (path):**

- `id` — book id (number)

**Returns:** one `Book`

**Status:** `200` | `404` with `{ error: "Book not found" }`

---

## `POST /api/books`

**Params (JSON body):**

- `title` (string, required)
- `author` (string, required)
- `year` (number, required)
- `available` (boolean, optional) — defaults to `true` if omitted
- `catalog` (object, optional) — nested metadata, e.g. tags plus who/when it was acquired

**Example body (nested JSON):**

```json
{
  "title": "The Mythical Man-Month",
  "author": "Fred Brooks",
  "year": 1975,
  "available": true,
  "catalog": {
    "tags": ["classic", "management"],
    "acquisition": { "vendor": "HQ", "date": "2025-01-10" }
  }
}
```

*(Interview stub: may only persist the flat `Book` fields; `catalog` is here so candidates talk through nested JSON.)*

**Returns:** created `Book` (server assigns `id`)

**Status:** `201` | `400` with `{ error: "title, author, year required" }`

---

## `PATCH /api/books/:id`

**Params (path):** `id` — book id

**Params (JSON body):** partial object; any of `title`, `author`, `year`, `available` (same types as `Book` minus `id`)

**Returns:** merged `Book` (`id` unchanged; unspecified fields keep previous values)

**Status:** `200` | `404`

---

## `DELETE /api/books/:id`

**Params (path):** `id` — book id

**Returns:** empty body

**Status:** `204` | `404`
