# Changelog

All notable changes to the **Shopping Agent Protocol (SAP)** specification will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2025-12-29 (Draft)

### Added
- Initial draft specification (v0.1.0) released
- Core architecture and conformance rules defined in `specification.md`
- Introduction and motivation document (`introduction.md`)
- Detailed tools reference (`tools.md`) with:
  - Required tools: `search_products`, `get_product_details`, `get_genres`, `get_ranking`
  - Recommended tools: `generate_affiliate_link`, `get_user_points_balance`
  - Optional tool: `get_personalized_recommendations`
  - Future tool placeholders (cart management, checkout)
- Authentication and security guidelines (`authentication.md`) including:
  - Mandatory OAuth 2.0 Authorization Code Flow with PKCE
  - Defined scopes: `shopping.read`, `shopping.points`, `shopping.personalize`
  - Security best practices and example configuration
- Example requests and responses in `examples/` folder covering all major tools
- README with project overview, badges, documentation links, and license information
- Apache License 2.0 applied to the repository

### Changed
- N/A (initial release)

### Deprecated
- N/A

### Removed
- N/A

### Fixed
- N/A

### Security
- Established privacy-first design requiring explicit OAuth consent for all personalized operations

## Planned for Future Versions

### v0.2.0
- Add write-capable tools (e.g., `add_to_cart`, `get_cart`, `checkout_initiate`)
- Define deep-link or embedded checkout patterns
- Multi-merchant aggregation patterns
- Enhanced error codes and messaging

### v1.0.0
- Stabilize core tools and authentication
- Reference implementations (Rakuten, Shopify, etc.)
- Official registry for SAP-compliant merchants

---

This changelog will be updated as the specification evolves with community feedback and real-world implementations.

