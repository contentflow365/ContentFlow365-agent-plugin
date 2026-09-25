# ContentFlow365 agent integration

One OAuth-protected remote MCP endpoint serves all clients:

`https://mcp.contentflow365.com/api/mcp/mcp?require_oauth=1`

Public setup guide: https://www.contentflow365.com/mcp
Spanish guide: https://www.contentflow365.com/es/mcp

The `require_oauth=1` connection challenges at the initial handshake. Never place a bearer token or client secret in this package. The server's OAuth consent and scopes remain authoritative. The package contains a portable Agent Plugin manifest, a Codex compatibility manifest, a Claude Code manifest, one shared skill, and a copy of the existing product icon for directory listings. Cursor recognizes the portable manifest. Grok currently documents a custom remote MCP connector rather than this package format.

For an OpenAI directory listing, use the same endpoint with `?tool_auth=1` in the portal after that server revision is deployed and verified. That mode advertises authentication per tool and returns an MCP OAuth challenge when a protected tool needs linking or wider scopes. The packaged manual-install URL above continues to use the initial HTTP challenge for existing clients. These are source instructions; neither mode establishes directory approval.

## Install for testing

| Client | Path | What is installed |
| --- | --- | --- |
| ChatGPT / Codex | Register the URL as a custom remote MCP app in developer mode; for packaged skill testing, add this folder to a local plugin marketplace or upload the package through the plugin authoring flow. Complete OAuth in the client. | Remote tools; skill when package is installed. |
| Claude / Claude Desktop | Customize → Connectors → Add custom connector, enter the URL, then connect the account. Team/Enterprise owners add the connector for their organization first. A custom plugin upload can carry the bundled skill. | Remote tools; skill when plugin is installed. |
| Claude Code | For a one-session plugin test, run `claude --plugin-dir /absolute/path/to/contentflow365`, then inspect `/plugin` and use `/mcp` to complete OAuth and confirm the server is connected. For tools without the packaged skill, run `claude mcp add --transport http contentflow365 'https://mcp.contentflow365.com/api/mcp/mcp?require_oauth=1'`, then `/mcp`. After marketplace publication, install the plugin by its published marketplace name. | Remote tools and skill for plugin loading; tools only for MCP add. |
| Cursor | Import the root Agent Plugin package or add the URL in Settings → MCP. The root `plugin.json` plus `mcp.json` and `skills/` are the Cursor-compatible package. | Remote tools and skill for plugin install; tools only for direct MCP. |
| Grok on grok.com | Connectors → New Connector → Custom, enter the URL and authenticate. Business/Enterprise administrators provision it for their team. | Remote tools. No bundled skill/package support established in the cited Grok connector contract. |

For the quickest Cursor connection, use [Add ContentFlow365 MCP to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=contentflow365&config=eyJ1cmwiOiJodHRwczovL21jcC5jb250ZW50ZmxvdzM2NS5jb20vYXBpL21jcC9tY3A/cmVxdWlyZV9vYXV0aD0xIn0%3D). Cursor must still prompt for OAuth. This link installs the MCP server only; the plugin skill is included when the full package is installed. For local package testing, copy the package folder to `~/.cursor/plugins/local/contentflow365`, reload Cursor and inspect Customize. The bare package is discovered there as one MCP server plus one skill; Customize → Add → From Local Repository instead expects a marketplace manifest and does not import a bare plugin folder. Cursor may require local plugin imports to be enabled by the workspace admin.

These are authoring and custom-connector paths. This package does not claim that any public directory has accepted or listed ContentFlow365. The OAuth callback, consent, tool discovery and account-specific results must be tested in each client before calling that path verified. Never test a paid generation merely to check installation.

### Codex CLI: direct connection

```sh
codex mcp add contentflow365 --url 'https://mcp.contentflow365.com/api/mcp/mcp?require_oauth=1'
codex mcp login contentflow365
codex mcp list
```

`add` saves the remote server, `login` opens the OAuth flow, and `list` shows the configured connection. These commands were checked against this Mac's `codex mcp add/login/list --help`; a fresh install and login still need a clean-client smoke test. The command installs the MCP tools, while the packaged skill requires a plugin install.

Claude Code's `--plugin-dir` loads the folder only for that session; it is not a persistent install or a directory listing. Before testing, run `claude plugin validate /absolute/path/to/contentflow365`. The folder passed to either command is the plugin root containing `.claude-plugin/plugin.json`. The Claude Code commands above follow the [official plugin development](https://code.claude.com/docs/en/plugins/create) and [MCP setup](https://code.claude.com/docs/en/mcp) instructions; they still need a fresh run in Claude Code with this package.

### Why a desktop OAuth flow may finish on `localhost`

After approval, the browser must visit the exact callback URL supplied and registered by the agent. Desktop agents can choose a temporary `localhost` address so the installed app receives the authorization code on the user's own computer. That page belongs to the agent, not to ContentFlow365; the address does not mean that ContentFlow365 is running locally. The ContentFlow365 consent screen is hosted on our domain, but we cannot replace the agent's callback with a ContentFlow365 success URL or redirect through a hidden frame without risking a broken or unsafe OAuth exchange. If the callback looks unfinished, return to the agent and check that ContentFlow365 appears connected. The appearance of the final callback must be improved by the client that owns it. Web clients such as ChatGPT use their own hosted callback instead of a desktop loopback URL.

## Sources of truth

- Business and wizard rules: `src/lib/product-knowledge/` and the live product services; `consult_product_guide` exposes supported Persona journeys.
- Editorial help: published Help Center articles through `search_help_center`, with role and translation checks.
- Credit quote and balance: `get_pricing_table`, operation-specific estimate/quote tools, and `get_credit_balance`; the server performs reservation, charging, refunds and idempotency.
- Account and Library navigation: returned MCP links, backed by `src/lib/mcp/account-navigation.ts`. A collection fallback is labelled as such.
- Shared workflow presentation: this package's skill. It contains no rates, account data, tool catalog snapshot or copied FAQ.

When product behavior changes, update the service/API/MCP contract and the relevant knowledge or Help Center entry in the same delivery. Review this skill only if the cross-client workflow changes. Re-scan and re-test a published plugin after metadata or skill changes; document the served SHA separately from the repository SHA.

## Public directory readiness

- OpenAI: its universal ChatGPT/Codex directory accepts a remote MCP plus skills. Submission requires verified publisher identity, support/privacy/terms URLs, domain challenge, tool annotations, a scan, demo and five positive plus three negative cases. Review approval precedes a separate Publish step. This package is a source prototype; it is not a submission receipt.
- Claude: remote MCP can be submitted to the Connectors Directory, separate from a Claude plugin listing. Claude Code's `.claude-plugin/plugin.json` and `.mcp.json` are package formats. The public plugin directory reviews this standalone package. Validate it with `claude plugin validate` and submit the public repository through the appropriate Anthropic form; this GitHub repository is the package source, while the application code stays private. Review, acceptance and an optional Anthropic Verified badge are distinct from adding a custom connector.
- Cursor: accepts this Agent Plugin format. Public marketplace submission requires a public Git repository and Cursor review. The Cursor manifest schema lists `license` as optional; this package grants no open-source license. The application repository stays private. The application repository stays private.
- Grok: official documentation supports custom MCP connectors. A public third-party submission path for Grok's catalog was not established; do not describe the custom entry as a certified listing.

Official references: [OpenAI packaging](https://developers.openai.com/plugins/build/plugins), [OpenAI submission](https://developers.openai.com/plugins/deploy/submission), [Claude connectors](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp), [Claude plugins](https://support.claude.com/en/articles/13837440-use-plugins-in-claude), [Cursor plugin formats](https://prod.cursor.com/docs/reference/plugins), [Cursor install links](https://prod.cursor.com/docs/mcp/install-links), [Grok custom connectors](https://docs.x.ai/grok/connectors).

### Claude Code: install from the ContentFlow365 marketplace

```sh
claude plugin marketplace add contentflow365/ContentFlow365-agent-plugin
claude plugin install contentflow365@contentflow365
```

The marketplace entry lives in `.claude-plugin/marketplace.json` and points to this repository root. Installation still requires each user to authorize the remote MCP server with their own ContentFlow365 account.

Package files are published for installation and review. All rights are reserved unless ContentFlow365 grants a separate license. The private application repository is not part of this package.
