# Tigris Storage — Cursor Plugin

Complete Tigris object storage integration for [Cursor](https://cursor.com). Uses 4 Cursor plugin primitives: **skills**, **rules**, **MCP server**, and a **subagent**.

## What's Included

### Skills (5)

| Skill | Description |
|-------|-------------|
| **tigris-authentication** | CLI install, login (OAuth + credentials), configure, whoami, logout, credentials test, orgs |
| **tigris-buckets** | Create, configure, and delete buckets — regions, tiers, CORS, migrations, TTL, notifications, snapshots, forks |
| **tigris-objects** | Upload, download, list, move, delete, and presign objects — unix-style commands + low-level API |
| **tigris-access-keys** | Create, list, assign roles, and delete programmatic access keys |
| **tigris-iam** | Manage IAM policies, users, invitations, and permissions |

### Rules (2)

| Rule | Trigger | Description |
|------|---------|-------------|
| **tigris-sdk-patterns** | `**/*.{ts,tsx,js,jsx}` | SDK best practices — endpoint config, client uploads, error handling, pagination |
| **tigris-security** | Always | Security guardrails — access control, key management, safe destructive operations |

### MCP Server (10 tools)

Uses the hosted Tigris MCP server over HTTP with OAuth authentication — no API keys to configure. On first use, Cursor opens a browser for you to log in. The configuration in `.mcp.json` is:

```json
{
  "mcpServers": {
    "tigris": {
      "type": "http",
      "url": "https://mcp.storage.dev/mcp"
    }
  }
}
```

Tools available:

| Tool | Description |
|------|-------------|
| `tigris_list_buckets` | List all buckets |
| `tigris_create_bucket` | Create a new bucket |
| `tigris_delete_bucket` | Delete a bucket |
| `tigris_list_objects` | List objects in a bucket |
| `tigris_put_object` | Upload an object from text/data |
| `tigris_put_object_from_path` | Upload a local file |
| `tigris_get_object` | Download an object |
| `tigris_delete_object` | Delete an object |
| `tigris_get_signed_url_object` | Generate a presigned URL |
| `tigris_upload_file_and_get_url` | Upload a file and return its URL |

### Subagent

**tigris-storage-agent** — A specialized agent for multi-step storage workflows:

- New project setup (bucket + access key + CORS + SDK)
- S3 migration via shadow bucket
- Dev sandbox via copy-on-write fork
- Bucket security audit
- Multi-step production deployment

## Getting Started

### 1. Install the plugin

Install from the [Cursor Marketplace](https://cursor.com/marketplace), or for local testing copy this directory to `~/.cursor/plugins/local/tigris-storage/` and reload the window.

### 2. Authenticate

The MCP server uses OAuth — you'll be prompted to log in via your browser on first use.

For CLI operations, install and authenticate separately:

```bash
npm install -g @tigrisdata/cli
tigris login
```

### 3. Start using

- Ask the agent to "set up Tigris storage for my project"
- Use MCP tools for bucket/object operations
- The SDK patterns rule activates automatically in `.ts`/`.js` files
- Security rules are always active

## Alternative MCP Configurations

The default configuration above uses the hosted remote server with OAuth — no keys needed. If you prefer to run the MCP server locally (e.g. for air-gapped environments or custom endpoints), replace `.mcp.json` with one of the following:

### Local stdio (requires access key)

```json
{
  "mcpServers": {
    "tigris": {
      "command": "npx",
      "args": ["-y", "@tigrisdata/tigris-mcp-server", "run"],
      "env": {
        "AWS_ACCESS_KEY_ID": "tid_xxx",
        "AWS_SECRET_ACCESS_KEY": "tsec_yyy",
        "AWS_ENDPOINT_URL_S3": "https://t3.storage.dev"
      }
    }
  }
}
```

### Docker

```json
{
  "mcpServers": {
    "tigris": {
      "command": "docker",
      "args": [
        "run",
        "-e", "AWS_ACCESS_KEY_ID",
        "-e", "AWS_SECRET_ACCESS_KEY",
        "-e", "AWS_ENDPOINT_URL_S3",
        "-i", "--rm",
        "quay.io/tigrisdata/tigris-mcp-server:latest"
      ],
      "env": {
        "AWS_ACCESS_KEY_ID": "tid_xxx",
        "AWS_SECRET_ACCESS_KEY": "tsec_yyy",
        "AWS_ENDPOINT_URL_S3": "https://t3.storage.dev"
      }
    }
  }
}
```

## Links

- [Tigris Documentation](https://www.tigrisdata.com/docs)
- [Tigris Dashboard](https://console.tigris.dev)
- [SDK Reference](https://www.tigrisdata.com/docs/sdks/tigris/)
- [MCP Server](https://github.com/tigrisdata/tigris-mcp-server)
- [CLI](https://github.com/tigrisdata/cli)

## License

MIT — see [LICENSE](LICENSE).
