# Tools Reference

This document defines the standard tools that a **Shopping Agent Protocol (SAP)** server MUST, SHOULD, or MAY expose to enable AI agents to perform shopping-related tasks on e-commerce platforms.

SAP builds directly on the [Model Context Protocol (MCP)](https://modelcontextprotocol.io) tool-calling mechanism and follows its JSON-based request and response format.

Tools are categorized as:

- **Required** – Must be implemented for basic SAP compliance
- **Recommended** – Strongly encouraged for a rich shopping experience
- **Optional** – Implementation depends on platform capabilities

All tool responses MUST be valid JSON objects wrapped in MCP's `content` array.

## Required Tools

### search_products

Searches the merchant's product catalog. Essential for keyword and filtered discovery.

**Parameters** (JSON Schema)

```json:disable-run
{
  "type": "object",
  "properties": {
    "query": { "type": "string", "description": "Search keywords" },
    "genre_id": { "type": "string", "description": "Category/genre ID to narrow scope" },
    "min_price": { "type": "number" },
    "max_price": { "type": "number" },
    "sort": { "type": "string", "description": "e.g., '+price', '-point_rate', 'relevance'" },
    "page": { "type": "integer", "minimum": 1, "default": 1 },
    "hits": { "type": "integer", "minimum": 1, "maximum": 100, "default": 20 }
  },
  "required": ["query"]
}
```

**Example Response Fields**

- `results[]`: array of products with `item_code`, `name`, `price`, `currency`, `point_rate`, `point_multiplier`, `image_urls[]`, `item_url`, `affiliate_url`, `availability`, `review_score`, `review_count`
- `total_hits`, `page`, `hits`

### get_product_details

Fetches full details for a single product.

**Parameters**

```json
{
  "type": "object",
  "properties": {
    "item_code": { "type": "string" },
    "item_url": { "type": "string", "description": "Alternative if item_code unknown" }
  },
  "required": ["item_code"]
}
```

**Key Response Fields**

description, images[], specs (object), shipping_info, etc.

### get_genres

Returns the category hierarchy for guided browsing (critical for platforms like Rakuten).

**Parameters**

```json
{
  "type": "object",
  "properties": {
    "parent_genre_id": { "type": "string", "description": "Omit or '0' for root" }
  }
}
```

**Response**

Array of { genre_id, name, children[] }

### get_ranking

Retrieves real-time or periodic ranking lists – a core discovery feature on Rakuten.

**Parameters**

```json
{
  "type": "object",
  "properties": {
    "genre_id": { "type": "string" },
    "age": { "type": "string", "enum": ["10s", "20s", "30s", "40s", "50s", "60s"] },
    "sex": { "type": "string", "enum": ["men", "women", "all"] },
    "period": { "type": "string", "enum": ["realtime", "daily"] }
  }
}
```

## Recommended Tools

### generate_affiliate_link

Creates a personalized affiliate URL that maximizes Rakuten Points cashback.

**Parameters**

```json
{
  "type": "object",
  "properties": {
    "item_url": { "type": "string" }
  },
  "required": ["item_url"]
}
```

**Response Example**

```json
{
  "affiliate_url": "https://item.rakuten.co.jp/.../?scid=af_...",
  "expected_points": 1200,
  "campaign_info": "20× points campaign active"
}
```

### get_user_points_balance

Returns the authenticated user's current Rakuten Points balance.

**Parameters**: None (uses OAuth context)

**Response Example**

```json
{
  "total_balance": 15420,
  "regular_points": 12000,
  "limited_points": [
    { "amount": 3420, "expiry_date": "2026-03-31" }
  ]
}
```

## Optional Tools

### get_personalized_recommendations

Returns recommendations based on user purchase/browse history.

**Parameters**

```json
{
  "type": "object",
  "properties": {
    "limit": { "type": "integer", "default": 10 }
  }
}
```

## Future Tools (Planned)

- add_to_cart
- get_cart
- checkout_initiate
- track_order

These will be standardized when merchants provide write-capable APIs or secure deep-link flows.

## Error Handling

Tools MUST follow MCP error conventions and include a clear `error_message` when possible.

Common codes: 400 (bad request), 401 (unauthorized), 404 (not found), 429 (rate limited), 500 (server error).

---

Contributions of new tools, refinements, or platform-specific examples (Rakuten, Amazon, Shopify, etc.) are welcome!
```
