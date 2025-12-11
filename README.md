## Script Description: Explode Multistream Media File to New one-stream

Add the following repository to [ReaPack](https://reapack.com):

```
https://github.com/ArslanAnwarCom/reaper/raw/main/index.xml
```
This Lua script for **REAPER** (a Digital Audio Workstation) automates the process of breaking a single selected media item containing multiple streams (like a video with video and audio, or a single file with multiple audio channels) into separate media items, each containing one stream, placed on newly created tracks.

---

### **Key Functionality**

The script executes the following steps:

1.  **Identify Multi-stream Items:** It inspects all selected media items in the REAPER project to determine which ones contain multiple streams (e.g., video and audio, or multiple distinct audio tracks) using the external `ffmpeg` tool.
2.  **External Conversion & Extraction:** For every multi-stream item found, it uses `ffmpeg` to:
    * **Extract Streams:** Convert each stream into a new, single-stream file on your system.
    * **Determine Format:** Video streams are saved as **.mp4** and are copied directly (`-c copy`) for quality preservation and speed. Audio streams are saved as **.wav** files.
    * **Unique Naming:** Ensures the new files have unique names based on the original file name and the stream index (e.g., `original_file [stream 1].wav`).
3.  **Track and Folder Creation:**
    * It creates the necessary number of new tracks immediately below the original track to hold the extracted streams.
    * The **original track** is converted into a **folder track** (folder depth set to 1).
    * The **new tracks** are nested as children and named using the parent's name plus the stream index (e.g., "Parent Track Name [stream 1]").
    * The last new track is set to close the folder.
4.  **Item Insertion and Synchronization:**
    * New media items are created on the newly formed child tracks.
    * Each new item references one of the extracted single-stream files.
    * The new items inherit the original item's **position**, **length**, **mute state**, and **take start offset** to maintain synchronization.
5.  **Clean-up:**
    * The original multi-stream media item is **muted** to prevent it from playing alongside the new extracted streams.
    * The script concludes by triggering REAPER's internal command to **build missing peaks** for the new media items, ensuring their waveforms are visible.

---

### **⚠️ Prerequisite**

This script requires the command-line tool **FFmpeg** to be installed and accessible system-wide.

The
```
ffmpeg
```
executable must be resolvable from your system's PATH environment variable, as the script relies on being able to call the command `ffmpeg` directly to inspect media files and perform the stream extraction and conversion.
You can download ffmpeg from https://www.gyan.dev/ffmpeg/builds/#release-builds or open command prompt and type
```
winget install ffmpeg
```
and hit enter. This will install and configure ffmpeg.