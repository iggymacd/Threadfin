# Playlists (M3U / HDHomeRun)

Threadfin uses M3U playlists and can discover HDHomeRun tuners to source your channels. This section explains how to add and manage these sources.

Access this section via the **Playlists** item in the main navigation menu.

## Overview

The Playlists page typically displays a list of your configured M3U playlists and HDHomeRun tuners. For each entry, you might see:

*   **Name:** The custom name you assigned to the playlist/tuner.
*   **Type:** M3U or HDHomeRun.
*   **URL/IP Address:** The source URL for M3U or IP for HDHomeRun.
*   **Status:** Indicates if the playlist was successfully updated, any errors, etc.
*   **Last Update:** Timestamp of the last successful update.
*   **Streams:** Number of streams detected in the playlist.
*   **Availability %:** A measure of how many streams in the playlist are currently considered online or valid (if Threadfin performs such checks).
*   **Group Title %:** Percentage of streams that have a `group-title` attribute in the M3U.
*   **TVG-ID %:** Percentage of streams that have a `tvg-id` attribute (used for EPG mapping).
*   **Unique ID %:** Percentage of streams with a detectable unique identifier (e.g., `CUID`, `channel-id`), which helps in reliably mapping channels even if names change.
*   **Actions:** Buttons to Edit, Delete, or manually Update the playlist.

## Adding a New M3U Playlist

1.  Click the **"New"** or **"Add Playlist"** button.
2.  Select **"M3U"** as the type if prompted.
3.  Fill in the following details:
    *   **Name:** A descriptive name for this playlist (e.g., "My IPTV Provider", "Local Channels").
    *   **Description (Optional):** A brief description.
    *   **M3U File URL or Path:**
        *   **URL:** Enter the full HTTP or HTTPS URL of your M3U playlist (e.g., `http://provider.com/playlist.m3u8?user=...&pass=...`).
        *   **Local Path:** Enter the absolute file path to an M3U file stored on the same system where Threadfin is running (e.g., `/opt/threadfin/playlists/mychannels.m3u`). *Note: Ensure Threadfin has read access to this path, especially in Docker deployments where volume mapping is crucial.*
    *   **Tuner / Streams (Optional, related to buffer settings):**
        *   Some IPTV proxies allow setting a maximum number of concurrent streams that can be pulled from *this specific playlist* if a buffer is enabled. This can prevent overwhelming your provider. Refer to the [Settings](settings.md) section on Streaming/Buffer for more details on global tuner limits.
    *   **Auto Update (Recommended):** Enable this to have Threadfin automatically refresh the playlist based on the schedule defined in Settings.

4.  Click **"Save"**. Threadfin will attempt to fetch and process the playlist.

## Adding an HDHomeRun Tuner

Threadfin can often auto-discover HDHomeRun tuners on your network. If not, or if you want to add one manually:

1.  Click the **"New"** or **"Add Playlist"** button.
2.  Select **"HDHomeRun"** as the type.
3.  Fill in the details:
    *   **Name:** A descriptive name (e.g., "Living Room HDHR", "HDHomeRun Prime").
    *   **Description (Optional):**
    *   **HDHomeRun IP Address:** The IP address of your HDHomeRun tuner (e.g., `192.168.1.50`). Sometimes a port is needed if non-standard (e.g., `192.168.1.50:5004`).
    *   **Tuner / Streams (Optional):** Similar to M3U playlists, this might limit concurrent streams from this specific HDHR device if buffering is involved. HDHomeRun devices have their own physical tuner limits (e.g., 2, 3, 4, or 6 tuners).
    *   **Auto Update:** HDHomeRun channel lineups are generally static but enabling this might refresh the device status.

4.  Click **"Save"**. Threadfin will attempt to connect to the HDHomeRun device.

## Managing Playlists

*   **Edit:** Click the "Edit" button for a playlist to modify its settings.
*   **Delete:** Click the "Delete" button to remove a playlist. This will also remove associated channels from your filters and mapping unless they are sourced from another playlist.
*   **Update Manually:** Click the "Update" button to force an immediate refresh of the selected playlist. Threadfin will re-download the M3U or query the HDHomeRun and re-process its streams. This is useful after making changes on the provider's side or to test connectivity.

## Important M3U Attributes

Threadfin (like xTeVe) relies on specific attributes within your M3U playlist for optimal functionality, especially when using XEPG for EPG management:

*   `#EXTM3U`: Standard M3U header.
*   `#EXTINF:-1`: Header line for each stream, followed by attributes and channel name.
*   `tvg-id="channel.id"`: The Electronic Program Guide (EPG) identifier for the channel. This ID should match a channel ID in your XMLTV file for automatic EPG mapping.
*   `tvg-name="Channel Name"`: An EPG-specific name for the channel. Can be used for mapping if `tvg-id` is missing or as a fallback.
*   `tvg-logo="http://path.to/logo.png"`: URL to the channel's logo.
*   `group-title="News"`: Category or group for the channel. Used for creating Group Filters and organizing channels.
*   **Unique Identifiers (e.g., `cuid`, `channel-id`, `tvg-chno`):** Some M3U playlists include persistent unique IDs for channels. Threadfin attempts to use these to maintain channel settings even if the channel name or order changes in the M3U. If no such ID is found, the channel name is often used as a fallback, which can be less reliable.

**Example M3U Stream Entry:**
```m3u
#EXTINF:-1 tvg-id="news.tv.us" tvg-name="News Channel US" tvg-logo="http://example.com/logos/news_us.png" group-title="USA News",News Channel US HD
http://provider.stream.url/live/news_us/playlist.m3u8
```

Ensure your M3U provider includes these attributes, especially `tvg-id` and `group-title`, for the best experience with Threadfin's filtering and EPG mapping features.
