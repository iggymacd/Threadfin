# Frequently Asked Questions (FAQ)

This FAQ provides answers to common questions about Threadfin.

## General

**Q1: What is Threadfin?**
A: Threadfin is an M3U proxy for Plex DVR, Emby Live TV, and Jellyfin Live TV. It allows you to use custom M3U playlists and XMLTV EPG sources with these media servers, effectively acting as a virtual HDHomeRun tuner. It is based on the xTeVe project but includes additional features and a new UI.

**Q2: What are the main differences between Threadfin and xTeVe?**
A: Threadfin builds upon xTeVe with features like:
*   A new Bootstrap-based web UI.
*   RAM-based buffering (internal characteristic, user still selects None/FFmpeg/VLC for buffering mode).
*   Enhancements to Filter Groups (e.g., starting channel number).
*   Significant improvements to the Map Editor (multi-select, separate inactive table, backup channels, bulk renumbering, PPV mapping).
*   The direct "xTeVe internal buffer" option has been removed in favor of FFmpeg/VLC or no buffer.

**Q3: Is Threadfin free?**
A: Yes, Threadfin is open-source software.

**Q4: Where can I get support for Threadfin?**
A: The primary places for support are:
*   The official [Threadfin GitHub Issues](https://github.com/Threadfin/Threadfin/issues) page for bug reports and feature requests.
*   The Threadfin Discord server (link usually found in the project's main `README.md`) for community discussion and help.

## Installation & Setup

**Q5: What's the easiest way to install Threadfin?**
A: Using Docker is generally the recommended and easiest method. See the [Installation Guide](getting-started/installation.md).

**Q6: Can I run Threadfin on Windows/macOS/Linux?**
A: Yes, Threadfin can be run on various platforms either via Docker, direct binary, or by building from source.

**Q7: What is XEPG mode? Should I use it?**
A: XEPG mode means Threadfin manages your Electronic Program Guide (EPG) data using XMLTV files you provide. This enables advanced features like channel mapping, custom EPG categories, and a single EPG source for all clients. It is generally the recommended mode for the best experience. The alternative is "PMS" mode (or client-side EPG), where your media server (Plex/Emby/Jellyfin) handles EPG data directly.

**Q8: How many tuners should I configure?**
A: This depends on:
*   How many concurrent streams your IPTV provider allows.
*   How many streams your clients will watch simultaneously.
*   The resources (CPU/RAM/Network) of your Threadfin server.
Start with a conservative number (e.g., 1-5) and monitor.

## Playlists & EPG

**Q9: My M3U playlist URL has a username and password. How do I add it?**
A: Simply include the username and password directly in the URL when adding the playlist to Threadfin, e.g., `http://user:pass@provider.com/playlist.m3u`.

**Q10: Why are no channels showing up after adding my M3U playlist?**
A: Possible reasons:
*   The M3U URL is incorrect or the provider is unreachable.
*   The M3U file is empty or malformed.
*   Your [Filters](user-guide/filters.md) are too restrictive and are excluding all channels. Try disabling filters to test.
*   You haven't run an update on the playlist yet in Threadfin.

**Q11: How do I get EPG data for my channels?**
A: You need an XMLTV EPG source. Many IPTV providers supply an XMLTV URL, or you can find third-party XMLTV providers (some free, some paid). Add this URL in Threadfin's XMLTV/EPG section and then map your channels to the EPG data in the [Channel Mapping](user-guide/channel-mapping.md) section.

**Q12: What does `tvg-id` mean in M3U and `channel id` in XMLTV?**
A: These are identifiers used to link a channel in your M3U playlist to its corresponding EPG data in an XMLTV file. For automatic EPG mapping in Threadfin, the `tvg-id` from the M3U should match the `channel id` in the XMLTV.

## Streaming & Playback

**Q13: What is stream buffering in Threadfin? Should I use FFmpeg or VLC?**
A: Buffering helps stabilize streams and can allow multiple clients to watch the same stream using only one connection to your provider.
*   **None:** Threadfin passes the stream directly to the client. Lowest server resource usage.
*   **FFmpeg:** Recommended for most users needing a buffer. It's versatile and stable for remuxing streams.
*   **VLC:** Another option for buffering.
You'll need to provide the path to the FFmpeg or VLC executable in Threadfin's settings.

**Q14: My streams are buffering a lot or failing to play.**
A: See the [Troubleshooting Guide](troubleshooting.md#4-streaming-problems-playback-issues). Common causes include provider issues, tuner limits, network congestion, or incorrect buffer settings.

## Plex/Emby/Jellyfin Integration

**Q15: Why can't Plex/Emby/Jellyfin find Threadfin as an HDHomeRun tuner?**
A:
*   Ensure Threadfin is running and accessible on the same network.
*   SSDP (device discovery protocol) might be blocked by your firewall or network configuration (especially with Docker). Try manually adding Threadfin's IP address and port (e.g., `192.168.1.X:34400`) in your media server's DVR setup.

**Q16: Channels are showing in Threadfin but not in my Plex/Emby/Jellyfin guide.**
A:
*   Ensure channels are **active** and **mapped** to an EPG source (or Dummy EPG) in Threadfin's Channel Mapping.
*   Verify your media server is configured to use Threadfin's XMLTV URL for EPG data.
*   Trigger a guide refresh in your media server. Client-side EPG caching can be aggressive.

## Development & Contribution

**Q17: How can I contribute to Threadfin?**
A: Please see the [Contribution Guidelines](developer-guide/contribution-guidelines.md). Contributions like code, documentation, bug reports, and feature suggestions are welcome.

*(This FAQ will be expanded as more common questions arise.)*
