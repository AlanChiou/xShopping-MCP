# Authentication & Security

This document describes the authentication and authorization mechanisms required for the **Shopping Agent Protocol (SAP)**.

SAP relies on **OAuth 2.0** to ensure secure, user-consented access to personalized data. No direct credentials are ever shared with the AI agent.

## Why OAuth 2.0?

- Users must explicitly authorize the AI agent to access their account data.
- Merchants retain full control over scopes and data exposure.
- Aligns with industry standards (Google, Rakuten, Shopify, Amazon all support OAuth).
- Prevents prompt injection or unauthorized access.

## OAuth 2.0 Flow (Recommended: Authorization Code with PKCE)

SAP servers MUST support the **Authorization Code Flow with PKCE** (Proof Key for Code Exchange) for maximum security, especially for public clients like AI agents.

### Step-by-Step Flow

1. **Discovery**  
   The AI agent (MCP client) discovers the SAP server's OAuth endpoints via the MCP metadata or a well-known configuration URL (e.g., `https://merchant.com/.well-known/sap-oauth.json`).

2. **User Consent**  
   When the AI needs personalized access (e.g., points balance, member pricing), it initiates OAuth:  
   - Redirect user to merchant's authorization endpoint with parameters:  
     - `client_id` (AI agent's registered client ID)  
     - `redirect_uri` (AI's callback, e.g., `https://grok.x.ai/callback`)  
     - `scope` (space-separated, e.g., `shopping.read shopping.points`)  
     - `state` (CSRF token)  
     - `code_challenge` & `code_challenge_method` (for PKCE)  
   - User logs in to merchant (Rakuten login) and grants consent.

3. **Token Exchange**  
   Merchant redirects back to AI agent's `redirect_uri` with `code` and `state`.  
   AI agent exchanges `code` for `access_token` and `refresh_token` at merchant's token endpoint.

4. **Using the Token**  
   The AI agent includes the `access_token` in MCP requests (via `Authorization: Bearer <token>` header).  
   SAP server validates the token and associates it with the user's merchant account.

5. **Token Refresh & Revocation**  
   - AI SHOULD refresh tokens using `refresh_token` when expired.  
   - Users can revoke access via merchant's account settings (standard OAuth revocation endpoint).

## Required OAuth Scopes

Merchants MUST support at least these scopes:

| Scope                     | Description                                                                 | Required for Tools                  |
|---------------------------|-----------------------------------------------------------------------------|-------------------------------------|
| `shopping.read`           | Read access to public and user-specific product data                        | All required tools                  |
| `shopping.points`         | Access to user's Rakuten Points balance and maximized affiliate links       | `generate_affiliate_link`, `get_user_points_balance` |
| `shopping.personalize`    | Access to user history for personalized recommendations                     | `get_personalized_recommendations`  |

Additional scopes MAY be defined for future write operations (e.g., `shopping.cart.write`).

## Security Best Practices

- **HTTPS Only** – All endpoints MUST use TLS 1.2+.
- **PKCE Mandatory** – Protects against code interception.
- **Short-lived Tokens** – Recommend access tokens expire in 1 hour; refresh tokens in 30 days.
- **Input Validation** – Sanitize all parameters to prevent injection attacks.
- **Rate Limiting** – Limit requests per user/IP to prevent abuse.
- **Token Storage** – AI agents MUST securely store tokens (never expose to users).
- **Consent UI** – Merchant MUST show clear consent screen explaining what data the AI will access.
- **Audit Logging** – Log token issuance and usage for security monitoring.

## Example OAuth Configuration (for Merchants)

Merchants can expose a `.well-known` endpoint:

```json:disable-run
{
  "authorization_endpoint": "https://auth.merchant.com/oauth/authorize",
  "token_endpoint": "https://auth.merchant.com/oauth/token",
  "revocation_endpoint": "https://auth.merchant.com/oauth/revoke",
  "scopes_supported": ["shopping.read", "shopping.points", "shopping.personalize"],
  "grant_types_supported": ["authorization_code", "refresh_token"]
}
```

## Integration with MCP

The `access_token` is passed in every MCP tool call via the standard `Authorization` header.  
SAP servers MUST validate the token before executing any tool that requires user context.

## Future Considerations

- **Device Flow** for voice assistants (no browser).
- **Federated Identity** (e.g., login with Google/Rakuten ID).
- **Write Operations** – When added, require additional consent and security reviews.

---

Secure authentication is the foundation of trust in agentic commerce. By using standard OAuth 2.0, SAP ensures privacy and safety for users and merchants alike.
