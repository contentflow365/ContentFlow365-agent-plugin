---
name: contentflow-workflows
description: Help an authenticated ContentFlow365 customer discover features, inspect account content, and prepare or execute a content workflow through the ContentFlow365 MCP server with credit and consent checks.
---

# ContentFlow365 workflows

Use the `contentflow365` MCP connection. Treat its current tool catalog and authenticated results as the authority for available operations. Do not infer an operation from this package, a screenshot, or an old conversation. If the connection or a required scope is missing, ask the user to connect or reauthorize it. Never request credentials in chat.

## Discover and explain

For how-to questions, call `search_help_center` in the user's language. For a Persona journey, use `consult_product_guide` when its journey matches the request. Cite the article title and returned URL or identify the guide and its version. Distinguish a published capability from a proposed one. Do not invent account data from general help content.

For account content, use the relevant account-scoped list/detail tools. Resolve IDs through discovery; do not assume that a slug or URL grants access. Preserve full text, media URLs and pagination when returned. Show the server's app or Biblioteca link when available. A collection link is not an item detail link.

## Before a paid or external action

1. Discover the relevant tool and parameters. Consult `get_pricing_table` and the operation-specific estimate or quote tool for the exact current selection. Consult `get_credit_balance` for the authenticated account. If options change, quote again.
2. Present available balance, the current quote, what will be created or changed, and any external destination. Identify estimates as estimates. If a value is unavailable or uncertain, say so and do not fabricate it.
3. Obtain the user's explicit consent for this specific paid action, publication, send, or external mutation. Consent to connect the plugin is not consent to spend or publish.
4. Call the authorized server tool once with a stable idempotency key when its contract supports one. For an uncertain or pending result, query its status before considering a retry.
5. Report the server's actual consumption and updated balance when available. Distinguish reservation, pending charge and refund. Never calculate a final balance by subtracting an estimate.

OAuth scopes, account and brand ownership, balance checks, reservations, charging and idempotency are enforced by the server. Do not bypass them through another API or account. Do not imply that this skill itself grants permissions.

## Results and presentation

Use returned structured content and readable text. Provide direct app and Biblioteca links supplied by the server, and label collection fallbacks clearly. Do not claim an asset is in Biblioteca unless the server identifies its Library record or link. A generated media URL is not proof of cataloging.

If an MCP Apps view is offered by a tool, use it when helpful, while keeping the answer usable as text and links. Never invent a UI resource or embed a private asset in an arbitrary public page. When a client cannot display native image/audio/video blocks, use the returned HTTPS links without regenerating the asset.
