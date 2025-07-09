# XMLTV / EPG Sources

If you've configured Threadfin to use **XEPG** (XMLTV Electronic Program Guide) as its EPG source (see [Initial Configuration](getting-started/initial-configuration.md) and [Settings](settings.md)), this section is where you manage your XMLTV files. These files provide the program guide data for your channels.

Access this section via the **XMLTV** or **EPG** item in the main navigation menu.

## Overview

The XMLTV/EPG page lists all the XMLTV sources you have configured. For each source, you might see:

*   **Name:** The custom name you assigned to this EPG source.
*   **URL/Path:** The source URL or local file path for the XMLTV file.
*   **Status:** Indicates if the XMLTV file was successfully updated, any errors, etc.
*   **Last Update:** Timestamp of the last successful update.
*   **Availability %:** A measure of the XMLTV file's accessibility.
*   **Channels:** Number of unique channel data entries found in this XMLTV file.
*   **Programs:** Total number of program entries in this file.
*   **Actions:** Buttons to Edit, Delete, or manually Update the XMLTV source.

Threadfin will merge data from all active XMLTV sources to create a consolidated EPG. This EPG is then made available via an XMLTV URL that your media clients (Plex, Emby, Jellyfin) can use.

## Adding a New XMLTV Source

1.  Click the **"New"** or **"Add XMLTV"** button.
2.  Fill in the following details:
    *   **Name:** A descriptive name for this XMLTV source (e.g., "My EPG Provider", "Local OTA EPG").
    *   **Description (Optional):**
    *   **XMLTV File URL or Path:**
        *   **URL:** Enter the full HTTP or HTTPS URL of your XMLTV file (e.g., `http://provider.com/guide.xml`, `http://myepg.com/epg.xml.gz`). Some providers offer gzipped XMLTV files (`.xml.gz`), which Threadfin can usually handle.
        *   **Local Path:** Enter the absolute file path to an XMLTV file stored on the same system where Threadfin is running (e.g., `/opt/threadfin/epg/guide.xml`).
            *   *Ensure Threadfin has read access to this path, especially in Docker deployments (use volume mapping).*
    *   **Auto Update (Recommended):** Enable this to have Threadfin automatically refresh the XMLTV file based on the schedule defined in Settings.

3.  Click **"Save"**. Threadfin will attempt to fetch and process the XMLTV file.

## Managing XMLTV Sources

*   **Edit:** Click the "Edit" button for an XMLTV source to modify its settings.
*   **Delete:** Click the "Delete" button to remove an XMLTV source. Channels previously mapped to EPG data from this source will lose their guide data unless re-mapped to another source.
*   **Update Manually:** Click the "Update" button to force an immediate refresh of the selected XMLTV file. Threadfin will re-download and re-process it. This is useful after your EPG provider has updated their data or to test connectivity.

## XMLTV File Format Basics

XMLTV is a standard XML-based format for structuring EPG data. A typical XMLTV file contains:

*   **`<tv>` root element:** Contains all other EPG data.
    *   Attributes like `generator-info-name`, `source-info-name`.
*   **`<channel id="channel.id">` elements:** Define each channel.
    *   The `id` attribute is crucial. This ID should match the `tvg-id` attribute from your M3U playlist for automatic EPG mapping in Threadfin.
    *   `<display-name>Channel Name</display-name>`: The name of the channel. Multiple `display-name` entries can exist for different languages or versions.
    *   `<icon src="http://path.to/logo.png" />`: URL to the channel's logo.
*   **`<programme channel="channel.id" start="YYYYMMDDHHMMSS Z" stop="YYYYMMDDHHMMSS Z">` elements:** Define each program.
    *   The `channel` attribute links the program to a `<channel>` element's `id`.
    *   `start` and `stop` attributes define the program's air time (usually in UTC, indicated by `Z` or a timezone offset like `+0100`).
    *   `<title lang="en">Program Title</title>`
    *   `<desc lang="en">Program Description</desc>`
    *   `<category lang="en">Movie</category>`, `<category lang="en">News</category>`: Program genres.
    *   Other elements like `<episode-num>`, `<icon>` (for program-specific images), `<rating>`, etc.

**Example XMLTV Snippet:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<tv generator-info-name="MyEPGGenerator">
  <channel id="news.tv.us">
    <display-name>News Channel US</display-name>
    <icon src="http://example.com/logos/news_us.png" />
  </channel>
  <channel id="movies.hd.us">
    <display-name>Movies HD US</display-name>
  </channel>

  <programme channel="news.tv.us" start="20231027180000 +0000" stop="20231027190000 +0000">
    <title lang="en">Evening News Report</title>
    <desc lang="en">The latest news from around the world.</desc>
    <category lang="en">News</category>
  </programme>
  <programme channel="movies.hd.us" start="20231027200000 +0000" stop="20231027220000 +0000">
    <title lang="en">Action Movie Premiere</title>
    <desc lang="en">An exciting action-packed film.</desc>
    <category lang="en">Movie</category>
    <category lang="en">Action</category>
  </programme>
</tv>
```

## Tips for EPG Management

*   **Consistent IDs:** The key to successful EPG mapping is consistency between the `tvg-id` in your M3U playlist and the `channel id` in your XMLTV file.
*   **Multiple EPG Sources:** Threadfin can merge data from multiple XMLTV files. If two sources provide data for the same channel ID, Threadfin's behavior for handling conflicts (e.g., which data takes precedence) should be noted (often it's the first one processed or a specific merging logic).
*   **EPG Update Schedule:** Configure a regular update schedule in Threadfin's [Settings](settings.md) to ensure your EPG data remains current. Most EPG providers update their data daily.
*   **File Encoding:** XMLTV files should ideally be UTF-8 encoded.
*   **Large XMLTV Files:** Some XMLTV files can be very large. Ensure Threadfin has enough resources (memory, disk space for temporary processing) to handle them. Check if your EPG provider offers filtered or regional XMLTV files if the full feed is too large.
*   **"xTeVe Dummy" / Threadfin Dummy EPG:** If you have channels for which no EPG data is available in your XMLTV files, Threadfin's [Mapping](channel-mapping.md) section often provides a "Dummy" EPG option. This generates placeholder program information (e.g., "Channel Name - 24/7 Broadcast") so the channel can still be tuned.

By correctly configuring your XMLTV sources, you enable rich program guide information for your channels within Threadfin and your connected media clients.I've drafted the content for the three files in this step:
*   `docs/user-guide/playlists.md`
*   `docs/user-guide/filters.md`
*   `docs/user-guide/xmltv-epg.md`

This content covers managing M3U/HDHomeRun playlists, creating Group and Custom filters (including the Threadfin-specific "starting channel number" for filter groups), and managing XMLTV EPG sources. The information is adapted from typical M3U/XMLTV proxy functionalities and aims to align with Threadfin's features as understood.
