---
name: youtube-data
description: "YouTube channels, videos, transcripts, and comments for agents — content research and channel intelligence."
version: 2.0.0
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [youtube-data, video-research, transcripts]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "⚡"
    homepage: https://superagnt.com/r/ch-youtube-data-docs
---

# YouTube Data

The superagnt YouTube API wraps YouTube surface areas into a single integration. Instead of managing multiple keys, proxies, and rate limits yourself, you call superagnt with one credential and consume structured JSON optimized for downstream AI and analytics. Whether you are building a social listening agent, a content research pipeline, or a creator-intelligence product, this API gives you consistent access to video metadata, channel discovery, comments, subtitles, and recommendations with predictable billing and operational simplicity.

## Best install: connect the MCP server

If this client speaks MCP, connect the scoped server instead of using this
skill's curl calls — native tools, structured parameters, OAuth sign-in, and a
tool surface that can grow on demand:

```
https://mcp.superagnt.com/mcp/youtube
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

Get an API key from the [dashboard](https://superagnt.com/r/ch-youtube-data-key) and export it as
`SUPERAGNT_API_KEY`. Every request sends it as a Bearer token:

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-youtube-data-key.

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
https://api.superagnt.com/v1/data/youtube
```

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `GET` | `/video/screenshot` | Video Screenshot |
| `GET` | `/channel/search/continuation` | Channel Search Continuation |
| `GET` | `/channel/search` | Channel Search |
| `GET` | `/channel/shorts` | Channel Shorts |
| `GET` | `/channel/videos/continuation` | Channel Videos Continuation |
| `GET` | `/channel/details` | Channel Details |
| `GET` | `/channel/id` | Youtube Channel ID |
| `GET` | `/channel/videos` | Channel Videos |
| `POST` | `/channel/videos` | POST Channel Videos |
| `GET` | `/audio/videos/continuation` | Audio Videos Continuation |
| `GET` | `/audio/videos` | Audio Videos |
| `GET` | `/audio/details` | Audio Details |
| `GET` | `/video/recommendations/continuation` | Video Recommendation Continuation |
| `GET` | `/video/recommendations` | Video Recommendation |
| `GET` | `/video/comments` | Video Comments |
| `GET` | `/video/subtitles` | Video Subtitles |
| `GET` | `/video/details` | Video Details |
| `GET` | `/video/data` | Video Data |
| `GET` | `/search/continuation` | Youtube Search Continuation |
| `GET` | `/search/` | Youtube Search |
| `GET` | `/trending/` | Trending Videos |
| `GET` | `/video/comments/continuation` | Video Comments Continuation |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
  {
    "name": "superagnt_youtube_Video_Screenshot",
    "description": "Video Screenshot",
    "method": "GET",
    "path": "/video/screenshot",
    "parameters": {
      "type": "object",
      "properties": {
        "video_id": {
          "type": "string",
          "description": "video_id"
        },
        "timestamp_s": {
          "type": "number",
          "description": "timestamp_s"
        }
      },
      "required": [
        "video_id",
        "timestamp_s"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Channel_Search_Continuation",
    "description": "Channel Search Continuation",
    "method": "GET",
    "path": "/channel/search/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "lang": {
          "type": "string",
          "description": "lang"
        },
        "country": {
          "type": "string",
          "description": "country"
        },
        "query": {
          "type": "string",
          "description": "query"
        },
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        }
      },
      "required": [
        "query",
        "continuation_token",
        "channel_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Channel_Search",
    "description": "Channel Search",
    "method": "GET",
    "path": "/channel/search",
    "parameters": {
      "type": "object",
      "properties": {
        "lang": {
          "type": "string",
          "description": "lang"
        },
        "query": {
          "type": "string",
          "description": "query"
        },
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        },
        "country": {
          "type": "string",
          "description": "country"
        }
      },
      "required": [
        "query",
        "channel_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Channel_Shorts",
    "description": "Channel Shorts",
    "method": "GET",
    "path": "/channel/shorts",
    "parameters": {
      "type": "object",
      "properties": {
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        }
      },
      "required": [
        "channel_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Channel_Videos_Continuation",
    "description": "Channel Videos Continuation",
    "method": "GET",
    "path": "/channel/videos/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        }
      },
      "required": [
        "continuation_token",
        "channel_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Channel_Details",
    "description": "Channel Details",
    "method": "GET",
    "path": "/channel/details",
    "parameters": {
      "type": "object",
      "properties": {
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        }
      },
      "required": [
        "channel_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Youtube_Channel_ID",
    "description": "Youtube Channel ID",
    "method": "GET",
    "path": "/channel/id",
    "parameters": {
      "type": "object",
      "properties": {
        "channel_name": {
          "type": "string",
          "description": "channel_name"
        }
      },
      "required": [
        "channel_name"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Channel_Videos",
    "description": "Channel Videos",
    "method": "GET",
    "path": "/channel/videos",
    "parameters": {
      "type": "object",
      "properties": {
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        }
      },
      "required": [
        "channel_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_POST_Channel_Videos",
    "description": "POST Channel Videos",
    "method": "POST",
    "path": "/channel/videos",
    "parameters": {
      "type": "object",
      "properties": {
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        },
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        }
      }
    }
  },
  {
    "name": "superagnt_youtube_Audio_Videos_Continuation",
    "description": "Audio Videos Continuation",
    "method": "GET",
    "path": "/audio/videos/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "audio_id": {
          "type": "string",
          "description": "audio_id"
        }
      },
      "required": [
        "continuation_token",
        "audio_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Audio_Videos",
    "description": "Audio Videos",
    "method": "GET",
    "path": "/audio/videos",
    "parameters": {
      "type": "object",
      "properties": {
        "audio_id": {
          "type": "string",
          "description": "audio_id"
        }
      },
      "required": [
        "audio_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Audio_Details",
    "description": "Audio Details",
    "method": "GET",
    "path": "/audio/details",
    "parameters": {
      "type": "object",
      "properties": {
        "audio_id": {
          "type": "string",
          "description": "audio_id"
        }
      },
      "required": [
        "audio_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Video_Recommendation_Continuation",
    "description": "Video Recommendation Continuation",
    "method": "GET",
    "path": "/video/recommendations/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "video_id": {
          "type": "string",
          "description": "video_id"
        }
      },
      "required": [
        "continuation_token",
        "video_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Video_Recommendation",
    "description": "Video Recommendation",
    "method": "GET",
    "path": "/video/recommendations",
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
    "name": "superagnt_youtube_Video_Comments",
    "description": "Video Comments",
    "method": "GET",
    "path": "/video/comments",
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
    "name": "superagnt_youtube_Video_Subtitles",
    "description": "Video Subtitles",
    "method": "GET",
    "path": "/video/subtitles",
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
    "name": "superagnt_youtube_Video_Details",
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
    "name": "superagnt_youtube_Video_Data",
    "description": "Video Data",
    "method": "GET",
    "path": "/video/data",
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
    "name": "superagnt_youtube_Youtube_Search_Continuation",
    "description": "Youtube Search Continuation",
    "method": "GET",
    "path": "/search/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "query": {
          "type": "string",
          "description": "query"
        },
        "lang": {
          "type": "string",
          "description": "lang"
        },
        "country": {
          "type": "string",
          "description": "country"
        },
        "order_by": {
          "type": "string",
          "description": "Possible values: \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"last_hour\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"today\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"this_week\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"this_month\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"this_year\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\""
        }
      },
      "required": [
        "continuation_token",
        "query"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Youtube_Search",
    "description": "Youtube Search",
    "method": "GET",
    "path": "/search/",
    "parameters": {
      "type": "object",
      "properties": {
        "lang": {
          "type": "string",
          "description": "lang"
        },
        "query": {
          "type": "string",
          "description": "query"
        },
        "order_by": {
          "type": "string",
          "description": "Possible values: \\\\\\\\\\\\\\\"last_hour\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\"today\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\"this_week\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\"this_month\\\\\\\\\\\\\\\", \\\\\\\\\\\\\\\"this_year\\\\\\\\\\\\\\\""
        },
        "country": {
          "type": "string",
          "description": "country"
        }
      },
      "required": [
        "query"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Trending_Videos",
    "description": "Trending Videos",
    "method": "GET",
    "path": "/trending/",
    "parameters": {
      "type": "object",
      "properties": {
        "country": {
          "type": "string",
          "description": "country"
        },
        "lang": {
          "type": "string",
          "description": "lang"
        },
        "section": {
          "type": "string",
          "description": "Possible values: \\\\\\\"Now\\\\\\\", \\\\\\\"Music\\\\\\\", \\\\\\\"Movies\\\\\\\", \\\\\\\"Gaming\\\\\\\""
        }
      }
    }
  },
  {
    "name": "superagnt_youtube_Video_Comments_Continuation",
    "description": "Video Comments Continuation",
    "method": "GET",
    "path": "/video/comments/continuation",
    "parameters": {
      "type": "object",
      "properties": {
        "continuation_token": {
          "type": "string",
          "description": "continuation_token"
        },
        "video_id": {
          "type": "string",
          "description": "video_id"
        }
      },
      "required": [
        "continuation_token",
        "video_id"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Get_Channel_Email_By_URL",
    "description": "Get Channel Email by URL",
    "method": "POST",
    "path": "/channel/email",
    "parameters": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "description": "Full YouTube channel URL (e.g. https://www.youtube.com/@theAIsearch)."
        }
      },
      "required": [
        "url"
      ]
    }
  },
  {
    "name": "superagnt_youtube_Get_Channel_Email_By_Id",
    "description": "Get Channel Email by Channel ID",
    "method": "GET",
    "path": "/channel/{channel_id}/email",
    "parameters": {
      "type": "object",
      "properties": {
        "channel_id": {
          "type": "string",
          "description": "channel_id"
        }
      },
      "required": [
        "channel_id"
      ]
    }
  }
]
```

## Example

```bash
curl -X GET &#x27;https://api.superagnt.com/v1/data/youtube&#x27; \
  -H &#x27;X-API-Key: your_api_key_here&#x27; \
  -H &#x27;Content-Type: application/json&#x27;
```

## Use Cases

- Social and creator intelligence agents using YouTube
- Marketing and research teams monitoring accounts and content
- Product teams building alerts, digests, and dashboards
- Developers prototyping LLM tools that need live network data
- Data teams joining YouTube signals with CRM or warehouse data

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

- [Documentation](https://superagnt.com/r/ch-youtube-data-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-youtube-data-key)
- [This listing](https://clawhub.ai/superagnt/skills/youtube-data)
