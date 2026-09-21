# Gallery tour video

The Gallery includes the client-supplied tour below the treatment photos. The
portrait player uses native controls, inline mobile playback, a lobby still as
its poster, and `preload="none"` so the full video is not requested on page load.

## Media

- Original: `816 Aesthetic Medspa Tour Video.MOV` (141,866,308 bytes), retained
  outside the repository in Downloads.
- Web copy: `public/videos/816-med-spa-tour.mp4` (23,006,461 bytes / 21.94 MiB).
- Full 76.63-second runtime, 1080 × 1920 portrait resolution, 30 fps.
- H.264 video with the original AAC audio copied without re-encoding.
- Fast-start MP4 puts playback metadata before the video data.
- Poster: `public/images/gallery/med-spa-tour-poster.jpg`, taken at 13 seconds.

The web copy is about 84% smaller and fits the current 25 MiB individual asset
limit for both [Cloudflare Workers](https://developers.cloudflare.com/workers/platform/limits/#static-assets)
and [Cloudflare Pages](https://developers.cloudflare.com/pages/platform/limits/#file-size).
Do not add the original MOV to the deployed assets.

## Reproduce

Run from the project root with FFmpeg installed. These commands refuse to
overwrite existing files; use different output names when comparing new versions.

```sh
ffmpeg -i "$HOME/Downloads/816 Aesthetic Medspa Tour Video.MOV" \
  -map 0:v:0 -map 0:a:0 \
  -c:v libx264 -preset slow -crf 24 -maxrate 2300k -bufsize 4600k \
  -pix_fmt yuv420p -g 60 -c:a copy -map_metadata -1 -movflags +faststart \
  -n public/videos/816-med-spa-tour.mp4

ffmpeg -ss 13 -i "$HOME/Downloads/816 Aesthetic Medspa Tour Video.MOV" \
  -frames:v 1 -vf 'scale=720:-1' -q:v 3 \
  -n public/images/gallery/med-spa-tour-poster.jpg
```

Check the actual output size after any re-encode: every deployed file must remain
below 26,214,400 bytes. The source already contains text overlays; compression
preserves those overlays and cannot correct motion blur present in the source.
