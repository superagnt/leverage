# SEO API Reference

**App name:** `seo`
**Base URL:** `https://api.superagnt.com/v1/data/seo`
**Endpoints:** 16

Live Google results, keyword research, competitor and backlink gaps, page audits and AI answer visibility, all as agent tools.

## Authentication

All requests require the superagnt API key:

```
Authorization: Bearer $SUPERAGNT_API_KEY
```

---

## Available Endpoints

| Method | Path | Summary |
|--------|------|---------|
| `POST` | `/serp` | Live Google search results |
| `POST` | `/serp-ai-mode` | Google AI Mode results |
| `POST` | `/keyword-ideas` | Keyword ideas from seed keywords |
| `POST` | `/keyword-overview` | Metrics for specific keywords |
| `POST` | `/search-intent` | Classify keyword search intent |
| `POST` | `/ranked-keywords` | Keywords a domain ranks for |
| `POST` | `/keyword-gap` | Keyword gap between two domains |
| `POST` | `/competitors` | Find a domain's organic competitors |
| `POST` | `/domain-overview` | Domain rank and traffic overview |
| `POST` | `/backlinks-summary` | Backlink profile summary |
| `POST` | `/referring-domains` | Referring domains for a target |
| `POST` | `/backlink-gap` | Domains linking to competitors but not you |
| `POST` | `/page-audit` | Instant on-page audit of a URL |
| `POST` | `/ai-visibility` | How often LLMs mention a domain |
| `POST` | `/tech-stack` | Technologies a website runs |
| `POST` | `/tech-search` | Find websites by technology |

## Tool Schemas

The following JSON defines all available tools with their parameters. Each tool maps to an API endpoint.

```json
[
  {
    "name": "superagnt_seo_google_search",
    "description": "Live Google search results",
    "method": "POST",
    "path": "/serp",
    "parameters": {
      "type": "object",
      "properties": {
        "keyword": {
          "type": "string",
          "description": "The search query. Supports Google search operators (site:, inurl:, filetype:), which cost roughly 5x."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Common values: 2840 United States (default), 2826 United Kingdom, 2124 Canada, 2036 Australia, 2276 Germany, 2250 France, 2724 Spain, 2356 India, 2076 Brazil."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code, e.g. 'en' (default), 'de', 'es', 'fr', 'pt'."
        },
        "device": {
          "type": "string",
          "description": "Device type to emulate. Default 'desktop'."
        },
        "depth": {
          "type": "integer",
          "description": "Number of results to return, in steps of 10. Default 10 (cheapest). Each additional 10 results bills as one more results page."
        }
      },
      "required": [
        "keyword"
      ]
    }
  },
  {
    "name": "superagnt_seo_google_ai_mode_search",
    "description": "Google AI Mode results",
    "method": "POST",
    "path": "/serp-ai-mode",
    "parameters": {
      "type": "object",
      "properties": {
        "keyword": {
          "type": "string",
          "description": "The question or query to run through Google AI Mode."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Common values: 2840 United States (default), 2826 United Kingdom, 2124 Canada, 2036 Australia, 2276 Germany."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code, e.g. 'en' (default)."
        }
      },
      "required": [
        "keyword"
      ]
    }
  },
  {
    "name": "superagnt_seo_keyword_ideas",
    "description": "Keyword ideas from seed keywords",
    "method": "POST",
    "path": "/keyword-ideas",
    "parameters": {
      "type": "object",
      "properties": {
        "keywords": {
          "type": "array",
          "description": "Seed keywords to expand (1-200)."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Default 2840 (United States)."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        },
        "limit": {
          "type": "integer",
          "description": "Max keyword ideas to return. Default 100. Cost scales with rows, so raise only when needed."
        },
        "offset": {
          "type": "integer",
          "description": "Pagination offset."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression as [field, operator, value] triples joined by 'and'/'or'. Operators: =, <>, <, <=, >, >=, in, not_in, like, not_like (use %% wildcards with like). Recipes: [[\"keyword_info.search_volume\",\">\",100]] · keep only on-topic ideas: [[\"keyword\",\"like\",\"%deliverability%\"]] · combine: [[\"keyword_info.search_volume\",\">\",100],\"and\",[\"keyword_properties.keyword_difficulty\",\"<\",40]]."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules, e.g. [\"keyword_info.search_volume,desc\"]."
        }
      },
      "required": [
        "keywords"
      ]
    }
  },
  {
    "name": "superagnt_seo_keyword_overview",
    "description": "Metrics for specific keywords",
    "method": "POST",
    "path": "/keyword-overview",
    "parameters": {
      "type": "object",
      "properties": {
        "keywords": {
          "type": "array",
          "description": "The exact keywords to look up (1-700). Cost scales with the number of keywords."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Default 2840 (United States)."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        }
      },
      "required": [
        "keywords"
      ]
    }
  },
  {
    "name": "superagnt_seo_search_intent",
    "description": "Classify keyword search intent",
    "method": "POST",
    "path": "/search-intent",
    "parameters": {
      "type": "object",
      "properties": {
        "keywords": {
          "type": "array",
          "description": "Keywords to classify (1-1000). Cost scales with the number of keywords."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        }
      },
      "required": [
        "keywords"
      ]
    }
  },
  {
    "name": "superagnt_seo_ranked_keywords",
    "description": "Keywords a domain ranks for",
    "method": "POST",
    "path": "/ranked-keywords",
    "parameters": {
      "type": "object",
      "properties": {
        "target": {
          "type": "string",
          "description": "Domain (e.g. 'example.com', no protocol) or full page URL to analyze."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Default 2840 (United States)."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        },
        "limit": {
          "type": "integer",
          "description": "Max keywords to return. Default 100. Cost scales with rows."
        },
        "offset": {
          "type": "integer",
          "description": "Pagination offset."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression, e.g. [[\"ranked_serp_element.serp_item.rank_absolute\", \"<=\", 20]]."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules, e.g. [\"keyword_data.keyword_info.search_volume,desc\"]."
        }
      },
      "required": [
        "target"
      ]
    }
  },
  {
    "name": "superagnt_seo_keyword_gap",
    "description": "Keyword gap between two domains",
    "method": "POST",
    "path": "/keyword-gap",
    "parameters": {
      "type": "object",
      "properties": {
        "target1": {
          "type": "string",
          "description": "First domain (e.g. your domain), no protocol."
        },
        "target2": {
          "type": "string",
          "description": "Second domain (e.g. a competitor), no protocol."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Default 2840 (United States)."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        },
        "limit": {
          "type": "integer",
          "description": "Max intersecting keywords to return. Default 100."
        },
        "offset": {
          "type": "integer",
          "description": "Pagination offset."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression as [field, operator, value] triples joined by 'and'/'or'. Operators: =, <>, <, <=, >, >=, in, not_in, like, not_like. Recipe (their wins, your gaps): [[\"first_domain_serp_element.rank_absolute\",\">\",20],\"and\",[\"second_domain_serp_element.rank_absolute\",\"<=\",10]]."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules."
        }
      },
      "required": [
        "target1",
        "target2"
      ]
    }
  },
  {
    "name": "superagnt_seo_competitors",
    "description": "Find a domain's organic competitors",
    "method": "POST",
    "path": "/competitors",
    "parameters": {
      "type": "object",
      "properties": {
        "target": {
          "type": "string",
          "description": "Domain to find competitors for, no protocol."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Default 2840 (United States)."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        },
        "limit": {
          "type": "integer",
          "description": "Max competitor domains to return. Default 100."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression as [field, operator, value] triples. Recipe: [[\"intersections\",\">\",50]] keeps only domains sharing 50+ keywords with the target."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules, e.g. [\"intersections,desc\"]."
        }
      },
      "required": [
        "target"
      ]
    }
  },
  {
    "name": "superagnt_seo_domain_overview",
    "description": "Domain rank and traffic overview",
    "method": "POST",
    "path": "/domain-overview",
    "parameters": {
      "type": "object",
      "properties": {
        "target": {
          "type": "string",
          "description": "Domain to summarize, no protocol."
        },
        "location_code": {
          "type": "integer",
          "description": "Geo target as a location code. Default 2840 (United States)."
        },
        "language_code": {
          "type": "string",
          "description": "Two-letter language code. Default 'en'."
        }
      },
      "required": [
        "target"
      ]
    }
  },
  {
    "name": "superagnt_seo_backlinks_summary",
    "description": "Backlink profile summary",
    "method": "POST",
    "path": "/backlinks-summary",
    "parameters": {
      "type": "object",
      "properties": {
        "target": {
          "type": "string",
          "description": "Domain, subdomain or full page URL. Domains without protocol; pages with protocol."
        },
        "include_subdomains": {
          "type": "boolean",
          "description": "Count links to subdomains too. Default true."
        },
        "exclude_internal_backlinks": {
          "type": "boolean",
          "description": "Ignore links from the target to itself. Default true."
        }
      },
      "required": [
        "target"
      ]
    }
  },
  {
    "name": "superagnt_seo_referring_domains",
    "description": "Referring domains for a target",
    "method": "POST",
    "path": "/referring-domains",
    "parameters": {
      "type": "object",
      "properties": {
        "target": {
          "type": "string",
          "description": "Domain, subdomain or full page URL to list referring domains for."
        },
        "limit": {
          "type": "integer",
          "description": "Max referring domains to return. Default 100. Cost scales with rows."
        },
        "offset": {
          "type": "integer",
          "description": "Pagination offset."
        },
        "exclude_internal_backlinks": {
          "type": "boolean",
          "description": "Ignore links from the target to itself. Default true."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression as [field, operator, value] triples. Operators: =, <>, <, <=, >, >=, in, not_in, like, not_like. Recipe: [[\"rank\",\">\",100]] keeps stronger referring domains (rank is the referring domain's authority, higher is better)."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules, e.g. [\"rank,desc\"]."
        }
      },
      "required": [
        "target"
      ]
    }
  },
  {
    "name": "superagnt_seo_backlink_gap",
    "description": "Domains linking to competitors but not you",
    "method": "POST",
    "path": "/backlink-gap",
    "parameters": {
      "type": "object",
      "properties": {
        "targets": {
          "type": "array",
          "description": "Competitor domains whose backlink sources to intersect (1-20)."
        },
        "exclude_targets": {
          "type": "array",
          "description": "Domains the referring sites must NOT already link to. Put your own domain here."
        },
        "limit": {
          "type": "integer",
          "description": "Max referring domains to return. Default 100."
        },
        "offset": {
          "type": "integer",
          "description": "Pagination offset."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression, e.g. minimum domain rank."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules, e.g. [\"1.rank,desc\"]."
        }
      },
      "required": [
        "targets"
      ]
    }
  },
  {
    "name": "superagnt_seo_page_audit",
    "description": "Instant on-page audit of a URL",
    "method": "POST",
    "path": "/page-audit",
    "parameters": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "description": "The page URL to audit (with protocol)."
        },
        "enable_javascript": {
          "type": "boolean",
          "description": "Render JavaScript before auditing. Slower; only for JS-rendered pages."
        }
      },
      "required": [
        "url"
      ]
    }
  },
  {
    "name": "superagnt_seo_ai_visibility",
    "description": "How often LLMs mention a domain",
    "method": "POST",
    "path": "/ai-visibility",
    "parameters": {
      "type": "object",
      "properties": {
        "targets": {
          "type": "array",
          "description": "Domains to measure LLM mention metrics for, no protocol. Pass at least one of targets or keywords."
        },
        "keywords": {
          "type": "array",
          "description": "Keyword phrases to measure LLM mention metrics for, as an alternative or addition to domains."
        },
        "limit": {
          "type": "integer",
          "description": "Max rows to return. Default 100. Cost scales with rows."
        }
      }
    }
  },
  {
    "name": "superagnt_seo_tech_stack",
    "description": "Technologies a website runs",
    "method": "POST",
    "path": "/tech-stack",
    "parameters": {
      "type": "object",
      "properties": {
        "target": {
          "type": "string",
          "description": "Domain to inspect, no protocol."
        }
      },
      "required": [
        "target"
      ]
    }
  },
  {
    "name": "superagnt_seo_tech_search",
    "description": "Find websites by technology",
    "method": "POST",
    "path": "/tech-search",
    "parameters": {
      "type": "object",
      "properties": {
        "technologies": {
          "type": "array",
          "description": "Technology names the sites must run, e.g. [\"Shopify\", \"Klaviyo\"]. Multiple entries intersect."
        },
        "limit": {
          "type": "integer",
          "description": "Max domains to return. Default 100. Per-row cost is high; raise deliberately."
        },
        "offset": {
          "type": "integer",
          "description": "Pagination offset."
        },
        "filters": {
          "type": "array",
          "description": "Optional filter expression, e.g. [[\"country_iso_code\", \"=\", \"US\"]]."
        },
        "order_by": {
          "type": "array",
          "description": "Sort rules, e.g. [\"domain_rank,desc\"]."
        }
      },
      "required": [
        "technologies"
      ]
    }
  }
]
```

## Example

```bash
curl -X POST "https://api.superagnt.com/v1/data/seo/serp" \
  -H "Authorization: Bearer $SUPERAGNT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"key": "value"}'
```

## Links

- [Documentation](https://superagnt.com)
- [API Reference](https://superagnt.com/apis/social/seo)
- [Dashboard](https://app.superagnt.com/dashboard)
