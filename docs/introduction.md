# Introduction

## What is the Shopping Agent Protocol (SAP)?

The **Shopping Agent Protocol (SAP)** is an open, lightweight protocol that allows AI agents to interact with e-commerce platforms in a standardized, secure, and user-friendly way.

Built on top of the [Model Context Protocol (MCP)](https://modelcontextprotocol.io), SAP defines a common set of tools and data formats that enable AI assistants—such as Grok, Claude, ChatGPT, Gemini, or any future agent—to help users discover products, compare options, view personalized pricing and rewards, and complete purchases, all within a natural conversation.

With SAP, shopping becomes as simple as talking to an AI.

## Motivation

In 2025, AI agents are becoming central to daily life. Users increasingly turn to conversational interfaces on platforms like X, messaging apps, and voice assistants for recommendations and tasks.

E-commerce platforms already have powerful discovery features:
- Rich product catalogs
- Real-time rankings and trends
- Loyalty and point systems (e.g., Rakuten Points)
- Personalization engines

However, there is no unified way for AI agents to access these capabilities securely and consistently across merchants.

SAP solves this by providing a standard interface that merchants can implement once, making their platform instantly compatible with any SAP-enabled AI agent.

## Key Benefits

### For Users
- Shop without leaving the conversation
- Get personalized results (member pricing, points earned, recommendations)
- Discover products through natural language, rankings, or categories
- Make informed decisions with clear pricing, reviews, and reward information

### For Merchants (e.g., Rakuten, Shopify, Amazon partners)
- Reach users directly in AI-driven environments
- Increase conversion through seamless, low-friction experiences
- Leverage existing APIs (no need for full write access initially)
- Gain visibility in new AI-native traffic channels

### For AI Developers
- One protocol works across multiple e-commerce platforms
- Structured, predictable responses
- Secure user authorization via OAuth 2.0

## Design Principles

- **Read-first, write-later**: v0.1 focuses on discovery and information retrieval—features most merchants can safely expose today.
- **Privacy by design**: Personal data (points, history, recommendations) requires explicit user consent via OAuth.
- **Platform-agnostic**: Works with marketplace models (Rakuten Ichiba), direct-to-consumer brands, and headless commerce systems.
- **Extensible**: Merchants can add custom tools; popular extensions can be promoted to core in future versions.

## How It Works (High-Level Flow)

1. A user talks to an AI agent (e.g., Grok on X):  
   “Find me a black down jacket under 6,000 JPY with high point cashback.”

2. The AI connects to a SAP-enabled merchant server (e.g., Rakuten Ichiba).

3. If personalization is needed, the user authorizes access via OAuth.

4. The AI calls SAP tools (`search_products`, `get_ranking`, `generate_affiliate_link`, etc.).

5. Results are shown in-chat with images, prices, expected points, and a ready-to-click affiliate link.

6. User taps the link → completes checkout on the merchant site (secure and familiar).

## Current Status

SAP v0.1.0 is a **draft specification** released in December 2025.  
Early implementations are encouraged, especially from platforms with strong discovery APIs like Rakuten Ichiba.

The protocol will evolve based on real-world feedback, with future versions potentially adding cart management, direct checkout, and multi-merchant aggregation.

## Getting Started

- **Merchants**: Read the [Specification](specification.md) and [Tools Reference](tools.md) to implement a SAP server.
- **AI Developers**: Integrate MCP client support and request SAP endpoints from partner merchants.
- **Contributors**: All feedback, examples, and pull requests are welcome!

---

Let's build the future where shopping is just a conversation. 🚀
