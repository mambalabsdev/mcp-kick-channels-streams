# Kick Channels, Streams, and Clips MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-kick-channels-streams%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-kick-channels-streams&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-kick-channels-streams)](https://www.npmjs.com/package/@mambalabsdev/mcp-kick-channels-streams) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-kick-channels-streams)](https://www.npmjs.com/package/@mambalabsdev/mcp-kick-channels-streams) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-kick-channels-streams)](https://github.com/mambalabsdev/mcp-kick-channels-streams/blob/main/LICENSE)

An MCP server for the Mamba Labs Kick Channels, Streams, and Clips actor on Apify. Kick.com channels, live streams, and clips as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes Kick channel names, links, or search words and returns one row per channel with followers, socials, and live status, with optional past broadcasts and clips, and can list streams live now by category and language.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-kick-channels-streams": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-kick-channels-streams"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_kick_channels_streams` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "How many followers does the Kick channel xqc have, and is it live now?"
- "List the 20 most viewed clips of adinross on Kick."
- "Which English streams are live on Kick in Just Chatting right now, by viewers?"

## Tool

`get_kick_channels_streams`. One tool. Example arguments:

```json
{
  "channels": [
    "xqc"
  ],
  "include_clips": true,
  "max_clips_per_channel": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `channels` | array | no | Kick channel names (xqc or @xqc) or links (https://kick.com/xqc). Each channel returns one row with followers, verified mark, bio, social links, recent categories, and live status. |
| `search` | array | no | Optional. Words to search Kick channels with, such as poker or valorant. Each word returns its top channels, and each found channel gets a channel row. |
| `max_channels_per_search` | number | no | How many channels to keep from each search word, in Kick's order. Default 5, maximum 20. |
| `include_videos` | boolean | no | yes to add one row per past broadcast (VOD) of each channel, newest first. Default no. |
| `max_videos_per_channel` | number | no | Past broadcasts to return per channel. Kick keeps a limited history, often the last 30 days. Default 20, maximum 500. |
| `include_clips` | boolean | no | yes to add one row per clip of each channel. Default no. |
| `max_clips_per_channel` | number | no | Clips to return per channel. Kick sends 20 a page. Default 20, maximum 1000. |
| `clips_sort` | string | no | view (default): most viewed first. date: newest first, in Kick's order by clip ID, where clips made within the same minute can appear slightly out of time order. |
| `clips_period` | string | no | Which clips: day, week, month, or all (default). |
| `live_now` | boolean | no | yes to return streams live now, most viewers first, one row per stream. Default no. |
| `live_category` | string | no | Optional, with live_now. A Kick category as it appears in its kick.com link: a main category (games, irl, music, gambling, creative, alternative) or a subcategory such as just-chatting, slots, or valorant. |
| `live_language` | string | no | Optional, with live_now. Keep only streams in this language, such as English, Spanish, or en. |
| `max_live_streams` | number | no | Live streams to return, with live_now. Default 50, maximum 1000. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `channel-row`, one channel | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `video-row`, one past broadcast | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `clip-row`, one clip | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `live-stream-row`, one live stream | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Channels that do not exist, banned channels, search and live summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/kick-channels-streams](https://apify.com/mambalabs/kick-channels-streams)

## License

MIT. See [LICENSE](LICENSE).
