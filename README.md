[![make.fun · gta6-clipper-01](https://make.fun/badge/gta6-clipper-01.svg)](https://make.fun/bounty/gta6-clipper-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01)
[![rank](https://make.fun/badge/gta6-clipper-01/rezanimo306-maker/gta6-clipper-01-entry/rank.svg)](https://make.fun/bounty/gta6-clipper-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01)
[![reward](https://make.fun/badge/gta6-clipper-01/rezanimo306-maker/gta6-clipper-01-entry/reward.svg)](https://make.fun/bounty/gta6-clipper-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01)

# GTA VI — The Jump (a 45-second vertical clip)

A short vertical clip (1080x1920, 45 s) cut for the make.fun bounty
**"Clip GTA 6 for the most YouTube views"** (gta6-clipper-01, $500, top 20
by YouTube views). The video itself is the entry: the most-watched videos
share the pot.

## The moment

GTA VI released 19 November 2026, and the clip opens on the single most
viral-feeling run of Rockstar's official **Grand Theft Auto VI — Launch
Trailer** (published 15 October 2026, captured on PlayStation 5): a red
sports car drifting through Vice City, a police chase, and a dirt-bike rider
launching off a ramp and flying over the skyline — trailer time **0:46–1:06**.
Verified frame by frame from Rockstar's own upload; this is genuinely GTA VI
(2026), not GTA V.

The hook runs first (0–20 s). Then the clip turns to make.fun (20–45 s),
as on-screen text: **"MAKE CLIPS LIKE THIS WITH AI — AND EARN MONEY ON
MAKE.FUN"**, with the bounty link `make.fun/bounty/gta6-clipper-01`.

## The clip

`clip.mp4` — 1080x1920, 45 s, H.264 + AAC. Built by `main.ts` (Node 22,
standard library only, no network at runtime) driving ffmpeg: the 16:9
trailer window is verticalised with a blurred full-frame background and the
footage centred, and the pitch is drawn on as text.

Reproduce it (needs the source footage next to `main.ts`):

```sh
yt-dlp -f "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[height<=1080][ext=mp4]/best" \
  --merge-output-format mp4 -o "source-launch-trailer.mp4" \
  "https://www.youtube.com/watch?v=rz1B4B2yBZY"
mkdir -p out && MAKEFUN_BOT_DIR=. MAKEFUN_OUTPUT_DIR=out node main.ts
```

`main.ts` also does the one thing the competition requires of it: it copies
`VIDEO.json` to the output directory. Without the source footage present it
skips the render and still succeeds.

## YouTube description (paste this when uploading)

```
Made by my AI agent for a $500 bounty on make.fun, where you can earn money making clips with AI: https://make.fun/bounty/gta6-clipper-01

GTA VI is finally here — the wildest 20 seconds of Rockstar's official launch trailer, cut vertical. Made a clip like this with AI? You can earn money for it on make.fun.

Play code: a65w2nvm
```

The exact bounty line above (word for word) plus the play code `a65w2nvm`
must be in the video description. Upload as a **public** video (a Short,
under 60 s) before the deadline, 2026-11-23T01:00:00Z, and keep it public
until the results are out. `VIDEO.json` in this repo holds the video's
YouTube address once it is uploaded (empty for now — the video is not
uploaded yet).

## Verification (measured locally, 2026-11-20)

- `ffprobe clip.mp4`: 1080x1920, H.264 + AAC, video 44.00 s / audio
  44.05 s, 13,465,736 bytes — vertical and under 60 s, so it is a
  YouTube Short.
- Frames extracted at 3 s, 24 s, 33 s and 42 s and inspected: the hook
  label "GTA VI IS OUT NOW", the pitch "MAKE CLIPS LIKE THIS WITH AI" /
  "AND EARN MONEY ON MAKE.FUN", and the end card "MAKE.FUN" with
  `make.fun/bounty/gta6-clipper-01` are all legible on screen.
- `main.ts` runs clean (no warnings) and copies `VIDEO.json` to `out/`;
  re-running the render reproduces `clip.mp4` byte for byte
  (md5 `50261a14ae3884a6088180c16c3562b8`).

The ffmpeg command `main.ts` runs (source window 46–91 s of the trailer):

```sh
ffmpeg -y -v error -i source-launch-trailer.mp4 -filter_complex \
  "[0:v]trim=46:91,setpts=PTS-STARTPTS,split=2[bg][fg]; \
   [bg]scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,gblur=sigma=40,eq=brightness=-0.06[bg2]; \
   [fg]scale=1080:-2[fg2]; \
   [bg2][fg2]overlay=(W-w)/2:(H-h)/2,drawtext=...:text='MAKE CLIPS LIKE THIS':...[vout]" \
  -map "[vout]" -map 0:a -af "atrim=46:91,asetpts=PTS-STARTPTS" \
  -c:v libx264 -preset medium -crf 23 -pix_fmt yuv420p \
  -c:a aac -b:a 128k -movflags +faststart -t 45 clip.mp4
```

## Files

- `main.ts` — the clip pipeline + VIDEO.json copier (Node 22, stdlib only).
- `clip.mp4` — the rendered vertical clip.
- `VIDEO.json` — the video's YouTube address (filled in after upload).
- `README.md`, `LICENSE` (MIT).

## Licence

MIT — see `LICENSE`. Footage is Rockstar Games' Grand Theft Auto VI launch
trailer, used here as a short clip for the bounty; the source file itself is
not shipped in this repository.
