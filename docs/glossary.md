# Glossary

This glossary defines common terms related to Threadfin and IPTV technology.

---

**API (Application Programming Interface)**
:   A set of rules and protocols that allows different software applications to communicate with each other. Threadfin has an API for programmatic control.

**Authentication**
:   The process of verifying the identity of a user or process trying to access a system. Threadfin supports authentication for its web UI and data feeds.

**Backup Channel**
:   A feature in Threadfin's [Channel Mapping](user-guide/channel-mapping.md) that allows you to specify alternative streams for a channel if the primary stream fails.

**Bootstrap**
:   A popular front-end framework for developing responsive and mobile-first websites. Threadfin's UI is based on Bootstrap.

**Buffer (Stream Buffer)**
:   A temporary storage area for video data. Buffering can help ensure smooth playback by pre-loading parts of a stream and absorbing network jitter. Threadfin can use FFmpeg or VLC for buffering, or pass streams directly (no buffer).

**Channel Mapping**
:   The process within Threadfin of linking channels from your M3U playlists to EPG data from XMLTV sources, and customizing channel properties like name, number, and logo.

**CLI (Command Line Interface)**
:   A text-based interface used to interact with software. Threadfin can be started and configured to some extent via CLI options.

**Docker**
:   A platform for developing, shipping, and running applications in containers. Running Threadfin in Docker is a recommended installation method.

**Dummy EPG**
:   A feature in Threadfin that generates placeholder EPG data for channels that do not have actual guide information from an XMLTV source. This allows the channel to be tunable in clients.

**DVR (Digital Video Recorder)**
:   In the context of Plex, Emby, and Jellyfin, refers to their Live TV and DVR functionalities. Threadfin emulates an HDHomeRun DVR tuner for these media servers.

**Emby**
:   A personal media server application, similar to Plex and Jellyfin, that can use Threadfin for Live TV.

**EPG (Electronic Program Guide)**
:   An on-screen guide that displays scheduling information for television programs. Threadfin uses XMLTV files to provide EPG data.

**FFmpeg**
:   A powerful open-source multimedia framework used by Threadfin (optionally) for stream buffering and remuxing.

**Filter**
:   A set of rules in Threadfin to selectively include or exclude channels from your M3U playlists before they reach the mapping stage.

**Go (Golang)**
:   The programming language Threadfin is primarily written in.

**Group Title (`group-title`)**
:   An attribute in M3U playlists used to categorize channels (e.g., "News," "Sports," "Movies"). Threadfin uses this for its Group Filter feature.

**HDHomeRun (HDHR)**
:   A brand of network-attached television tuners. Threadfin emulates an HDHomeRun device to make its channels discoverable by media servers like Plex, Emby, and Jellyfin.

**Helm Chart**
:   A package format for Kubernetes, simplifying the deployment and management of applications. Threadfin has a Helm chart available via TrueCharts.

**HLS (HTTP Live Streaming)**
:   A common adaptive bitrate streaming protocol. FFmpeg/VLC in Threadfin might remux streams to HLS for client compatibility.

**IPTV (Internet Protocol Television)**
:   The delivery of television content over Internet Protocol networks. M3U playlists are often used to access IPTV streams.

**Jellyfin**
:   An open-source, free software personal media server, similar to Plex and Emby, that can use Threadfin for Live TV.

**JSON (JavaScript Object Notation)**
:   A lightweight data-interchange format. Threadfin uses JSON for its configuration files and API responses.

**Kubernetes (K8s)**
:   An open-source container orchestration system for automating software deployment, scaling, and management.

**M3U / M3U8**
:   A file format for multimedia playlists. M3U files typically contain a list of URLs to audio or video streams. `.m3u8` often denotes HLS playlists. Threadfin uses M3U files as a primary source for channels.

**Mapping**
:   See **Channel Mapping**.

**Plex**
:   A popular personal media server application that can use Threadfin for its Live TV & DVR features.

**Playlist (M3U Playlist)**
:   A list of media streams, typically defined in an M3U file, that Threadfin uses as a source for channels.

**PPV (Pay-Per-View)**
:   A type of service where users pay for individual events. Threadfin has features to better map PPV channels.

**Proxy**
:   An intermediary server that acts on behalf of another server or client. Threadfin acts as a proxy for M3U streams and EPG data.

**Remuxing**
:   The process of changing the container format of a media file or stream without re-encoding the audio or video content. This is much faster and less CPU-intensive than transcoding. FFmpeg/VLC in Threadfin primarily remux streams.

**Reverse Proxy**
:   A server that sits in front of web servers (like Threadfin's UI) and forwards client requests to those servers. Often used for SSL/TLS termination, load balancing, and security.

**SSDP (Simple Service Discovery Protocol)**
:   A network protocol used for advertising and discovering network services. HDHomeRun devices (and Threadfin emulating one) use SSDP to be found by clients.

**Transcoding**
:   The process of converting a media file or stream from one encoding format to another (e.g., changing video codec or resolution). This is CPU-intensive and generally avoided by Threadfin's default buffer configurations.

**`tvg-id` (TellyVera Guide ID)**
:   An attribute in M3U playlists that specifies an identifier for a channel, used for matching it with EPG data in an XMLTV file.

**`tvg-logo`**
:   An attribute in M3U playlists that specifies a URL for a channel's logo.

**`tvg-name`**
:   An attribute in M3U playlists that specifies an EPG-specific name for a channel.

**TypeScript**
:   A superset of JavaScript that adds static typing. Threadfin's frontend UI is written in TypeScript.

**UI (User Interface)**
:   The means by which a user interacts with a software application. Threadfin has a web-based UI.

**VLC (VideoLAN Client)**
:   A free and open-source cross-platform multimedia player and framework. Threadfin can optionally use VLC for stream buffering.

**XEPG (XMLTV EPG Proxy/Provider)**
:   A mode in Threadfin (and xTeVe) where the application itself manages and provides EPG data to clients via an XMLTV feed. This is opposed to "PMS" mode where the media server handles EPG.

**XMLTV**
:   An XML-based file format for storing and exchanging television program guide (EPG) data. Threadfin uses XMLTV files as EPG sources when in XEPG mode.
