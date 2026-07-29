# vicciz-demo

The Kiipling product demo page, served at **https://demo.vicciz.com**.

A single static page — no build step, no server. GitHub Pages publishes the
repository root of `main` directly.

## Hosting

Same setup as `AI-for-Lawyers` / lawyers.nerya.io:

| Piece       | Value                                                    |
| ----------- | -------------------------------------------------------- |
| Pages source| branch `main`, path `/`                                   |
| `CNAME`     | `demo.vicciz.com`                                          |
| `.nojekyll` | Stops Jekyll from processing the files                     |
| DNS         | `CNAME demo.vicciz.com → vicciz-ceo.github.io` (Cloud DNS) |

Push to `main` and the site rebuilds. There is no deploy script.

## Files

| File                    | Notes                                                    |
| ----------------------- | -------------------------------------------------------- |
| `index.html`            | The whole page — inline CSS and JS, relative asset paths. |
| `kiipling-demo.mp4`     | 500×1080, 64s, 3.8 MB, H.264 + AAC, `+faststart`.         |
| `kiipling-demo.en.vtt`  | English captions, opt-in via the player's CC control.     |
| `poster.jpg`            | Frame at 0:12 (all three suggestions on screen).          |

Asset paths are relative, so the page also renders correctly at
`vicciz-ceo.github.io/vicciz-demo/` before DNS resolves.

## Replacing the video

The source recording is a portrait phone capture letterboxed inside a 1920×1080
frame. The committed file is cropped to the phone itself, so the player shows
the device rather than two-thirds dark bars:

```bash
ffmpeg -i source.mp4 -vf "crop=500:1080:710:0" \
  -c:v libx264 -profile:v high -preset slow -crf 22 -pix_fmt yuv420p \
  -c:a aac -b:a 128k -movflags +faststart kiipling-demo.mp4
```

Re-measure the crop for a new recording rather than reusing `710`. If the
dimensions change, update `aspect-ratio: 500 / 1080` in `index.html` to match,
or the video will be letterboxed inside the bezel.

`+faststart` matters: it moves the MP4 index to the front of the file so the
browser can start playing before the whole thing downloads.

## Local preview

```bash
python3 -m http.server 8090
```

Then open http://localhost:8090.
