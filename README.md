# WebAR Template — how it works

One site, unlimited AR experiences. Each experience ("project") is just a folder with
a target image and a config file. No app install for the person scanning — it opens
in their phone's browser.

## Folder structure

```
webar-template/
  index.html                      <- the only page, never touch this per-project
  projects/
    sample-wedding/                <- one folder = one AR experience
      config.json
      target.mind                  <- YOU add this (compiled target image)
      media.mp4                    <- YOU add this (the video)
    sample-image-audio/
      config.json
      target.mind
      photo.jpg
      song.mp3
    sample-image-only/
      config.json
      target.mind
      photo.jpg
```

Each project is opened with a URL like:
`https://yoursite.netlify.app/?project=sample-wedding`

## Step 1 — Compile your target image

MindAR needs your image (JPG/PNG) converted into a `.mind` file so it can recognize it.

1. Go to: https://hiukim.github.io/mind-ar-js-doc/tools/compile
2. Upload your target image (the wedding invite, poster, photo — whatever guests will scan)
3. Click compile, then download the `.mind` file it gives you
4. Rename it to `target.mind` and drop it in your project's folder

**Good target images:** busy, high-contrast, lots of visual detail (a printed invite with text/design works great).
**Bad target images:** plain colors, a single face close-up, blank backgrounds — nothing for the tracker to grab onto.

## Step 2 — Add your media

- **Video overlay** → put your video file in the folder (keep it short and compressed — under ~15MB loads fast on mobile data). Set `"type": "video"` in config.json, `"media": "yourfile.mp4"`.
- **Photo + music** → put an image + an audio file in the folder. Set `"type": "image-audio"`, `"media": "photo.jpg"`, `"audio": "song.mp3"`.
- **Photo only** → just the image. Set `"type": "image"`, `"media": "photo.jpg"`.

## Step 3 — Create a new project (for a new client/use case)

1. Duplicate any `projects/sample-*` folder
2. Rename it (e.g. `projects/priya-wedding`)
3. Replace `target.mind` + media files with the new ones
4. Edit `config.json` for that project's content type
5. Done — no code changes needed anywhere else

## Step 4 — Deploy

1. Push this whole `webar-template` folder to a GitHub repo
2. Connect the repo to Netlify (New site from Git)
3. Netlify gives you a live URL, e.g. `yourname.netlify.app`
4. Each project is now live at `yourname.netlify.app/?project=<folder-name>`

Important: **the site must be served over HTTPS** for camera access to work — Netlify
gives you this automatically, so this only matters if you ever try running it elsewhere.

## Step 5 — Generate the QR code

Use any free QR generator (e.g. qr-code-generator.com) and point it at your project's
full URL: `https://yourname.netlify.app/?project=priya-wedding`

Print that QR code on the invite/poster/product itself.

## Config reference

| Field         | Type    | Notes                                              |
|---------------|---------|-----------------------------------------------------|
| `targetFile`  | string  | always `"target.mind"`                              |
| `type`        | string  | `"video"` \| `"image-audio"` \| `"image"`            |
| `media`       | string  | video or image filename                             |
| `audio`       | string  | only used with `"image-audio"`                      |
| `width`       | number  | plane width (default 1)                             |
| `height`      | number  | plane height (default 1.33) — match your image's aspect ratio |
| `loop`        | boolean | loop video/audio (default true)                     |
| `pauseOnLost` | boolean | pause video when target leaves frame (default true) |

## Testing before you deploy

You can test locally, but MindAR requires a real HTTPS URL (or `localhost`) for camera
access — opening `index.html` directly as a file (`file://`) will NOT work. Easiest path:
push a test project to Netlify and open it on your phone.
