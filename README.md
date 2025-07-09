<div align="center" style="background-color: #111; padding: 100;">
    <a href="https://github.com/Threadfin/Threadfin"><img width="285" height="80" src="html/img/threadfin.png" alt="Threadfin" /></a>
</div>
<br>

# Threadfin
## M3U Proxy for Plex DVR and Emby/Jellyfin Live TV. Based on xTeVe.

## Overview

Threadfin helps you bridge the gap between your IPTV providers and your home media server. Many IPTV services provide access to live TV channels via M3U playlist files and XMLTV for EPG data. However, media servers like Plex, Emby, or Jellyfin may have limitations in how they handle these sources directly, or you might have multiple IPTV subscriptions that you want to manage centrally.

**What problem does it solve?**
Threadfin simplifies the management of IPTV streams by:
- Allowing you to add multiple M3U playlists and XMLTV EPG sources.
- Providing tools to filter, map, and reorder channels.
- Generating a unified, clean M3U playlist and EPG for your media server.
- Offering a buffer to improve stream stability and reduce issues for media servers that are sensitive to stream interruptions.

**What is a realistic use case?**
Imagine you subscribe to two different IPTV services to get a wide range of channels. Instead of struggling to manage two separate M3U files and EPGs in Plex, you can add both sources to Threadfin. Threadfin then lets you pick the channels you want, assign consistent channel numbers, and provides a single M3U and EPG link for Plex. This means a smoother setup and a more reliable Live TV experience.

**When do you need this tool?**
- You use Plex, Emby, or Jellyfin for Live TV and your IPTV provider uses M3U playlists.
- Your media server has trouble with your provider's M3U or EPG format.
- You want to combine channels from multiple IPTV providers.
- You need to filter out unwanted channels or customize channel names and numbers.
- You want a more stable streaming experience by using a buffer.

**What is the experience without this tool?**
Without Threadfin (or a similar tool), you might face:
- Difficulty loading M3U playlists directly into your media server.
- Incomplete or inaccurate EPG data.
- Manually editing M3U files, which can be tedious and error-prone.
- Juggling multiple playlists if you have more than one IPTV provider.
- Stream interruptions or compatibility issues with your media server.

Threadfin aims to make your IPTV experience seamless and more customizable.

## Documentation

Our comprehensive documentation covers setup, configuration, user guides, and more:
- **[Threadfin Documentation](docs/index.md)**

The original xTeVe documentation may still be a useful reference for some underlying concepts or advanced xTeVe-specific configurations not yet covered in our new guides.

### Donation
[Github Sponsor](https://github.com/sponsors/Fyb3roptik)

### Support
- [Discord](https://discord.gg/CNaSkER2zD)

## Requirements
### Plex
* Plex Media Server (1.11.1.4730 or newer)
* Plex Client with DVR support
* Plex Pass

### Emby
* Emby Server (3.5.3.0 or newer)
* Emby Client with Live-TV support
* Emby Premiere

### Jellyfin
* Jellyfin Server (10.7.1 or newer)
* Jellyfin Client with Live-TV support

--- 

## Threadfin Features

* New Bootstrap based UI
* RAM based buffer instead of File based

#### Filter Group
* Can now add a starting channel number for the filter group

#### Map Editor
* Can now multi select Bulk Edit by holding shift
* Now has a separate table for inactive channels
* Can add 3 backup channels for an active channel (backup channels do NOT have to be active)
* Alpha Numeric sorting now sorts correctly
* Can now add a starting channel number for Bulk Edit to renumber multiple channels at a time
* PPV channels can now map the channel name to an EPG
* Removed old Threadfin buffer option, since FFMPEG/VLC will always be a better solution

## xTeVe Features

#### Files
* Merge external M3U files
* Merge external XMLTV files
* Automatic M3U and XMLTV update
* M3U and XMLTV export

#### Channel management
* Filtering streams
* Channel mapping
* Channel order
* Channel logos
* Channel categories

#### Streaming
* Buffer with HLS / M3U8 support
* Re-streaming
* Number of tuners adjustable
* Compatible with Plex / Emby / Jellyfin EPG

---

## Docker Image
[Threadfin](https://hub.docker.com/r/fyb3roptik/threadfin)

* Docker compose example

```
version: "2.3"
services:
  threadfin:
    image: fyb3roptik/threadfin
    container_name: threadfin
    ports:
      - 34400:34400
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=America/Los_Angeles
    volumes:
      - ./data/conf:/home/threadfin/conf
      - ./data/temp:/tmp/threadfin:rw
    restart: unless-stopped
```

---                                                                                             

## Helm Chart on Kubernetes using TrueCharts
[Threadfin](https://truecharts.org/charts/stable/threadfin/)

* Helm-Chart Installation
```helm install threadfin oci://tccr.io/truecharts/threadfin```

---

### Threadfin Beta branch
New features and bug fixes are only available in beta branch. Only after successful testing are they are merged into the main branch.

**It is not recommended to use the beta version in a production system.**  

With the command line argument `branch` the Git Branch can be changed. Threadfin must be started via the terminal.  

#### Switch from master to beta branch:
```
threadfin -branch beta

...
[Threadfin] GitHub:                https://github.com/Threadfin/Threadfin
[Threadfin] Git Branch:            beta [Threadfin]
...
```

#### Switch from beta to master branch:
```
threadfin -branch main

...
[Threadfin] GitHub:                https://github.com/Threadfin/Threadfin
[Threadfin] Git Branch:            main [Threadfin]
...
```

When the branch is changed, an update is only performed if there is a new version and the update function is activated in the settings.  

---

## Build from source code [Go / Golang]

#### Requirements
* [Go](https://golang.org) (go1.18 or newer)

#### Dependencies
* [go-ssdp](https://github.com/koron/go-ssdp)
* [websocket](https://github.com/gorilla/websocket)
* [osext](https://github.com/kardianos/osext)
* [avfs](github.com/avfs/avfs)

#### Build
1. Download source code
2. Install dependencies
```
go mod tidy && go mod vendor
```
3. Build Threadfin
```
go build threadfin.go
```

4. Update web files (optional)

If TypeScript files were changed, run:

```sh
tsc -p ./ts/tsconfig.json
```

Then, to embed updated JavaScript files into the source code (src/webUI.go), run it in development mode at least once:

```sh
go build threadfin.go
threadfin -dev
```

---

## Fork without pull request :mega:
When creating a fork, the Threadfin GitHub account must be changed from the source code or the update function disabled.
Future updates of Threadfin would update your fork.

threadfin.go - Line: 29
```Go
var GitHub = GitHubStruct{Branch: "main", User: "Threadfin", Repo: "Threadfin", Update: true}

/*
  Branch: GitHub Branch
  User:   GitHub Username
  Repo:   GitHub Repository
  Update: Automatic updates from the GitHub repository [true|false]
*/

```


