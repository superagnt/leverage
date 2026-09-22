---
name: heyreach-outreach
description: "Manage LinkedIn outreach campaigns, leads, lists, inbox conversations, and analytics via HeyReach through the superagnt unified API."
version: 2.0.2
author: superagnt
license: MIT-0
platforms: [macos, linux, windows]
required_environment_variables:
  - SUPERAGNT_API_KEY
metadata:
  hermes:
    tags: [heyreach, integration, api, ai-agent]
  openclaw:
    requires:
      env:
        - SUPERAGNT_API_KEY
      bins:
        - curl
    primaryEnv: SUPERAGNT_API_KEY
    emoji: "🤝"
    homepage: https://superagnt.com/r/ch-heyreach-outreach-docs
---

# HeyReach Integration

The HeyReach integration connects your HeyReach workspace to superagnt, letting AI agents and workflows manage LinkedIn outreach programmatically. Read and control campaigns, push leads into running campaigns with the right sender account, sync conversations from the unified inbox, and pull aggregated outreach analytics — all proxied through your superagnt API key.

## Alternative install: the MCP server

If this client speaks MCP, you can connect the workspace server instead of
using this skill's curl calls — after the user connects their
HeyReach account in the dashboard, the same endpoints below are exposed
as native MCP tools:

```
https://mcp.superagnt.com/mcp
```

The URL publishes full OAuth discovery — an MCP-capable client needs the URL
and nothing else (approve once in the browser). Per-client setup lines:
https://mcp.superagnt.com/agent-setup/prompt.md

Everything below works on curl-only environments with just the API key.

## Capabilities and safety

This skill documents the HeyReach API surface the user's connected
account can reach — which can include destructive operations (updates,
deletes) and, where the vendor supports them, actions performed as the user
(sending messages, modifying records, changing settings). Two hard rules:

- **Confirm before destructive or outbound actions.** Never delete, overwrite,
  or send on the user's behalf without their explicit confirmation in the
  conversation.
- **Stay inside the user's request.** Use only the endpoints the task needs;
  this skill grants no access beyond the HeyReach connection the user
  set up themselves.

## Prerequisites

1. An API key — get one from the [dashboard](https://superagnt.com/r/ch-heyreach-outreach-key) and export it as
   `SUPERAGNT_API_KEY`.
2. A connected HeyReach account — connect it in the
   [connections dashboard](https://app.superagnt.com/dashboard/connections). Calls fail with a clear error
   until the vendor is connected; that error is the signal to send the user to
   the connections page, not a bug.

Note: `/v1/connections/*` calls run on the USER'S HeyReach credentials —
vendor-side rate limits and billing are theirs, not platform credits.

## Authentication

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

If the variable is not set, ask the user for their key or point them at
https://superagnt.com/r/ch-heyreach-outreach-key.

## Verify the install (do this first)

```bash
curl -s https://api.superagnt.com/v1/credits \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27;
```

A JSON result proves the key end to end. 401 = bad key; an error naming the
connection means the HeyReach account is not connected yet.

## Base URL

```
https://api.superagnt.com/v1/connections/heyreach
```

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `GET` | `/auth/CheckApiKey` | Check API Key |
| `POST` | `/campaign/GetAll` | List campaigns |
| `GET` | `/campaign/GetById` | Get campaign by ID |
| `POST` | `/campaign/Pause` | Pause campaign |
| `POST` | `/campaign/Resume` | Resume campaign |
| `POST` | `/campaign/StartCampaign` | Start campaign |
| `POST` | `/campaign/Create` | Create campaign |
| `POST` | `/campaign/CreateCampaignFromTemplate` | Create campaign from template |
| `POST` | `/campaign/UpdateSettings` | Update campaign settings |
| `POST` | `/campaign/UpdateSchedule` | Update campaign schedule |
| `POST` | `/campaign/UpdateAccounts` | Update campaign sender accounts |
| `POST` | `/campaign/UpdateSequence` | Update campaign sequence |
| `GET` | `/campaign/GetCampaignSequence` | Get campaign sequence |
| `POST` | `/campaign/AddLeadsToCampaignV2` | Add leads to campaign |
| `POST` | `/campaign/AddLeadsToCampaign` | Add leads to campaign (v1, legacy) |
| `POST` | `/campaign/StopLeadInCampaign` | Stop lead in campaign |
| `POST` | `/campaign/GetLeadsFromCampaign` | Get leads in campaign |
| `POST` | `/campaign/GetCampaignsForLead` | Get campaigns for lead |
| `POST` | `/inbox/GetConversationsV2` | Get conversations |
| `GET` | `/inbox/GetChatroom/{accountId}/{conversationId}` | Get chatroom (single conversation thread) |
| `POST` | `/inbox/SendMessage` | Send message in conversation |
| `POST` | `/inbox/SetSeenStatus` | Set conversation seen status |
| `POST` | `/li_account/GetAll` | List connected LinkedIn accounts |
| `GET` | `/li_account/GetById` | Get connected LinkedIn account by ID |
| `POST` | `/list/GetAll` | List lead/company lists |
| `GET` | `/list/GetById` | Get list by ID |
| `POST` | `/list/CreateEmptyList` | Create empty list |
| `POST` | `/list/GetLeadsFromList` | Get leads from a list |
| `POST` | `/list/GetCompaniesFromList` | Get companies from a list |
| `POST` | `/list/AddLeadsToListV2` | Add leads to list |
| `POST` | `/list/AddLeadsToList` | Add leads to list (v1, legacy) |
| `DELETE` | `/list/DeleteLeadsFromList` | Delete leads from list by member ID |
| `DELETE` | `/list/DeleteLeadsFromListByProfileUrl` | Delete leads from list by profile URL |
| `POST` | `/list/GetListsForLead` | Get lists a lead belongs to |
| `POST` | `/lead/GetLead` | Get lead by profile URL |
| `POST` | `/lead/AddTags` | Add tags to lead |
| `POST` | `/lead/ReplaceTags` | Replace lead tags |
| `POST` | `/lead/GetTags` | Get tags for a lead |
| `POST` | `/lead_tags/CreateTags` | Create workspace lead tags |
| `POST` | `/stats/GetOverallStats` | Get overall stats |
| `POST` | `/stats/GetOverallStatsByCampaign` | Get overall stats grouped by campaign |
| `POST` | `/MyNetwork/GetMyNetworkForSender` | Get sender&#x27;s LinkedIn network |
| `POST` | `/MyNetwork/IsConnection` | Check if a lead is a 1st-degree connection |
| `POST` | `/webhooks/CreateWebhook` | Create webhook subscription |
| `GET` | `/webhooks/GetWebhookById` | Get webhook by ID |
| `POST` | `/webhooks/GetAllWebhooks` | List webhooks |
| `PATCH` | `/webhooks/UpdateWebhook` | Update webhook subscription |
| `DELETE` | `/webhooks/DeleteWebhook` | Delete webhook subscription |
| `GET` | `/management/organizations/workspaces` | List workspaces (organization-scoped key) |
| `POST` | `/management/organizations/workspaces` | Create workspace |
| `PATCH` | `/management/organizations/workspaces/{workspaceId}` | Update workspace |
| `GET` | `/management/organizations/api-keys/workspaces/{workspaceId}` | Get workspace API keys |
| `POST` | `/management/organizations/api-keys/workspaces/{workspaceId}` | Create workspace API key |
| `POST` | `/management/organizations/users` | List users in the organization |
| `GET` | `/management/organizations/users/{userId}` | Get user by ID |
| `POST` | `/management/organizations/users/workspaces/{workspaceId}` | List users in workspace |
| `POST` | `/management/organizations/users/invite/admins` | Invite admins |
| `POST` | `/management/organizations/users/invite/managers` | Invite managers |
| `POST` | `/management/organizations/users/invite/members` | Invite members |

## Tool Schemas

The following JSON defines every operation with its parameters. Each tool maps
to an API endpoint under the base URL.

```json
[
  {
    "name": "superagnt_connection_heyreach_auth_check_api_key",
    "description": "Check API Key",
    "method": "GET",
    "path": "/auth/CheckApiKey",
    "parameters": {
      "type": "object",
      "properties": {}
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_get_all",
    "description": "List campaigns",
    "method": "POST",
    "path": "/campaign/GetAll",
    "parameters": {
      "type": "object",
      "properties": {
        "offset": {
          "type": "integer",
          "description": "Zero-based offset into the result set."
        },
        "limit": {
          "type": "integer",
          "description": "Maximum number of campaigns to return."
        },
        "keyword": {
          "type": "string",
          "description": "Optional free-text filter applied to campaign names."
        },
        "statuses": {
          "type": "array",
          "description": "Optional list of campaign statuses to include."
        },
        "accountIds": {
          "type": "array",
          "description": "Optional list of LinkedIn sender account IDs to filter campaigns by."
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_get_by_id",
    "description": "Get campaign by ID",
    "method": "GET",
    "path": "/campaign/GetById",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "Numeric identifier of the campaign."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_pause",
    "description": "Pause campaign",
    "method": "POST",
    "path": "/campaign/Pause",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "Numeric identifier of the campaign to pause."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_resume",
    "description": "Resume campaign",
    "method": "POST",
    "path": "/campaign/Resume",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "Numeric identifier of the campaign to resume."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_start",
    "description": "Start campaign",
    "method": "POST",
    "path": "/campaign/StartCampaign",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "Numeric identifier of the campaign to start."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_create",
    "description": "Create campaign",
    "method": "POST",
    "path": "/campaign/Create",
    "parameters": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string",
          "description": "Display name for the new campaign."
        },
        "linkedInUserListId": {
          "type": "integer",
          "description": "ID of the existing lead list (`USER_LIST`) to back the campaign. Omit when the campaign should be created with an empty list to be populated via `/campaign/AddLeadsToCampaignV2`."
        },
        "linkedInAccountIds": {
          "type": "array",
          "description": "IDs of the connected LinkedIn sender accounts that should run this campaign."
        },
        "schedule": {
          "type": "string",
          "description": "schedule"
        },
        "sequence": {
          "type": "string",
          "description": "sequence"
        }
      },
      "required": [
        "name",
        "linkedInAccountIds",
        "schedule",
        "sequence"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_create_from_template",
    "description": "Create campaign from template",
    "method": "POST",
    "path": "/campaign/CreateCampaignFromTemplate",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignIdForTemplate": {
          "type": "integer",
          "description": "ID of the existing campaign whose schedule/sequence/settings should be cloned."
        },
        "name": {
          "type": "string",
          "description": "Display name for the new campaign."
        },
        "linkedInUserListId": {
          "type": "integer",
          "description": "ID of the lead list (`USER_LIST`) to back the new campaign."
        },
        "linkedInAcccountIdsForCampaign": {
          "type": "array",
          "description": "IDs of the connected LinkedIn sender accounts that should run the new campaign. (Note: spelling `linkedInAcccountIdsForCampaign` matches the upstream API.)"
        },
        "excludeContactedFromOtherCampaigns": {
          "type": "boolean",
          "description": "Skip leads that have been contacted by any other campaign in the workspace."
        },
        "excludeHasOtherAccConversations": {
          "type": "boolean",
          "description": "Skip leads who already have an open conversation with another connected LinkedIn account in the workspace."
        },
        "excludeContactedFromSenderInOtherCampaign": {
          "type": "boolean",
          "description": "Skip leads previously contacted by this campaign's sender from any other campaign."
        },
        "excludeListId": {
          "type": "integer",
          "description": "Optional ID of an exclusion list — leads present in this list will be skipped."
        }
      },
      "required": [
        "campaignIdForTemplate",
        "name",
        "linkedInAcccountIdsForCampaign"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_update_settings",
    "description": "Update campaign settings",
    "method": "POST",
    "path": "/campaign/UpdateSettings",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "name": {
          "type": "string",
          "description": "name"
        },
        "linkedInUserListId": {
          "type": "integer",
          "description": "ID of the lead list backing this campaign."
        },
        "excludeContactedFromOtherCampaigns": {
          "type": "boolean",
          "description": "excludeContactedFromOtherCampaigns"
        },
        "excludeHasOtherAccConversations": {
          "type": "boolean",
          "description": "excludeHasOtherAccConversations"
        },
        "excludeContactedFromSenderInOtherCampaign": {
          "type": "boolean",
          "description": "excludeContactedFromSenderInOtherCampaign"
        },
        "excludeListId": {
          "type": "integer",
          "description": "Optional exclusion list ID. Pass `null` to clear."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_update_schedule",
    "description": "Update campaign schedule",
    "method": "POST",
    "path": "/campaign/UpdateSchedule",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "schedule": {
          "type": "string",
          "description": "schedule"
        }
      },
      "required": [
        "campaignId",
        "schedule"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_update_accounts",
    "description": "Update campaign sender accounts",
    "method": "POST",
    "path": "/campaign/UpdateAccounts",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "linkedInAccountIds": {
          "type": "array",
          "description": "linkedInAccountIds"
        }
      },
      "required": [
        "campaignId",
        "linkedInAccountIds"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_update_sequence",
    "description": "Update campaign sequence",
    "method": "POST",
    "path": "/campaign/UpdateSequence",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "sequence": {
          "type": "string",
          "description": "sequence"
        }
      },
      "required": [
        "campaignId",
        "sequence"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_get_sequence",
    "description": "Get campaign sequence",
    "method": "GET",
    "path": "/campaign/GetCampaignSequence",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_add_leads_v2",
    "description": "Add leads to campaign",
    "method": "POST",
    "path": "/campaign/AddLeadsToCampaignV2",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "Numeric identifier of the destination campaign."
        },
        "accountLeadPairs": {
          "type": "array",
          "description": "List of `{ lead, linkedInAccountId }` pairs. `linkedInAccountId` selects which connected sender account should own the outreach for that lead and must be one of the campaign's assigned sender accounts."
        }
      },
      "required": [
        "campaignId",
        "accountLeadPairs"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_add_leads",
    "description": "Add leads to campaign (v1, legacy)",
    "method": "POST",
    "path": "/campaign/AddLeadsToCampaign",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "accountLeadPairs": {
          "type": "array",
          "description": "accountLeadPairs"
        },
        "resumeFinishedCampaign": {
          "type": "boolean",
          "description": "If true and the campaign is `FINISHED`, the API will resume it before appending leads."
        },
        "resumePausedCampaign": {
          "type": "boolean",
          "description": "If true and the campaign is `PAUSED`, the API will resume it before appending leads."
        }
      },
      "required": [
        "campaignId",
        "accountLeadPairs"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_stop_lead",
    "description": "Stop lead in campaign",
    "method": "POST",
    "path": "/campaign/StopLeadInCampaign",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "leadMemberId": {
          "type": "string",
          "description": "HeyReach internal ID of the lead within the campaign (one of `leadMemberId` or `leadUrl` is required)."
        },
        "leadUrl": {
          "type": "string",
          "description": "Public LinkedIn profile URL of the lead (one of `leadMemberId` or `leadUrl` is required)."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_get_leads",
    "description": "Get leads in campaign",
    "method": "POST",
    "path": "/campaign/GetLeadsFromCampaign",
    "parameters": {
      "type": "object",
      "properties": {
        "campaignId": {
          "type": "integer",
          "description": "campaignId"
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "timeFrom": {
          "type": "string",
          "description": "Inclusive lower bound on the timestamp selected by `timeFilter`."
        },
        "timeTo": {
          "type": "string",
          "description": "Inclusive upper bound on the timestamp selected by `timeFilter`."
        },
        "timeFilter": {
          "type": "string",
          "description": "Which lead timestamp `timeFrom`/`timeTo` apply to."
        }
      },
      "required": [
        "campaignId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_campaign_get_for_lead",
    "description": "Get campaigns for lead",
    "method": "POST",
    "path": "/campaign/GetCampaignsForLead",
    "parameters": {
      "type": "object",
      "properties": {
        "email": {
          "type": "string",
          "description": "email"
        },
        "linkedinId": {
          "type": "string",
          "description": "linkedinId"
        },
        "profileUrl": {
          "type": "string",
          "description": "profileUrl"
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_inbox_get_conversations_v2",
    "description": "Get conversations",
    "method": "POST",
    "path": "/inbox/GetConversationsV2",
    "parameters": {
      "type": "object",
      "properties": {
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "filters": {
          "type": "object",
          "description": "Optional filter object."
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_inbox_get_chatroom",
    "description": "Get chatroom (single conversation thread)",
    "method": "GET",
    "path": "/inbox/GetChatroom/{accountId}/{conversationId}",
    "parameters": {
      "type": "object",
      "properties": {
        "accountId": {
          "type": "integer",
          "description": "ID of the connected LinkedIn sender account that owns the chatroom."
        },
        "conversationId": {
          "type": "string",
          "description": "Opaque conversation/chatroom identifier (as returned by `/inbox/GetConversationsV2`)."
        }
      },
      "required": [
        "accountId",
        "conversationId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_inbox_send_message",
    "description": "Send message in conversation",
    "method": "POST",
    "path": "/inbox/SendMessage",
    "parameters": {
      "type": "object",
      "properties": {
        "message": {
          "type": "string",
          "description": "Plain-text body of the message."
        },
        "subject": {
          "type": "string",
          "description": "Optional InMail subject line. Ignored for non-InMail threads."
        },
        "conversationId": {
          "type": "string",
          "description": "Opaque conversation/chatroom identifier (as returned by `/inbox/GetConversationsV2`)."
        },
        "linkedInAccountId": {
          "type": "integer",
          "description": "ID of the connected LinkedIn sender account that should send the message."
        }
      },
      "required": [
        "message",
        "conversationId",
        "linkedInAccountId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_inbox_set_seen_status",
    "description": "Set conversation seen status",
    "method": "POST",
    "path": "/inbox/SetSeenStatus",
    "parameters": {
      "type": "object",
      "properties": {
        "conversationId": {
          "type": "string",
          "description": "conversationId"
        },
        "linkedInAccountId": {
          "type": "integer",
          "description": "linkedInAccountId"
        },
        "seen": {
          "type": "boolean",
          "description": "seen"
        }
      },
      "required": [
        "conversationId",
        "linkedInAccountId",
        "seen"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_li_account_get_all",
    "description": "List connected LinkedIn accounts",
    "method": "POST",
    "path": "/li_account/GetAll",
    "parameters": {
      "type": "object",
      "properties": {
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "keyword": {
          "type": "string",
          "description": "Optional free-text filter applied to the account's display name."
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_li_account_get_by_id",
    "description": "Get connected LinkedIn account by ID",
    "method": "GET",
    "path": "/li_account/GetById",
    "parameters": {
      "type": "object",
      "properties": {
        "accountId": {
          "type": "integer",
          "description": "ID of the connected LinkedIn sender account."
        }
      },
      "required": [
        "accountId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_get_all",
    "description": "List lead/company lists",
    "method": "POST",
    "path": "/list/GetAll",
    "parameters": {
      "type": "object",
      "properties": {
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "keyword": {
          "type": "string",
          "description": "Optional free-text filter applied to list names."
        },
        "listType": {
          "type": "string",
          "description": "Optional filter to restrict to lead lists or company lists."
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_get_by_id",
    "description": "Get list by ID",
    "method": "GET",
    "path": "/list/GetById",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        }
      },
      "required": [
        "listId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_create_empty",
    "description": "Create empty list",
    "method": "POST",
    "path": "/list/CreateEmptyList",
    "parameters": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string",
          "description": "Display name of the list."
        },
        "type": {
          "type": "string",
          "description": "Whether this list contains people (`USER_LIST`) or companies (`COMPANY_LIST`)."
        }
      },
      "required": [
        "name",
        "type"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_get_leads",
    "description": "Get leads from a list",
    "method": "POST",
    "path": "/list/GetLeadsFromList",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "keyword": {
          "type": "string",
          "description": "Optional free-text filter applied to lead first/last name and headline."
        },
        "leadProfileUrl": {
          "type": "string",
          "description": "Optional — restrict to a single lead by LinkedIn profile URL."
        },
        "leadLinkedInId": {
          "type": "string",
          "description": "Optional — restrict to a single lead by HeyReach-stored LinkedIn ID."
        },
        "createdFrom": {
          "type": "string",
          "description": "Inclusive lower bound on the lead's creation timestamp."
        },
        "createdTo": {
          "type": "string",
          "description": "Inclusive upper bound on the lead's creation timestamp."
        }
      },
      "required": [
        "listId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_get_companies",
    "description": "Get companies from a list",
    "method": "POST",
    "path": "/list/GetCompaniesFromList",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "keyword": {
          "type": "string",
          "description": "Optional free-text filter applied to company name."
        }
      },
      "required": [
        "listId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_add_leads_v2",
    "description": "Add leads to list",
    "method": "POST",
    "path": "/list/AddLeadsToListV2",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        },
        "leads": {
          "type": "array",
          "description": "leads"
        }
      },
      "required": [
        "listId",
        "leads"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_add_leads",
    "description": "Add leads to list (v1, legacy)",
    "method": "POST",
    "path": "/list/AddLeadsToList",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        },
        "leads": {
          "type": "array",
          "description": "leads"
        }
      },
      "required": [
        "listId",
        "leads"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_delete_leads",
    "description": "Delete leads from list by member ID",
    "method": "DELETE",
    "path": "/list/DeleteLeadsFromList",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        },
        "leadMemberIds": {
          "type": "array",
          "description": "leadMemberIds"
        }
      },
      "required": [
        "listId",
        "leadMemberIds"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_delete_leads_by_url",
    "description": "Delete leads from list by profile URL",
    "method": "DELETE",
    "path": "/list/DeleteLeadsFromListByProfileUrl",
    "parameters": {
      "type": "object",
      "properties": {
        "listId": {
          "type": "integer",
          "description": "listId"
        },
        "profileUrls": {
          "type": "array",
          "description": "profileUrls"
        }
      },
      "required": [
        "listId",
        "profileUrls"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_list_get_for_lead",
    "description": "Get lists a lead belongs to",
    "method": "POST",
    "path": "/list/GetListsForLead",
    "parameters": {
      "type": "object",
      "properties": {
        "email": {
          "type": "string",
          "description": "email"
        },
        "linkedinId": {
          "type": "string",
          "description": "linkedinId"
        },
        "profileUrl": {
          "type": "string",
          "description": "profileUrl"
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_lead_get_by_profile_url",
    "description": "Get lead by profile URL",
    "method": "POST",
    "path": "/lead/GetLead",
    "parameters": {
      "type": "object",
      "properties": {
        "profileUrl": {
          "type": "string",
          "description": "Public LinkedIn profile URL (e.g. `https://www.linkedin.com/in/janedoe/`)."
        }
      },
      "required": [
        "profileUrl"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_lead_add_tags",
    "description": "Add tags to lead",
    "method": "POST",
    "path": "/lead/AddTags",
    "parameters": {
      "type": "object",
      "properties": {
        "leadProfileUrl": {
          "type": "string",
          "description": "leadProfileUrl"
        },
        "leadLinkedInId": {
          "type": "string",
          "description": "leadLinkedInId"
        },
        "tags": {
          "type": "array",
          "description": "Tag display names to add."
        },
        "createTagIfNotExisting": {
          "type": "boolean",
          "description": "If true, missing tag names are created on the fly."
        }
      },
      "required": [
        "tags"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_lead_replace_tags",
    "description": "Replace lead tags",
    "method": "POST",
    "path": "/lead/ReplaceTags",
    "parameters": {
      "type": "object",
      "properties": {
        "leadProfileUrl": {
          "type": "string",
          "description": "leadProfileUrl"
        },
        "leadLinkedInId": {
          "type": "string",
          "description": "leadLinkedInId"
        },
        "tags": {
          "type": "array",
          "description": "tags"
        },
        "createTagIfNotExisting": {
          "type": "boolean",
          "description": "createTagIfNotExisting"
        }
      },
      "required": [
        "tags"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_lead_get_tags",
    "description": "Get tags for a lead",
    "method": "POST",
    "path": "/lead/GetTags",
    "parameters": {
      "type": "object",
      "properties": {
        "profileUrl": {
          "type": "string",
          "description": "profileUrl"
        }
      },
      "required": [
        "profileUrl"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_lead_tags_create",
    "description": "Create workspace lead tags",
    "method": "POST",
    "path": "/lead_tags/CreateTags",
    "parameters": {
      "type": "object",
      "properties": {
        "tags": {
          "type": "array",
          "description": "tags"
        }
      },
      "required": [
        "tags"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_stats_get_overall",
    "description": "Get overall stats",
    "method": "POST",
    "path": "/stats/GetOverallStats",
    "parameters": {
      "type": "object",
      "properties": {
        "startDate": {
          "type": "string",
          "description": "Inclusive start of the analytics window (ISO 8601)."
        },
        "endDate": {
          "type": "string",
          "description": "Inclusive end of the analytics window (ISO 8601)."
        },
        "campaignIds": {
          "type": "array",
          "description": "Restrict the aggregation to these campaign IDs."
        },
        "accountIds": {
          "type": "array",
          "description": "Restrict the aggregation to these connected LinkedIn sender accounts."
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_stats_get_by_campaign",
    "description": "Get overall stats grouped by campaign",
    "method": "POST",
    "path": "/stats/GetOverallStatsByCampaign",
    "parameters": {
      "type": "object",
      "properties": {
        "startDate": {
          "type": "string",
          "description": "startDate"
        },
        "endDate": {
          "type": "string",
          "description": "endDate"
        },
        "campaignIds": {
          "type": "array",
          "description": "campaignIds"
        },
        "accountIds": {
          "type": "array",
          "description": "accountIds"
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_network_get_for_sender",
    "description": "Get sender's LinkedIn network",
    "method": "POST",
    "path": "/MyNetwork/GetMyNetworkForSender",
    "parameters": {
      "type": "object",
      "properties": {
        "senderId": {
          "type": "integer",
          "description": "ID of the connected LinkedIn sender account whose network should be returned."
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "keyword": {
          "type": "string",
          "description": "Optional free-text filter applied to first/last name and headline."
        }
      },
      "required": [
        "senderId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_network_is_connection",
    "description": "Check if a lead is a 1st-degree connection",
    "method": "POST",
    "path": "/MyNetwork/IsConnection",
    "parameters": {
      "type": "object",
      "properties": {
        "senderAccountId": {
          "type": "integer",
          "description": "ID of the connected LinkedIn sender account."
        },
        "leadProfileUrl": {
          "type": "string",
          "description": "leadProfileUrl"
        },
        "leadLinkedInId": {
          "type": "string",
          "description": "leadLinkedInId"
        }
      },
      "required": [
        "senderAccountId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_webhook_create",
    "description": "Create webhook subscription",
    "method": "POST",
    "path": "/webhooks/CreateWebhook",
    "parameters": {
      "type": "object",
      "properties": {
        "webhookName": {
          "type": "string",
          "description": "webhookName"
        },
        "webhookUrl": {
          "type": "string",
          "description": "webhookUrl"
        },
        "eventType": {
          "type": "string",
          "description": "eventType"
        },
        "campaignIds": {
          "type": "array",
          "description": "Optional — restrict to events originated by these campaigns. Empty/omitted means all campaigns."
        },
        "customHeaders": {
          "type": "object",
          "description": "Optional custom HTTP headers to include on every webhook delivery (e.g. for authentication)."
        }
      },
      "required": [
        "webhookName",
        "webhookUrl",
        "eventType"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_webhook_get_by_id",
    "description": "Get webhook by ID",
    "method": "GET",
    "path": "/webhooks/GetWebhookById",
    "parameters": {
      "type": "object",
      "properties": {
        "webhookId": {
          "type": "integer",
          "description": "webhookId"
        },
        "includeCustomHeaders": {
          "type": "boolean",
          "description": "Set to `true` to include any configured custom HTTP headers in the response."
        }
      },
      "required": [
        "webhookId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_webhook_get_all",
    "description": "List webhooks",
    "method": "POST",
    "path": "/webhooks/GetAllWebhooks",
    "parameters": {
      "type": "object",
      "properties": {
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "includeCustomHeaders": {
          "type": "boolean",
          "description": "includeCustomHeaders"
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_webhook_update",
    "description": "Update webhook subscription",
    "method": "PATCH",
    "path": "/webhooks/UpdateWebhook",
    "parameters": {
      "type": "object",
      "properties": {
        "webhookId": {
          "type": "integer",
          "description": "webhookId"
        },
        "webhookName": {
          "type": "string",
          "description": "webhookName"
        },
        "webhookUrl": {
          "type": "string",
          "description": "webhookUrl"
        },
        "eventType": {
          "type": "string",
          "description": "eventType"
        },
        "campaignIds": {
          "type": "array",
          "description": "campaignIds"
        },
        "isActive": {
          "type": "boolean",
          "description": "isActive"
        },
        "customHeaders": {
          "type": "object",
          "description": "customHeaders"
        }
      },
      "required": [
        "webhookId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_webhook_delete",
    "description": "Delete webhook subscription",
    "method": "DELETE",
    "path": "/webhooks/DeleteWebhook",
    "parameters": {
      "type": "object",
      "properties": {
        "webhookId": {
          "type": "integer",
          "description": "webhookId"
        }
      },
      "required": [
        "webhookId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_workspaces_get_all",
    "description": "List workspaces (organization-scoped key)",
    "method": "GET",
    "path": "/management/organizations/workspaces",
    "parameters": {
      "type": "object",
      "properties": {
        "Offset": {
          "type": "integer",
          "description": "Offset"
        },
        "Limit": {
          "type": "integer",
          "description": "Limit"
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_workspaces_create",
    "description": "Create workspace",
    "method": "POST",
    "path": "/management/organizations/workspaces",
    "parameters": {
      "type": "object",
      "properties": {
        "workspaceName": {
          "type": "string",
          "description": "workspaceName"
        },
        "seatsLimit": {
          "type": "integer",
          "description": "Optional cap on workspace seats."
        }
      },
      "required": [
        "workspaceName"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_workspaces_update",
    "description": "Update workspace",
    "method": "PATCH",
    "path": "/management/organizations/workspaces/{workspaceId}",
    "parameters": {
      "type": "object",
      "properties": {
        "workspaceId": {
          "type": "integer",
          "description": "workspaceId"
        },
        "workspaceName": {
          "type": "string",
          "description": "workspaceName"
        },
        "seatsLimit": {
          "type": "object",
          "description": "Wrap the new seats cap as `{ \"value\": <integer> }`, or pass `null` to remove the cap."
        }
      },
      "required": [
        "workspaceId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_workspace_api_keys_get",
    "description": "Get workspace API keys",
    "method": "GET",
    "path": "/management/organizations/api-keys/workspaces/{workspaceId}",
    "parameters": {
      "type": "object",
      "properties": {
        "workspaceId": {
          "type": "integer",
          "description": "workspaceId"
        }
      },
      "required": [
        "workspaceId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_workspace_api_keys_create",
    "description": "Create workspace API key",
    "method": "POST",
    "path": "/management/organizations/api-keys/workspaces/{workspaceId}",
    "parameters": {
      "type": "object",
      "properties": {
        "workspaceId": {
          "type": "integer",
          "description": "workspaceId"
        },
        "apiKeyType": {
          "type": "string",
          "description": "apiKeyType"
        }
      },
      "required": [
        "workspaceId",
        "apiKeyType"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_users_get_all",
    "description": "List users in the organization",
    "method": "POST",
    "path": "/management/organizations/users",
    "parameters": {
      "type": "object",
      "properties": {
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "role": {
          "type": "string",
          "description": "role"
        },
        "invitationStatus": {
          "type": "array",
          "description": "invitationStatus"
        }
      }
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_user_get_by_id",
    "description": "Get user by ID",
    "method": "GET",
    "path": "/management/organizations/users/{userId}",
    "parameters": {
      "type": "object",
      "properties": {
        "userId": {
          "type": "integer",
          "description": "userId"
        }
      },
      "required": [
        "userId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_workspace_users_get_all",
    "description": "List users in workspace",
    "method": "POST",
    "path": "/management/organizations/users/workspaces/{workspaceId}",
    "parameters": {
      "type": "object",
      "properties": {
        "workspaceId": {
          "type": "integer",
          "description": "workspaceId"
        },
        "offset": {
          "type": "integer",
          "description": "offset"
        },
        "limit": {
          "type": "integer",
          "description": "limit"
        },
        "role": {
          "type": "string",
          "description": "role"
        },
        "invitationStatus": {
          "type": "array",
          "description": "invitationStatus"
        }
      },
      "required": [
        "workspaceId"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_invite_admins",
    "description": "Invite admins",
    "method": "POST",
    "path": "/management/organizations/users/invite/admins",
    "parameters": {
      "type": "object",
      "properties": {
        "inviterEmail": {
          "type": "string",
          "description": "Email of the existing organization user the invitation should appear to come from."
        },
        "emails": {
          "type": "array",
          "description": "emails"
        }
      },
      "required": [
        "inviterEmail",
        "emails"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_invite_managers",
    "description": "Invite managers",
    "method": "POST",
    "path": "/management/organizations/users/invite/managers",
    "parameters": {
      "type": "object",
      "properties": {
        "inviterEmail": {
          "type": "string",
          "description": "inviterEmail"
        },
        "emails": {
          "type": "array",
          "description": "emails"
        },
        "workspaceIds": {
          "type": "array",
          "description": "workspaceIds"
        }
      },
      "required": [
        "inviterEmail",
        "emails",
        "workspaceIds"
      ]
    }
  },
  {
    "name": "superagnt_connection_heyreach_org_invite_members",
    "description": "Invite members",
    "method": "POST",
    "path": "/management/organizations/users/invite/members",
    "parameters": {
      "type": "object",
      "properties": {
        "inviterEmail": {
          "type": "string",
          "description": "inviterEmail"
        },
        "emails": {
          "type": "array",
          "description": "emails"
        },
        "workspaceIds": {
          "type": "array",
          "description": "workspaceIds"
        },
        "permissions": {
          "type": "string",
          "description": "permissions"
        }
      },
      "required": [
        "inviterEmail",
        "emails",
        "workspaceIds",
        "permissions"
      ]
    }
  }
]
```

## Example

```bash
curl -X GET &#x27;https://api.superagnt.com/v1/connections/heyreach/auth/CheckApiKey&#x27; \
  -H &#x27;Authorization: Bearer $SUPERAGNT_API_KEY&#x27;
```

## Use Cases

- Push leads from your data warehouse or enrichment pipeline into running HeyReach campaigns
- Build AI agents that triage HeyReach replies and write them back to your CRM
- Sync the unified inbox into Slack or a CRM with per-campaign routing
- Pause or resume campaigns automatically based on inventory, deliverability, or budget signals
- Generate weekly outreach reports across senders and campaigns from &#x60;/stats/GetOverallStats&#x60;
- Mirror a sender&#x27;s 1st-degree LinkedIn network into a CRM for retargeting workflows

## Links

- [Documentation](https://superagnt.com/r/ch-heyreach-outreach-docs)
- [Dashboard / API keys](https://superagnt.com/r/ch-heyreach-outreach-key)
- [Connections dashboard](https://app.superagnt.com/dashboard/connections)
- [This listing](https://clawhub.ai/superagnt/skills/heyreach-outreach)
