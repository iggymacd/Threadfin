# Filters

Filters in Threadfin allow you to selectively control which channels from your [Playlists](playlists.md) are made available for [Mapping](channel-mapping.md) and subsequent streaming. This is essential for managing large M3U playlists, removing unwanted channels, or organizing channels into logical groups.

Access this section via the **Filters** item in the main navigation menu.

## Overview

The Filters page will list all the filters you have created. For each filter, you might see:

*   **Filter Name:** The custom name you assigned.
*   **Filter Type:** (e.g., Group Filter, Custom Filter).
*   **Filter Rule/Criteria:** A summary of the filter's rules.
*   **Enabled/Active:** Whether the filter is currently active.
*   **Actions:** Buttons to Edit, Delete, or change the status of the filter.

Channels must pass through at least one active filter to be available in the Mapping section. If a channel doesn't match any active filter's "include" criteria, or if it matches an "exclude" criterion, it will not be processed further.

## Filter Types

Threadfin typically supports two main types of filters:

### 1. Group Filter

*   **Based on M3U `group-title`:** This filter type works with the `group-title` attribute found in your M3U playlists.
*   **Ease of Use:** It's a straightforward way to include or exclude entire groups of channels as defined by your M3U provider.

**Creating/Editing a Group Filter:**

1.  Click **"New"** or **"Add Filter"**, then select "Group Filter" type.
2.  **Filter Name:** A descriptive name (e.g., "UK News Channels", "Exclude Shopping Channels").
3.  **Description (Optional):**
4.  **Group Title Selection:**
    *   Threadfin will typically display a list of all unique `group-title` values found in your processed playlists.
    *   Select the group(s) you want this filter to apply to.
5.  **Include Rules (Channel Name Contains):**
    *   Specify keywords or phrases (comma-separated). If a channel name *within the selected group(s)* contains any of these keywords, it will be **included**.
    *   Leave blank to include all channels from the selected group(s) by default (unless excluded below).
    *   **Case Sensitive:** Option to make the matching case-sensitive.
6.  **Exclude Rules (Channel Name Contains):**
    *   Specify keywords or phrases (comma-separated). If a channel name *within the selected group(s)* contains any of these keywords, it will be **excluded**, even if it matched an include rule.
    *   **Case Sensitive:** Option to make the matching case-sensitive.
7.  **Starting Channel Number (Threadfin Specific):**
    *   **This is an enhanced Threadfin feature.**
    *   You can specify a starting channel number for channels that pass this filter group.
    *   This helps in organizing channel numbering logically before they reach the main mapping stage. For example, all channels from a "UK News" filter group could start numbering from 2000.
    *   If multiple filters match the same channel, the behavior of which starting number takes precedence might depend on filter order or other logic (this needs to be documented based on actual Threadfin behavior).
8.  **Save** the filter.

### 2. Custom Filter (Advanced)

*   **Based on M3U Attributes:** This filter type provides more granular control by allowing you to create rules based on the content of various M3U attributes for each stream (e.g., `tvg-id`, `tvg-name`, channel name itself, parts of the stream URL if supported).
*   **Flexibility:** Powerful for complex filtering scenarios not covered by simple group titles.
*   **Syntax:** Custom filters often use a specific syntax (similar to xTeVe's original custom filter rules).

**Creating/Editing a Custom Filter:**

1.  Click **"New"** or **"Add Filter"**, then select "Custom Filter" type.
2.  **Filter Name:** A descriptive name.
3.  **Description (Optional):**
4.  **Case Sensitive:** Option to make matching case-sensitive for the entire rule.
5.  **Filter Rule:** This is where you define the logic. The rule analyzes a concatenated string of values from each stream's M3U `#EXTINF` line (excluding the stream URL itself).

    **Example M3U Stream Data for Filtering:**
    If an M3U line is:
    `#EXTINF:-1 CUID="1000" tvg-id="news.channel.1" tvg-name="News Channel 1" group-title="UK: News",News Channel 1 HD`
    The string xTeVe/Threadfin might analyze for this stream (excluding URL) could be:
    `1000 news.channel.1 News Channel 1 UK: News News Channel 1 HD`

    **Filter Rule Syntax (Commonly from xTeVe):**
    *   `keyword`: Includes streams containing "keyword".
    *   `{keyword1,keyword2}`: Includes streams containing "keyword1" OR "keyword2".
    *   `!{keyword}`: Excludes streams containing "keyword".
    *   `keyword1 {keyword2}`: Includes streams containing "keyword1" AND "keyword2".
    *   `keyword1 !{keyword3}`: Includes streams containing "keyword1" BUT NOT "keyword3".

    **Examples:**
    *   To include all channels with "HD" in their data:
        ```
        HD
        ```
    *   To include all "News" channels that are also "HD":
        ```
        News {HD}
        ```
    *   To include "Sports" channels but exclude those with "SD" or "French":
        ```
        Sports !{SD,French}
        ```
6.  **Save** the filter.

## Managing Filters

*   **Order of Execution:** The order in which filters are defined might be important, especially if multiple filters could apply to the same channel. Check Threadfin's behavior on how filter precedence is handled (e.g., first match, cumulative effect).
*   **Enable/Disable:** You can typically toggle filters on or off without deleting them, allowing you to test different filter combinations.
*   **Editing and Deleting:** Standard options to modify or remove filters.

## Best Practices

*   **Start Broad, Then Refine:** Begin with broader group filters and use custom filters or more specific include/exclude rules as needed.
*   **Leverage `group-title`:** If your M3U provider uses `group-title` effectively, Group Filters are often the easiest way to manage large channel sets.
*   **Test Your Filters:** After creating or modifying filters, check the [Mapping](channel-mapping.md) section to see if the expected channels are available. You might need to update playlists and then re-check mapping.
*   **Use Descriptive Names:** Give your filters clear, descriptive names so you can easily understand their purpose.
*   **Be Careful with Exclusions:** Overly broad exclusion rules (e.g., `!{a}`) can inadvertently remove all your channels.

By effectively using filters, you can create a clean and manageable list of channels to work with in Threadfin's mapping and EPG features.
