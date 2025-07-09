# Troubleshooting

This guide provides solutions and suggestions for common issues you might encounter while using Threadfin.

## General Troubleshooting Steps

1.  **Check Logs:** The first place to look for errors is Threadfin's log. Access it via the **Log** section in the web UI. Look for `ERROR` or `WARNING` messages around the time the issue occurred. (See [User Guide - Logs](user-guide/logs.md)).
2.  **Restart Threadfin:** Sometimes, a simple restart can resolve temporary glitches.
3.  **Clear Browser Cache:** If you experience UI issues, try clearing your browser's cache and cookies for the Threadfin site.
4.  **Update Threadfin:** Ensure you are running the latest stable version of Threadfin, as your issue might have been fixed in a newer release.
5.  **Check Resource Usage:** Ensure the system running Threadfin has enough CPU, RAM, and disk space. High resource consumption can lead to instability. (See [Enterprise Guide - Deployment at Scale](enterprise-guide/deployment-at-scale.md)).
6.  **Network Connectivity:** Verify network connectivity between:
    *   Threadfin and your M3U/XMLTV sources.
    *   Clients (Plex, Emby, VLC) and Threadfin.
    *   Use tools like `ping`, `curl`, or `traceroute`.

## Common Issues and Solutions

### 1. Cannot Access Threadfin Web UI

*   **Threadfin Not Running:** Ensure the Threadfin process or container is running.
    *   **Binary/Service:** `sudo systemctl status threadfin` (Linux), check Services (Windows).
    *   **Docker:** `docker ps -a` (check if container is running or exited with errors), `docker logs threadfin`.
*   **Incorrect IP Address or Port:** Verify you are using the correct IP address and port for Threadfin (default port is `34400`).
*   **Firewall Blocking Access:** Check host and network firewalls to ensure the Threadfin port is allowed.
*   **Port Conflict:** Another application might be using the same port. Check `netstat -tulnp | grep 34400` (Linux) or `Resource Monitor` (Windows). Change Threadfin's port if necessary (e.g., using `-port` CLI argument or Docker port mapping).
*   **Reverse Proxy Issues:** If using a reverse proxy (Nginx, Apache, etc.), check its configuration and logs. Ensure it's correctly forwarding requests to Threadfin.

### 2. M3U Playlist Issues

*   **Failed to Download Playlist:**
    *   **Incorrect URL:** Double-check the M3U URL in Threadfin's Playlist settings.
    *   **Network Issues:** Threadfin server cannot reach the M3U provider. Test with `curl <M3U_URL>` from the Threadfin server/container.
    *   **Authentication:** If the M3U URL requires authentication (username/password, token), ensure it's correctly entered.
    *   **Provider Blocking:** Some IPTV providers may block requests based on User-Agent or IP address. Try setting a common User-Agent in Threadfin's [Settings](user-guide/settings.md).
*   **No Streams Found / 0 Streams:**
    *   **Invalid M3U Format:** The M3U file might be corrupted or not in a standard format. Try opening it with VLC or another player to verify.
    *   **Filtering Issues:** Your [Filters](user-guide/filters.md) might be too restrictive and excluding all channels. Disable filters temporarily to test.
*   **Channels Missing After Update:**
    *   The provider might have changed channel IDs or names. If `tvg-id` or other unique identifiers are not consistently used by the provider, Threadfin might see them as new channels.
    *   Check filter configurations.

### 3. XMLTV / EPG Issues

*   **Failed to Download XMLTV File:**
    *   Similar to M3U issues: incorrect URL, network problems, authentication, provider blocking. Test with `curl <XMLTV_URL>`.
*   **No EPG Data Showing in Clients (Plex, Emby, Jellyfin):**
    *   **XEPG Mode Not Enabled:** Ensure Threadfin is in XEPG mode (Settings > General Settings > EPG Source).
    *   **No XMLTV Source Configured:** Add your XMLTV URL/path in Threadfin's XMLTV/EPG section.
    *   **Incorrect XMLTV URL in Client:** Ensure your client (Plex/Emby/Jellyfin) is pointing to Threadfin's XMLTV URL (e.g., `http://threadfin_ip:34400/xmltv/xteve.xml`).
    *   **Mapping Issues:** Channels in Threadfin's [Mapping](user-guide/channel-mapping.md) section must be:
        *   Active.
        *   Correctly mapped to an EPG channel from your XMLTV source (or the Dummy EPG).
    *   **EPG Update Not Run:** Manually trigger an XMLTV update in Threadfin, then refresh the guide in your client.
    *   **Client Cache:** Clients often cache EPG data. Try forcing a full guide refresh in the client.
*   **EPG Data Outdated:**
    *   Ensure Threadfin's scheduled EPG updates are running (Settings > Files Settings).
    *   Verify your XMLTV provider is supplying current data.
*   **Mismatched `tvg-id` and XMLTV `channel id`:** For automatic mapping, these IDs must match. Manually map if necessary.

### 4. Streaming Problems (Playback Issues)

*   **Stream Fails to Start / Buffering:**
    *   **Provider Issue:** The source stream itself might be down or unstable. Test the original M3U stream URL in VLC.
    *   **Tuner Limit Reached:** You might be exceeding Threadfin's configured tuner limit (Settings > General Settings) or your provider's concurrent stream limit.
    *   **Buffer Configuration (FFmpeg/VLC):**
        *   Incorrect FFmpeg/VLC path in Threadfin settings.
        *   Problematic FFmpeg/VLC options. Try with default remuxing options first.
        *   FFmpeg/VLC crashing (check Threadfin logs and system logs).
    *   **Network Congestion:** Insufficient bandwidth between Threadfin and the provider, or Threadfin and the client.
    *   **Firewall/Antivirus:** Security software on the Threadfin server might be interfering with streaming.
*   **Video Stutters / Poor Quality:**
    *   Often a source stream issue or network problem.
    *   If using FFmpeg/VLC for buffering, ensure the server has enough CPU/RAM. Transcoding (if accidentally enabled) is resource-intensive.
*   **Backup Channels Not Working:**
    *   Ensure backup channels are correctly configured in the [Mapping](user-guide/channel-mapping.md) editor.
    *   Verify the backup stream URLs themselves are valid and working.

### 5. Plex / Emby / Jellyfin Integration Issues

*   **Threadfin Not Detected as HDHomeRun Tuner:**
    *   Ensure Threadfin is running and accessible on the network from your media server.
    *   SSDP discovery issues: Sometimes network configuration (especially with Docker networking or complex VLANs) can block SSDP broadcasts. Manually enter Threadfin's IP and port (`<threadfin_ip>:34400`) in your media server's DVR setup if discovery fails.
*   **Channels Not Appearing in Media Server Guide:**
    *   Ensure channels are active and mapped in Threadfin.
    *   Verify the media server is using Threadfin's XMLTV URL for EPG (if in XEPG mode).
    *   Trigger a guide refresh in the media server.
*   **Authentication Errors from Media Server:**
    *   If using PMS/M3U/XML authentication in Threadfin, ensure the credentials (username:password@) are correctly entered in the media server's DVR/EPG setup.

## Seeking Further Help

If you can't resolve your issue with this guide:

1.  **Search GitHub Issues:** Check the [Threadfin GitHub Issues](https://github.com/Threadfin/Threadfin/issues) page to see if your problem has already been reported or solved.
2.  **Ask on Discord:** Join the Threadfin Discord server (link usually in the main `README.md`) and ask in the appropriate support channel.
    *   **Provide Details:** When asking for help, include:
        *   Threadfin version.
        *   How you are running Threadfin (Docker, binary, OS).
        *   Relevant snippets from your Threadfin logs.
        *   Steps you've already taken to troubleshoot.
        *   Clear description of the problem and expected behavior.

This will help the community and developers understand your issue and provide better assistance.
