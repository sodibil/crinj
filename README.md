

#### Crin.j discord bot repository based on Muse source code.

## Features

- 🎥 Livestreams
- ⏩ Seeking within a song/video
- 💾 Local caching for better performance
- 📋 No vote-to-skip - this is anarchy, not a democracy
- ↔️ Autoconverts playlists / artists / albums / songs from Spotify
- ↗️ Users can add custom shortcuts (aliases)
- 1️⃣ Muse instance supports multiple guilds
- 🔊 Normalizes volume across tracks
- ✍️ Written in TypeScript, easily extendable
- ❤️ Loyal Packers fan

## Running

### Environment variables:

- `DISCORD_TOKEN` can be acquired [here](https://discordapp.com/developers/applications) by creating a 'New Application', then going to 'Bot'.
- `SPOTIFY_CLIENT_ID` and `SPOTIFY_CLIENT_SECRET` can be acquired [here](https://developer.spotify.com/dashboard/applications) with 'Create a Client ID'.
- `YOUTUBE_API_KEY` can be acquired by [creating a new project](https://console.developers.google.com) in Google's Developer Console, enabling the YouTube API, and creating an API key under credentials.

### 🐳 Docker

There are a variety of image tags available:
- `:2`: versions >= 2.0.0
- `:2.1`: versions >= 2.1.0 and < 2.2.0
- `:2.1.1`: an exact version specifier
- `:latest`: whatever the latest version is

(Replace empty config strings with correct values.)

```bash
docker run -it -v "$(pwd)/data":/data -e DISCORD_TOKEN='' -e SPOTIFY_CLIENT_ID='' -e SPOTIFY_CLIENT_SECRET='' -e YOUTUBE_API_KEY='' ghcr.io/museofficial/muse:latest
```

This starts Muse and creates a data directory in your current directory.

**Docker Compose**:

```yaml
version: '3.4'

services:
  muse:
    image: ghcr.io/museofficial/muse:latest
    restart: always
    volumes:
      - ./muse:/data
    environment:
      - DISCORD_TOKEN=
      - YOUTUBE_API_KEY=
      - SPOTIFY_CLIENT_ID=
      - SPOTIFY_CLIENT_SECRET=
```

### Node.js

**Prerequisites**:
* Node.js (18.17.0 or later is required and latest 18.x.x LTS is recommended)
* ffmpeg (4.1 or later)

1. `git clone https://github.com/museofficial/muse.git && cd muse`
2. Copy `.env.example` to `.env` and populate with values
3. I recommend checking out a tagged release with `git checkout v[latest release]`
4. `yarn install` (or `npm i`)
5. `yarn start` (or `npm run start`)

**Note**: if you're on Windows, you may need to manually set the ffmpeg path. See [#345](https://github.com/museofficial/muse/issues/345) for details.

## ⚙️ Additional configuration (advanced)

### Cache

By default, Muse limits the total cache size to around 2 GB. If you want to change this, set the environment variable `CACHE_LIMIT`. For example, `CACHE_LIMIT=512MB` or `CACHE_LIMIT=10GB`.

### SponsorBlock

Muse can skip non-music segments at the beginning or end of a Youtube music video (Using [SponsorBlock](https://sponsor.ajay.app/)). It is disabled by default. If you want to enable it, set the environment variable `ENABLE_SPONSORBLOCK=true` or uncomment it in your .env.
Being a community project, the server may be down or overloaded. When it happen, Muse will skip requests to SponsorBlock for a few minutes. You can change the skip duration by setting the value of `SPONSORBLOCK_TIMEOUT`.

### Custom Bot Status

In the default state, Muse has the status "Online" and the text "Listening to Music". You can change the status through environment variables:

- `BOT_STATUS`:
  - `online` (Online)
  - `idle` (Away)
  - `dnd` (Do not Disturb)

- `BOT_ACTIVITY_TYPE`:
  - `PLAYING` (Playing XYZ)
  - `LISTENING` (Listening to XYZ)
  - `WATCHING` (Watching XYZ)
  - `STREAMING` (Streaming XYZ)

- `BOT_ACTIVITY`: the text that follows the activity type

- `BOT_ACTIVITY_URL` If you use `STREAMING` you MUST set this variable, otherwise it will not work! Here you write a regular YouTube or Twitch Stream URL.

#### Examples

**Muse is watching a movie and is DND**:
- `BOT_STATUS=dnd`
- `BOT_ACTIVITY_TYPE=WATCHING`
- `BOT_ACTIVITY=a movie`

**Muse is streaming Monstercat**:
- `BOT_STATUS=online`
- `BOT_ACTIVITY_TYPE=STREAMING`
- `BOT_ACTIVITY_URL=https://www.twitch.tv/monstercat`
- `BOT_ACTIVITY=Monstercat`

### Bot-wide commands

If you have Muse running in a lot of guilds (10+) you may want to switch to registering commands bot-wide rather than for each guild. (The downside to this is that command updates can take up to an hour to propagate.) To do this, set the environment variable `REGISTER_COMMANDS_ON_BOT` to `true`.
