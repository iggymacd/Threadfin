# Architecture Overview

This document provides a high-level overview of Threadfin's architecture. A deeper understanding would require code-level analysis.

## Core Components

Threadfin, being a Go application with a web UI (likely using JavaScript/TypeScript), generally consists of the following logical components:

1.  **Backend (Go):**
    *   **Web Server:** Handles incoming HTTP/HTTPS requests for both the API and the web UI. Standard Go `net/http` package or a web framework like Gin or Echo might be used.
    *   **API Handler:** Processes API requests (e.g., `/api/...`), interacts with other backend services, and returns JSON responses.
    *   **Configuration Management:** Loads, manages, and saves Threadfin's settings (likely from `settings.json` or similar).
    *   **M3U Playlist Parser & Manager:** Fetches, parses, and processes M3U playlists from configured sources. Manages the internal representation of streams.
    *   **XMLTV EPG Parser & Manager:** Fetches, parses, and processes XMLTV files. Manages EPG data and merges data from multiple sources if applicable.
    *   **Filter Engine:** Applies configured filter rules (Group and Custom) to the streams from M3U playlists.
    *   **Channel Mapping Logic:** Manages the mapping between filtered playlist streams and EPG data, including custom channel numbers, names, logos, and backup channels.
    *   **Streaming Proxy/Buffer Engine:**
        *   If buffering is enabled (FFmpeg/VLC): Manages the interaction with FFmpeg/VLC processes, proxies their output to clients.
        *   If no buffer (direct proxy): Passes stream URLs to clients.
        *   Handles concurrent stream limits (tuners).
    *   **M3U & XMLTV Generator:** Generates the `xteve.m3u` and `xteve.xml` output files based on active mapped channels and EPG data.
    *   **HDHomeRun Emulation:** Emulates an HDHomeRun device for discovery by clients like Plex, Emby, and Jellyfin, providing the DVR lineup.
    *   **Authentication Service:** Manages user authentication for UI and API access.
    *   **Task Scheduler:** Handles scheduled tasks like updating playlists and EPG sources.

2.  **Frontend (Web UI - JavaScript/TypeScript):**
    *   Built using HTML, CSS, and JavaScript (likely TypeScript, as indicated by `_ts.js` files and `tsconfig.json` in the repository).
    *   **UI Framework/Library:** Likely uses Bootstrap for styling and layout, as mentioned in the Threadfin README. It might use a minimal JS framework or be vanilla JS/TS for DOM manipulation and API calls.
    *   **API Client:** Makes asynchronous JavaScript calls (e.g., using `fetch` or `XMLHttpRequest`) to the backend API to retrieve data and send commands.
    *   **Dynamic Content Rendering:** Updates the web page dynamically based on data received from the backend (e.g., populating tables, forms, status indicators).
    *   **WebSocket Communication:** The UI might use WebSockets for real-time updates (e.g., live log viewing, progress updates for background tasks).

3.  **Data Storage:**
    *   **Configuration Files:** Primarily JSON files (e.g., `settings.json`, potentially others for mapping data) stored in the configuration directory (`/home/threadfin/conf`).
    *   **In-Memory Storage:** Runtime data like processed EPG information, active stream status, and session data is likely held in memory.
    *   **Temporary Files:** For downloaded M3Us, XMLTVs, image cache, and potentially buffer segments, stored in a temporary directory (`/tmp/threadfin`).

## High-Level Data Flow (Example: Client requests EPG)

1.  **Client (Plex/Emby/Jellyfin):** Sends an HTTP request to Threadfin's XMLTV URL (e.g., `http://threadfin_ip:34400/xmltv/xteve.xml`).
2.  **Threadfin Web Server (Go):** Receives the request.
3.  **XMLTV Generator (Go):**
    *   Retrieves active, mapped channel information.
    *   Accesses the processed and merged EPG data (from XMLTV Manager).
    *   Constructs the XMLTV document according to the EPG data.
4.  **Threadfin Web Server (Go):** Sends the generated XMLTV document back to the client as an HTTP response.

## High-Level Data Flow (Example: Client requests a stream)

1.  **Client (Plex/Emby/Jellyfin):** Requests a channel (e.g., `http://threadfin_ip:34400/stream/channel_id` or similar, based on the M3U provided by Threadfin).
2.  **Threadfin Web Server (Go):** Receives the request.
3.  **Authentication Service (Go):** Checks if authentication is required and if credentials are valid (if applicable).
4.  **Streaming Proxy/Buffer Engine (Go):**
    *   Checks available tuner slots.
    *   Identifies the actual stream URL from the mapped channel data (and any backup URLs).
    *   **If Buffering (FFmpeg/VLC):**
        *   Initiates an FFmpeg/VLC process to fetch the stream from the provider and buffer/remux it.
        *   Proxies the output from FFmpeg/VLC to the client.
    *   **If No Buffer:**
        *   May redirect the client to the original stream URL or directly proxy the bytes without significant buffering.
5.  **Threadfin Web Server (Go):** Transmits the video stream data to the client.

This overview is conceptual. The actual implementation details, specific Go packages used, and frontend component structure would require a more in-depth code review.
