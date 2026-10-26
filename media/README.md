# media/ — explainer reels

Drop the project explainer videos here, then point `PROJECTS` at them.
`PROJECTS` lives in `index.html`, in the page script, under the heading
`15 · the case studies`.

## Expected files

| Project            | id         | Video              | Poster              | Captions           |
| ------------------ | ---------- | ------------------ | ------------------- | ------------------ |
| ML Auto-Retraining | `mlops`    | `mlops.mp4`        | `mlops.webp`        | `mlops.vtt`        |
| E-Commerce Forecast| `forecast` | `forecast.mp4`     | `forecast.webp`     | `forecast.vtt`     |
| GRIIT              | `griit`    | `griit.mp4`        | `griit.webp`        | `griit.vtt`        |
| GoalTrace          | `goaltrace`| `goaltrace.mp4`    | `goaltrace.webp`    | `goaltrace.vtt`    |

Mounted: `mlops`, `griit`, `goaltrace` (no captions yet).

Nothing here is required. Until a `src` is set the player shows a leader
("explainer reel not mounted") and the chapter list still reads, so the case
study is never empty.

## Mounting one

```js
{
  id: 'mlops',
  runtime: '2:14',                 // shown on the card badge
  media: {
    src:      'media/mlops.mp4',
    poster:   'media/mlops.webp',
    captions: 'media/mlops.vtt',   // optional — the CC button hides without it
    chapters: [
      { t: 0,   label: 'The problem: models rot quietly' },
      { t: 28,  label: 'Scoring drift on the GPU' }
    ]
  }
}
```

`t` is seconds from the start. Chapters drive both the ticks on the scrub
rail and the list under the player, and they work with or without a video.

## Encoding

- **Container** mp4 (H.264 + AAC). Safari will not play VP9-in-webm reliably.
- **Resolution** 1920×1080 max; the frame is 16:9.
- **Bitrate** 2–4 Mbps is plenty for screen-capture and motion graphics.
- **Size** keep each clip under ~20 MB or first paint on the study suffers.

```
ffmpeg -i raw.mov -c:v libx264 -crf 23 -preset slow -vf scale=1920:-2 \
       -c:a aac -b:a 128k -movflags +faststart mlops.mp4
```

`+faststart` matters — without it the player cannot begin until the whole
file has arrived.

## Captions

WebVTT. The page renders cues itself into a styled caption line rather than
using the browser's box, so keep lines short (≤ 42 characters) and avoid
positioning cues — they are ignored.

```
WEBVTT

00:00:00.000 --> 00:00:03.200
Models do not fail loudly. They drift.
```
