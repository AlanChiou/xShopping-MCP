# Specification

**Shopping Agent Protocol (SAP)**  
**Version 0.1.0 (Draft)**  
**December 2025**

## Abstract

The Shopping Agent Protocol (SAP) defines a standardized way for AI agents to interact with e-commerce platforms using the Model Context Protocol (MCP) as its foundation.  
It enables AI assistants (such as Grok, Claude, ChatGPT, Gemini, etc.) to discover products, retrieve detailed information, provide personalized recommendations, and facilitate purchases — all within the context of a natural conversation.

SAP is deliberately lightweight and read-focused in its initial version, emphasizing discovery and personalization while remaining compatible with existing merchant APIs and security constraints.

## Status of This Document

This is a **draft** specification. Implementations and feedback are encouraged. The protocol is expected to evolve based on real-world adoption, especially from major platforms like Rakuten Ichiba, Amazon, Shopify, and others.

## Introduction

Modern e-commerce platforms expose rich product catalogs, ranking systems, point/loyalty programs, and personalization engines. However, AI agents currently lack a unified, secure way to access these capabilities.

SAP addresses this by:
- Defining a common set of tools built on MCP
- Standardizing request and response formats
- Requiring OAuth 2.0-based user authorization for personalized data
- Prioritizing merchant control and user privacy

The protocol is designed to work seamlessly in environments like X (formerly Twitter), messaging apps, voice assistants, and dedicated AI interfaces.

## Conformance

A SAP server MUST implement all **Required** tools defined in the [Tools Reference](tools.md).  
A SAP server SHOULD implement **Recommended** tools where feasible.  
All communication MUST follow the underlying MCP transport and authentication rules.

## Architecture Overview

SAP follows the client-server model defined by MCP:

1. The AI agent (MCP client) discovers or is configured with a SAP server endpoint.
2. When a user authorizes access, the AI initiates an OAuth 2.0 flow with the merchant's authorization server.
3. Upon successful authorization, the AI establishes a stateful MCP connection (typically over Server-Sent Events).
4. The AI calls SAP tools as needed during the conversation.
5. The SAP server executes tools using the authenticated user context and returns structured results.

## Transport

SAP uses the standard MCP transport:
- Server-Sent Events (SSE) over HTTPS for persistent, stateful connections
- JSON payloads as defined by MCP

Servers MUST expose an `/sse` endpoint and provide MCP-compliant metadata.

## Authentication and Authorization

SAP REQUIRES user consent for any personalized operation.

### OAuth 2.0 Scopes

Merchants MUST support the following scopes:

- `shopping.read` – Required for all read operations (search, details, ranking, genres)
- `shopping.points` – Recommended for accessing user point balance and maximized affiliate links
- `shopping.personalize` – Optional for history-based recommendations

Servers MAY define additional granular scopes.

### Token Usage

The access token obtained via OAuth MUST be used to identify the user when executing tools that require personalization.

## Tool Discovery

SAP servers MUST include the following in their MCP metadata:

```json:disable-run
{
  "name": "Shopping Agent Protocol",
  "description": "E-commerce discovery and purchasing tools",
  "version": "0.1.0",
  "tools": [ /* list of available tool names */ ]
}
```

## Tools

All tools are defined in detail in the separate [Tools Reference](tools.md) document.

### Required Tools

- `search_products`
- `get_product_details`
- `get_genres`
- `get_ranking`

### Recommended Tools

- `generate_affiliate_link`
- `get_user_points_balance`

### Optional Tools

- `get_personalized_recommendations`

### Future Tools

Write-capable tools (cart management, checkout) will be added in future versions when secure patterns emerge.

## Error Handling

SAP servers MUST follow MCP error conventions and include a clear, human-readable `error_message` field when appropriate.

## Security Considerations

- All connections MUST use HTTPS
- Servers MUST validate and sanitize all inputs
- Rate limiting SHOULD be applied per user and per IP
- Personal data MUST only be returned after valid OAuth authorization
- Affiliate links SHOULD be generated server-side to prevent tampering

## Extensibility

Merchants MAY add custom tools prefixed with `x_` (e.g., `x_rakuten_coupon_apply`).  
Future versions of SAP will incorporate widely adopted extensions into the core specification.

## References

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
- [Tools Reference](tools.md)
- [Authentication & Security](authentication.md) *(planned)*

## Acknowledgments

This specification is inspired by real-world needs observed on platforms like Rakuten Ichiba, Shopify, and Amazon, and by the broader adoption of agentic commerce in 2025.

Contributions from the open-source community are welcome and encouraged.

---

End of Specification v0.1.0 (Draft)
