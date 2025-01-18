# Bunny CDN "DRM" Video Downloader

A Python class for downloading Bunny
CDN's "[DRM](https://bunny.net/stream/media-cage-video-content-protection/)" videos
using [yt-dlp](https://github.com/yt-dlp/yt-dlp).

## Requirements

You'll need to install `yt-dlp` and `requests` modules in your Python environment.

```bash
pip install -r requirements.txt
```

> It is also better to have [FFmpeg](https://ffmpeg.org) installed on your system and its executable added to the PATH
> environment variable.

## Usage

To try the script for testing and demonstration, simply paste the iframe embed page URL at the bottom, where indicated
between the quotes, as well as the webpage referer, and run the script.

```bash
python3 b-cdn-drm-vod-dl.py
```

> Embed link structure: [
`https://iframe.mediadelivery.net/embed/{video_library_id}/{video_id}`](https://docs.bunny.net/docs/stream-embedding-videos)

## Expected Result

By default:

* the highest resolution video is to be downloaded. You can change this behavior in the `main_playlist` function located
  under the `prepare_dl` method.
* The video will be downloaded in the `~/Videos/Bunny CDN/` directory. This configuration can be changed by providing
  the `path` argument when instantiating a new `BunnyVideoDRM` object.
* The video file name will be extracted from the embed page. This can be overridden by providing the `name` argument .

> Please note that the video format will be always `mp4`.

## Explanation

The idea is all about simulating what's happening in a browser.

The program runs a sequence of requests tied to
a [session](https://requests.readthedocs.io/en/latest/user/advanced/#session-objects) object (for cookie persistence and
connection pooling) first of which is the embed page request, from which information are extracted such as the video
name.

After that, the download link (an HLS/M3U8 URL) is ready to be fed to `yt-dlp` to download the video segments, decrypt
them (as Bunny CDN's "DRM" videos are encrypted with the AES-128 algorithm), and merge them into a single playable video
file.