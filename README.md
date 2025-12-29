# Shopping Agent Protocol (SAP)  
An open protocol for AI agents to interact with e-commerce platforms securely and seamlessly.

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-blue)](https://github.com/yourusername/shopping-agent-protocol)
[![Spec Status: Draft](https://img.shields.io/badge/Spec%20Status-Draft-orange)](docs/specification.md)

## Overview

The **Shopping Agent Protocol (SAP)** is an open, lightweight protocol that enables AI assistants (such as Grok, Claude, ChatGPT, Gemini, etc.) to act as fully capable shopping agents on behalf of users.

By implementing a simple SAP server, any e-commerce platform can allow AI agents to:

- Search and browse products
- Retrieve personalized recommendations, pricing, and member-only offers
- Manage shopping carts
- Complete checkout and payment flows
- Track orders and returns

—all directly within the AI conversation, without redirects or leaving the host application (e.g., X, messaging apps, or dedicated AI interfaces).

SAP is heavily inspired by the [Model Context Protocol (MCP)](https://modelcontextprotocol.io) and extends its tool-calling and authentication patterns with e-commerce-specific tools, schemas, and security best practices.

## Why SAP?

- **Frictionless shopping experience**: Users complete purchases by simply talking to an AI.
- **Cross-model compatibility**: Works with any AI that supports structured tool calling.
- **Privacy-first design**: OAuth 2.0 authorization with scoped permissions; no sharing of raw credentials.
- **Merchant advantages**: Higher conversion rates, new AI-native traffic channels, and rich personalization opportunities.

## Goals

- Standardize a core set of e-commerce tools (search, product details, cart operations, checkout, order tracking).
- Provide clear JSON schemas, request/response examples, and reference implementations.
- Enable straightforward integration with existing membership, catalog, inventory, and payment systems.
- Drive broad adoption across platforms (Shopify, WooCommerce, Magento, custom stores, marketplace partners, etc.).

## Status

This specification is currently in **Draft** (v0.1.0).  
Community feedback, contributions, and early implementations are strongly encouraged!

## Documentation

- [Introduction](docs/introduction.md)
- [Specification](docs/specification.md) ← Core protocol definition
- [Authentication & Security](docs/authentication.md)
- [Tools Reference](docs/tools.md)
- [Examples](examples/)
- [Changelog](docs/changelog.md)

## Quick Start for Merchants

1. Set up a SAP server that exposes the required and optional tools.
2. Configure OAuth 2.0 endpoints for user authorization.
3. Test integration using any MCP-compatible client or reference tooling.

Full implementation details are available in the [Specification](docs/specification.md).

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to submit issues, propose changes, or add implementations.

## License

This project is licensed under the Apache License, Version 2.0.

See the [LICENSE](LICENSE) file for details.

---

Built with ❤️ for the future of agentic commerce.  
Let’s make shopping as natural as conversation. 🚀
