# Tigris Storage

[Agent Skills](https://www.anthropic.com/engineering/equipping-agents-from-the-real-world-with-agent-skills) for [Tigris](https://www.tigrisdata.com) object storage — bucket management, object operations, access keys, IAM, and migrations.

## Installing

### Claude Code

Install using the [plugin marketplace](https://code.claude.com/docs/en/discover-plugins#add-from-github):

```
/plugin marketplace add tigrisdata/tigris-agents-plugins
/plugin install tigris-storage@tigris-agents-plugins
```

### Cursor

Add manually via **Settings > Rules > Add Rule > Remote Rule (Github)** with `tigrisdata/tigris-agents-plugins`.

## Skills

Skills are contextual and auto-loaded based on your conversation. When a request matches a skill's triggers, the agent loads and applies the relevant skill to provide accurate, up-to-date guidance.

| Skill                 | Useful for                                                                                                           |
| --------------------- | -------------------------------------------------------------------------------------------------------------------- |
| tigris-authentication | Installing the CLI, logging in (OAuth + credentials), configuring credentials, switching orgs                        |
| tigris-buckets        | Creating, configuring, and deleting buckets — regions, tiers, CORS, migrations, TTL, notifications, snapshots, forks |
| tigris-objects        | Uploading, downloading, listing, moving, deleting, and presigning objects — unix-style commands + low-level API      |
| tigris-access-keys    | Creating, listing, assigning roles, and deleting programmatic access keys                                            |
| tigris-iam            | Managing IAM policies, users, invitations, and permissions                                                           |

## MCP Server

Tigris also provides a [remote MCP server](https://github.com/tigrisdata/mcp) with OAuth authentication — no API keys to configure. Add it to your editor's MCP config if you want tool-based access alongside the CLI:

| Tool                             | Purpose                          |
| -------------------------------- | -------------------------------- |
| `tigris_list_buckets`            | List all buckets                 |
| `tigris_create_bucket`           | Create a new bucket              |
| `tigris_delete_bucket`           | Delete a bucket                  |
| `tigris_list_objects`            | List objects in a bucket         |
| `tigris_put_object`              | Upload an object from text/data  |
| `tigris_put_object_from_path`    | Upload a local file              |
| `tigris_get_object`              | Download an object               |
| `tigris_delete_object`           | Delete an object                 |
| `tigris_get_signed_url_object`   | Generate a presigned URL         |
| `tigris_upload_file_and_get_url` | Upload a file and return its URL |

## Subagent

**tigris-storage-agent** — a specialized agent for multi-step storage workflows:

- New project setup (bucket + access key + CORS)
- S3 migration via shadow bucket
- Dev sandbox via copy-on-write fork
- Bucket security audit
- Multi-step production deployment

## Rules (Cursor only)

| Rule                | Trigger                | Description                                                                       |
| ------------------- | ---------------------- | --------------------------------------------------------------------------------- |
| tigris-sdk-patterns | `**/*.{ts,tsx,js,jsx}` | SDK best practices — endpoint config, client uploads, error handling              |
| tigris-security     | Always                 | Security guardrails — access control, key management, safe destructive operations |

## Resources

- [Tigris Documentation](https://www.tigrisdata.com/docs)
- [Tigris Dashboard](https://console.tigris.dev)
- [MCP Server](https://github.com/tigrisdata/mcp)
- [CLI Reference](https://github.com/tigrisdata/cli)
