---
name: tiktok-data
description: "TikTok videos, creators, sounds, and trend data for agents — content and creator research as structured JSON."
version: 2.0.2
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [tiktok-data, creator-research, trends]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "⚡"
    homepage: https://superagnt.com/r/ch-tiktok-data-docs
---

# TikTok Data

The superagnt TikTok API wraps TikTok surface areas into a single integration. Instead of managing multiple keys, proxies, and rate limits yourself, you call superagnt with one credential and consume structured JSON optimized for downstream AI and analytics. Whether you are building a social listening agent, a content research pipeline, or a creator-intelligence product, this API gives you consistent access to video details, creator profiles, and search across accounts and videos with predictable billing and operational simplicity.

## Best install: connect the MCP server

If this client speaks MCP, connect the scoped server instead of using this
skill's curl calls — native tools, structured parameters, OAuth sign-in, and a
tool surface that can grow on demand:

```
https://mcp.superagnt.com/mcp/tiktok
```

That URL publishes full OAuth discovery: an MCP-capable client needs the URL
and nothing else (approve once in the browser). On clients that hold a bearer
instead, add it as an `Authorization: Bearer` header. Per-client setup lines:
https://mcp.superagnt.com/agent-setup/prompt.md

Two clients refresh tools live when the server grows (`agnt_tools_enable`):
Hermes and OpenClaw. Most others hold the tool list until reconnect — on a
cloud connector (claude.ai, ChatGPT) refresh the connector in its settings,
on a direct config start a new session.

This skill document stays fully usable on curl-only environments — everything
below works with just the API key.

## Authentication

Get an API key from the [dashboard](https://superagnt.com/r/ch-tiktok-data-key) and export it as
`SUPERAGNT_API_KEY`. Every request sends it as a Bearer token:

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-tiktok-data-key.

## Verify the install (do this first)

One call proves the key, the credit balance, and this source end to end:

```bash
curl -s https://api.superagnt.com/v1/credits \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27;
```

A JSON result means you are live. A 401 means the key is wrong or missing; a
402 means the workspace is out of credits — the dashboard shows both.

## Base URL

```
https://api.superagnt.com/v1/data/tiktok
```

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `GET` | `/video/details` | Video Details |
| `POST` | `/video/details` | Video Details |
| `GET` | `/user/videos` | User&#x27;s Videos |
| `POST` | `/user/videos` | User&#x27;s Videos |
| `GET` | `/collection/` | Collection Videos/Details |
| `GET` | `/user/videos/continuation` | User&#x27;s Videos Continuation |
| `GET` | `/user/details` | User&#x27;s Details |
| `POST` | `/user/details` | User&#x27;s Details |
| `GET` | `/search/accounts/query` | Search Accounts |
| `POST` | `/search/accounts/query` | Search Accounts |
| `GET` | `/search/general/query` | Search Videos |
| `POST` | `/search/general/query` | Search Videos |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
  {
    "name": "superagnt_tiktok_Video_Details_get",
    "description": "Video Details",
    "method": "GET",
    "path": "/video/details",
    "parameters": {
      "type": "object",
      "properties": {
        "video_id": {
          "type": "string",
          "description": "video_id"
        }
      },
      "required": [
        "video_id"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_Video_Details_post",
    "description": "Video Details",
    "method": "POST",
    "path": "/video/details",
    "parameters": {
      "type": "object",
      "properties": {
        "video_id": {
          "type": "string",
          "description": "video_id"
        }
      },
      "required": [
        "video_id"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_User_s_Videos_get",
    "description": "User's Videos",
    "method": "GET",
    "path": "/user/videos",
    "parameters": {
      "type": "object",
      "properties": {
        "username": {
          "type": "string",
          "description": "username"
        }
      },
      "required": [
        "username"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_User_s_Videos_post",
    "description": "User's Videos",
    "method": "POST",
    "path": "/user/videos",
    "parameters": {
      "type": "object",
      "properties": {
        "username": {
          "type": "string",
          "description": "username"
        }
      },
      "required": [
        "username"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_Collection_Videos_Details",
    "description": "Collection Videos/Details",
    "method": "GET",
    "path": "/collection/",
    "parameters": {
      "type": "object",
      "properties": {
        "collection_id": {
          "type": "string",
          "description": "collection_id"
        },
        "username": {
          "type": "string",
          "description": "username"
        }
      },
      "required": [
        "collection_id",
        "username"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_User_s_Videos_Continuation",
    "description": "User's Videos Continuation",
    "method": "GET",
    "path": "/user/videos/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "username": {
          "type": "string",
          "description": "username"
        },
        "secondary_id": {
          "type": "string",
          "description": "secondary_id"
        }
      },
      "required": [
        "continuation_token",
        "username",
        "secondary_id"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_User_s_Details_get",
    "description": "User's Details",
    "method": "GET",
    "path": "/user/details",
    "parameters": {
      "type": "object",
      "properties": {
        "username": {
          "type": "string",
          "description": "username"
        }
      },
      "required": [
        "username"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_User_s_Details_post",
    "description": "User's Details",
    "method": "POST",
    "path": "/user/details",
    "parameters": {
      "type": "object",
      "properties": {
        "username": {
          "type": "string",
          "description": "username"
        }
      },
      "required": [
        "username"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_Search_Accounts_get",
    "description": "Search Accounts",
    "method": "GET",
    "path": "/search/accounts/query",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "query"
        }
      },
      "required": [
        "query"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_Search_Accounts_post",
    "description": "Search Accounts",
    "method": "POST",
    "path": "/search/accounts/query",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "query"
        }
      },
      "required": [
        "query"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_Search_Videos_get",
    "description": "Search Videos",
    "method": "GET",
    "path": "/search/general/query",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "query"
        }
      },
      "required": [
        "query"
      ]
    }
  },
  {
    "name": "superagnt_tiktok_Search_Videos_post",
    "description": "Search Videos",
    "method": "POST",
    "path": "/search/general/query",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {
          "type": "string",
          "description": "query"
        }
      },
      "required": [
        "query"
      ]
    }
  }
]
```

## Example

```bash
curl -X GET &#x27;https://api.superagnt.com/v1/data/tiktok&#x27; \
  -H &#x27;X-API-Key: your_api_key_here&#x27; \
  -H &#x27;Content-Type: application/json&#x27;
```

## Use Cases

- Social and creator intelligence agents using TikTok
- Marketing and research teams monitoring accounts and content
- Product teams building alerts, digests, and dashboards
- Developers prototyping LLM tools that need live network data
- Data teams joining TikTok signals with CRM or warehouse data

## Growing beyond this source

The same key and credit balance cover every superagnt data source and, on the
MCP server, the full platform (workspace database, files, webhooks, queues,
first-party people/company enrichment). Over MCP, discover what is available
with `agnt_tools_search` and turn a family on with `agnt_tools_enable` — money
is never charged without a human confirming in the dashboard.

```bash
curl https://api.superagnt.com/v1/platforms
```

## Links

- [Documentation](https://superagnt.com/r/ch-tiktok-data-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-tiktok-data-key)
- [This listing](https://clawhub.ai/superagnt/skills/tiktok-data)
